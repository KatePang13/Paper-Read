#### 即使是Go, 并发也不是简单的事情

origin: [Even in Go, concurrency is still not easy (with an example)](https://utcc.utoronto.ca/~cks/space/blog/programming/GoConcurrencyStillNotEasy)

这是一篇最近发表的博客，提出了Go并发编程也并不简单这个观点，引发了很激烈的讨论，这里以学习的目的记录一下翻译的内容，感兴趣的可以阅读原文。

---

Go  一个很大的特点是通过语言层面支持[goroutines](https://golangbot.com/goroutines/) ，以此让并发变得简单 。我认为Go简化的仅仅是并发编程的一个方面：使你的代码可以并发的执行，并发之间通过 channel 通信；而并发地做正确的事还是取决于使用者的实现，不幸的是，Go 目前在标准库上仍然没有为**正确地实现标准并发模型** 提供足够的支持。

比如，一个常规的需求是限制并发的数量；你希望可以同时做一系列的任务，同时任务的数量是可以限制的。这就要取决于你基于 goroutines，channels， sync package 的代码实现。这并不像看起来那么简单，很多很多人都会犯错。 正好我今天准备了一个例子。

[Gops](https://github.com/google/gops) 是一个命令行工具，用于列出系统中当前正在运行的Go 进程，其中包含其编译的Go 版本等额外信息，比如你想查看二进制文件是否过时了，是否是要重新编译部署等。

gops 要做的其中一件事就是并发地查看所有的Go进程，但是又不希望同时查看所有的进程，因为那样会[因为文件描述符个数限制而产生问题](https://github.com/google/gops/pull/118)。这是一个**limited Concurrency**的经典案例。

Gops 当前的实现是在 [goprocess.FindAll()](https://github.com/google/gops/blob/6fb0d860e5fa50629405d9e77e255cd32795967e/goprocess/gp.go#L29) ，这里我们对代码做了简化：

> ```go
> func FindAll() []P {
>    pss, err := ps.Processes()
>    [...]
>    found := make(chan P)
>    limitCh := make(chan struct{}, concurrencyProcesses)
> 
>    for _, pr := range pss {
>       limitCh <- struct{}{}
>       pr := pr
>       go func() {
>          defer func() { <-limitCh }()
>          [... get a P with some error checking ...]
>          found <- P
>       }()
>    }
>    [...]
> 
>    var results []P
>    for p := range found {
>       results = append(results, p)
>    }
>    return results
> }
> ```

（原始代码中，这里有一个 WaitGroup， found channel 会在适当的时候关闭 ）

这里的逻辑是很清晰的，是一个标准的模式（比如在Go 101's [Channel Use Cases](https://go101.org/article/channel-use-cases.html)  中有提及）。我们使用一个 buffered channel 来提供 数量有限的 token； 往 channel 里发送一个值相当于取走一个token(当token被拿完时阻塞)，从channel 接收一个值 相当于放回一个token。 我们在启动一个 goroutine  前拿走一个token, 在goroutine 结束时 放回 token。

尽管我们已经知道这里存在一个 [BUG]( https://github.com/google/gops/issues/123) ，当要检测的进程数过多时会出现，但其实这个BUG这并不容易被发现。 

这里面有2个 channel  :   

```go
   found := make(chan P) //用于goroutine 向 main 发送查看到的进程信息
   limitCh := make(chan struct{}, concurrencyProcesses) //用于限制同时执行的 goroutine个数
```

这个BUG的具体分析：

- goroutines只有在 `[found <-]  ` 后，才会 `[limitCh <-]` 来释放Token； 
- 而 main 只有在整个循环结束后，才会开始  `[<-found]` ，**main在for循环里取Token，当没有Token可用时阻塞**。
- 所有当你有很多进程需要查看时，会启动 N个 goroutine，他们会在 尝试 `[found<-]` 时阻塞，而无法执行 `l[imitCh <-]`，而main 在第一个for循环中, 尝试  `[limitCh <-]` ，永远不会执行到  `[<- found]` 

一方面，这是一个不那么容易出现的BUG，只有多种因素同时具备才会产生：

- 如果 `[limitCh <-]`  来获取Token的动作是放在goroutine 而不是 main，就不会产生这个BUG；main 的for循环会把所有的goroutine都启动，其中的大部分goroutine会阻塞，然后  接收found ，以便可以 `[<- limitCh]` 来释放Token，让其他的goroutine 获得运行机会。
- 如果 goroutine 在 `[found <-]` 前 `[<- limitCh]`  来释放 Token，那么BUG就不存在了（由于错误处理，在defer内接收会更加简单和可靠） 。
- 如果 整个 for 循环是放一个 单独的 goroutine, main 代码 就可以运行到  `[<- found]` ,  不会阻塞完成了的 goroutine 释放自己的Token，这种情况下 for 循环阻塞并等待 `[limit <-]`  也就不会有问题了。 

另一方面，这个例子也说明了：在Go中并发编程远没有看起来的那样简单。一个小小的错误也可能导致程序hang住，而所有的临时测试又都能通过。实现正常的并发对开发者还是有些难度的（关于这一点我们可以讨论，但我觉得这是很明显的。）

我相信 编写这部分代码的肯定是优秀的程序员，但是在诸多大神的层层审核的情况下，这个BUG还是产生了。甚至在已经知道它有并发问题的情况下，我还是花了一些时间才弄明白具体的问题。（因为我的gops 突然 hang住了，[Delve](https://github.com/go-delve/delve) 告诉了我是哪里出问题了）

