# Spam campaign detection, analysis, and investigation

**垃圾邮件活动的检测，分析与调查**

origin

https://www.sciencedirect.com/science/article/pii/S1742287615000079#aep-article-footnote-id9

Author links open overlay panel

[SonDinha](https://www.sciencedirect.com/science/article/pii/S1742287615000079#!)[TaherAzeba](https://www.sciencedirect.com/science/article/pii/S1742287615000079#!)[FrancisFortinb](https://www.sciencedirect.com/science/article/pii/S1742287615000079#!)[DjedjigaMouheba](https://www.sciencedirect.com/science/article/pii/S1742287615000079#!)[MouradDebbabia](https://www.sciencedirect.com/science/article/pii/S1742287615000079#!)

**Show more**

https://doi.org/10.1016/j.diin.2015.01.006[Get rights and content](https://s100.copyright.com/AppDispatchServlet?publisherName=ELS&contentID=S1742287615000079&orderBeanReset=true)

Under a Creative Commons [license](https://creativecommons.org/licenses/by-nc-nd/4.0/)

open access

---



## 概要

垃圾邮件已经成为违法犯罪分子在互联网上开展非法活动的主要手段之一，比如贩卖敏感信息，销售假货，散布恶意软件等。人工分析对于如此庞大数量级的垃圾信息已经杯水车薪。而且，现在的技术要么过于复杂，需要依赖大量的数据，要么不够深入，无法供司法调查使用（miss the extraction of vital security insights for forensic purposes）。在本文中，我们介绍一种针对垃圾邮件行为的探测，分析，调查的软件架构。本框架可以做到及时标记垃圾邮件行为，并基于相关的多种信息对垃圾级邮件行为进行Labels(标记分类)和Scores(计分)。本框架为执法人员（https://www.sciencedirect.com/topics/computer-science/law-enforcement-official）提供了一个强大的平台，可以对网络在的犯罪活动进行调查。

## 关键字

Spam   垃圾邮件

Spam campaign    垃圾邮件活动：传播大量类似的邮件

Spam analysis       垃圾邮件分析

Characterization   特征工程

Frequent pattern tree    频繁模式树

## 介绍

电子邮件 无处不在，对电子邮件的滥用也无处不在。垃圾邮件困扰着无数人，大量了占用资源，已然成为邮件系统的巨大负担。根据 Symantec Intelligence 的报告，全球邮件总量中，垃圾邮件占比 71.9%([Symantec Intelligence Report, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib38)).。此外，攻击者可以非常快速的发送无限量的匿名邮件，开展各种活动(宣传假货假药，商业诈骗)，甚至是实施网络犯罪（传播儿童色情，盗取个人信息，推送恶意软件）。因此，垃圾邮件蕴含着无价的网络安全情报，或许可以揭开网络罪犯世界的神秘面纱。

已经有很多使用垃圾邮件数据来检测和调查网络威胁的研究，比如

- [botnets](https://www.sciencedirect.com/topics/computer-science/botnets) ([Zhuang et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib43), [Xie et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib42), [John et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib19), [Pathak et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib32), [Thonnard and Dacier, 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib39), [Stringhini et al., 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib37)) 
-  [phishing attacks](https://www.sciencedirect.com/topics/computer-science/phishing-attack) ([Fette et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib11), [Bergholz et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib2), [Moore et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib29), [Bergholz et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib3))

另外，很多相关数据源的权限开放，也为研究者们带来了新的机遇和挑战。不幸的是，大多数研究者要么只使用单一的数据源，要么只针对一个特定时间段的垃圾邮件做研究([Pitsillidis et al., 2012](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib34))。 更重要的是， spammers 一直在不断的更新武器，刺穿反垃圾系统的盾。垃圾邮件技术已经从简单的程序进化成了相当智能的spamming 软件, 可以从多台机器散布模板生成的垃圾邮件。僵尸网络传播的垃圾邮件通常被组织成大规模的活动，已然是很多网络犯罪活动的主要手段。因此，spam 数据分析是迫切需要的，特别是 spam campaign分析，以供网络犯罪调查。但是数量巨大的垃圾邮件，人工分析是不现实的，所以，网络犯罪调查人员需要自动化的技术和工具来完成这项任务。

在这项研究中，我们旨在阐述垃圾邮件活动检测，分析和调查的方法，我们同时强调了聚合多数据源对提取垃圾邮件活动特征的重要性。在实践上，我们提供可一种框架：

1. 将spam email 分组到 各个 campaign 中。【译者注：发送大量类似的垃圾邮件，将这些邮件归类为一个spam campaign，后文都用英文表示】
2. 使用标签标记spam campaigns, 根据每个campaign相关的条目，生成 Labels ,条目来自 Wikipedia
3. 关联不同的数据源，比如 被动DNS, 恶意软件，WHOIS 和 地理位置信息，更深入地提炼 spam campaign 的特征。
4. 根据一些自定义的标准对垃圾邮件活动进行评分

标识 spam campaigns 是分析 spammer 发件人的关键步骤，原因如下：

1. spam 的数量是巨大的，分析所有的spam 几乎是不可能的。因此，将spam 聚类到campaigns 可以明显的减少需要分析的数据量，同时维护每个campaign的关键特征。
2. 圾邮件通常以特定目的批量发送，通过聚合spams到campaigns，我们可以更深入的提取相关内在特征，帮助调查人员弄清楚 spammers 是怎么混淆和传播 消息的。
3. 标记spam campaigns 和 关联相关的多方数据，可以揭露campaigns 的特征，显著提高调查的效率
4. 给 spam campaigns 评分可以帮助 调查者 专注于那些更危险的campaigns（比如传播恶意软件，钓鱼邮件）。

本文的剩余部分的组织形式：

- Section 2. 介绍相关的研究工作
- Section 3. 讨论现有的 spam campaigns 检测技术 
- Section 4. 介绍软件架构
- Section 5. 展示实验结果
- Section 6. 总结



## 相关工作

参考文献中给出了几种将垃圾邮件聚类的方法：

### 基于url的垃圾邮件聚类

(URL-based spam email clustering)

垃圾邮件根据嵌套的URLs 和 源IP 进行分类。F. Li et al. ([Li and Hsieh, 2006](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib27)) 提出一种基于相同URLs进行垃圾邮件聚类的方法。该方法还将电子邮件内容中提到的金额用作聚类的辅助因素。 [Xie et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib42)提出了AutoRE，一种检测僵尸网络 [botnet](https://www.sciencedirect.com/topics/computer-science/botnets)的框架：通过统计邮件中嵌套的URLs 。这些基于URL的据类方法在性能上和误判上表现优秀，但是 spammer 可以通过 动态IP地址，短URL服务，多URL 等技术规避。

### 使用文本发掘方法的垃圾邮件聚类

(Spam email clustering using text mining methods)

[Zhuang et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib43)  开发了一种通过spam追踪揭露botnets和它们的特征 的技术，通过shingling algorithm ([Broder et al., 1997](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib4)) 对 Spam 进行聚类。通过IP地址之间的关系确定这些spam行为是否来自同一个 botnet。[Qian et al. (2010)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib35),Qian et al.  设计了一种 无监督的在线 spam行为检测，叫做 *SpamCampaignAssassin* (SCA)，并且根据提取的行为签名进行spam过滤。SCA 基于 潜在语义分析 (*[Latent Semantic Analysis](https://www.sciencedirect.com/topics/computer-science/latent-semantic-analysis)* (LSA) ([Landauer et al., 1998](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib24)) ) 来检测行为。但是模板生成和灵活调整的spam会使文本发掘方法失效。

### 通过检索URL指向的页面进行spam聚类

（Spam email clustering using web pages retrieved from embedded URLs）

[Anderson et al. (2007)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib1) 提出了*spamscatter* 技术，使用图像拼接，来判断和聚类诈骗类型的spam。[Konte et al. (2009)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib22)  分析了诈骗托管基础架构，作者通过不同的其他方法[heuristic methods](https://www.sciencedirect.com/topics/computer-science/heuristic-methods) 从url相应的web界面提取诈骗行为。即使根据web内容可以得到较好的结果，但是邮件内容和网页内容的灵活性仍然是一个挑战。更重要的，主动爬取URL页面，可能会提醒spammers 并暴露我方收集数据的垃圾邮件陷阱(spamtraps )。

### 基于 email 内容的 spam 聚类

(Spam email clustering based on email content)

[Wei et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib40)  提出一种spam聚类方法：基于层次聚类算法的，并将组件用加权的边连接起来。作者仅测试了用于广告的垃圾邮件。

 [Calais et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib5) , Calais et al.  提出了基于频率树的spam行为标识技术。通过挑选 子节点数据量显著增加的节点来识别垃圾邮件行为。这个技术的一个优点是：无需事先了解即可自然地检测垃圾邮件的混淆部分。

[Guerra et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib13)), Calais et al. 简要介绍了*Spam Miner*，一个检测和表征垃圾邮件活动的平台。

[Kanich et al. (2009)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib21) 通过对现有的僵尸网络设施的寄生渗透来分析垃圾邮件活动，以衡量垃圾邮件的投递，点击和转化。

 [Haider and Scheffer (2009)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib14) 应用贝叶斯层次聚类来聚合spam信息。作者开发了一个生成模型([generative model](https://www.sciencedirect.com/topics/computer-science/generative-model))，基于输入矢量的转换对二进制矢量进行聚类。

### spam campaigns 检测方法

A straightforward approach for grouping spam emails into campaigns is to calculate the distances between spam emails and then apply a [clustering technique](https://www.sciencedirect.com/topics/computer-science/clustering-technique), which generates clusters of emails that are “close” to each other. The distance between two emails can be measured by computing the similarity of their textual contents, e.g., *w-shingling* and the [Jaccard coefficient](https://www.sciencedirect.com/topics/computer-science/jaccard-coefficient) ([Broder et al., 1997](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib4)), *Context Triggered Piecewise Hashing* (CTPH) ([Kornblum, 2006](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib23)) or *[Locality-Sensitive Hashing](https://www.sciencedirect.com/topics/computer-science/locality-sensitive-hashing)* (LSH) ([Damiani et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib9)). The first metric has been used in many studies ([Zhuang et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib43), [Anderson et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib1), [Gao et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib12)) while the second and the third metrics are originally utilized for spam mitigation ([spamsum, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib36), [Damiani et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib9)). However, one disadvantage of this approach is its non-scalability because of the enormous amount of spam emails to be analyzed. Additionally, the use of templates for content generation and numerous [obfuscation techniques](https://www.sciencedirect.com/topics/computer-science/obfuscation-technique) has remarkably increased the diversity of spam emails in one campaign. For these reasons, clustering techniques based on the textual similarity of spam emails have become ineffective.

一种聚类spam的前向方法是：计算spam emails 之间的距离，然后使用聚类技术，将靠近的放在同一个分组中。2个邮件的距离可以通过计算文本内容的相似性来衡量，比如 *w-shingling* 和 [Jaccard coefficient](https://www.sciencedirect.com/topics/computer-science/jaccard-coefficient) ([Broder et al., 1997](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib4))，上下文触发分段哈希 , 位置敏感哈希。第一种已经再多个研究中被使用，第二 第三种 最初是用于减少spam。但是，这种方法有一个缺点就是拓展性差，因为需要对大量spam进行分析。另外内容生成模板的使用和各种混淆技术可以在同一个spam活动中显著增量spam的多样性。由于这些原因，基于文本相似度的聚类技术已经变得收效甚问。

首先，我们明确一个前提，一个spam活动所发送的spam都是以相同的方式自动发送的，我们要做的是识别这样的活动，所以，这些邮件的目的是一致的，而且具有共同特征。但是，spammer 可能会在标题，内容和URL上使用各种混淆技术。spammer 可以在同一个活动中使用不同的标题，但是这些标题的含义对人类而言是一样的。另一种现象是spam email 包含了书本或者Wikipedia文件中的一段随机的文本，这种文本是由人类写的，因此很难用统计的方式加以识别。Spammer 也会使用 快速通行服务网络 和 随机生成的子域名 来混淆 URL。还有，spammer 经常在URL上插入一段随机字符串来迷惑spam过滤器，或者包含追踪信息来验证目标email列表并监控其进度。

因为混淆可能被应用在一封 spam email的任何地方，我们的 spam活动检测技术必须考虑整个 spam email 的特征。我们从spam email的 header, 文本内容，URL和附件 中提取特征。我们主要的检测方法是基于 频率模式树 （ *[frequent-pattern](https://www.sciencedirect.com/topics/computer-science/frequent-patterns)* tree），这是 频率模式增长算法的第一步。 FP-Tree 法最早在 [Calais et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib5) 被使用。但是，我们的方法在使用的特征 与 用于从先前构建的FP-Tree识别垃圾邮件活动的信号方面有所不同。另外，我们通过采用逐步构建FP-Tree的技术 ([Cheung and Zaiane, 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib6))来增强快速检测垃圾邮件活动的方法。方法的详细说明将在下一节展开。

## 软件框架

框架的主要模块如下图所示：



![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr1.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr1.jpg)Fig. 1. Architecture of our software framework.

**中心数据库**：精心选型以满足 可拓展，灵活，关系型数据的需求。Section 4.1 将详细描述。

**过滤器和特征提取**：解析spam emails, 提取和存储特征到中心数据库，以供后续分析。Section 4.2 将详细描述。

**spam campaign 检测**：从中心数据库获取特征，标识 spam campaign 并存入到 中心数据库。Section 4.3 将详细描述

**spam campaign Characterization**:  深入分析spam campaigns, 以进一步减少分析时长和工作量。我们连接不同的数据源，label 和 score campaigns,  帮助调查人员快速掌握的一个campaign的目的，有针对性的做处理。详细内容请看 Section 4.4

### 中心数据库

为了存储大量的垃圾邮件，我们主要考虑的是诸如  文档数据库 这样的 NoSQL 数据库，而不是传统的关系型数据库。文档数据库以它们的灵活和高可拓展性出名，但是它们大多没有一个像SQL这样强大的查询语言，另外，表示数据关系也是我们的一个必要的需求，我们需要表示spam emails之间的关系，spam campaigns之间的关系，IP 地址和域名之间的关系。这些关系可以用图数据库高效地存储和处理。我们的系统使用的是 OrientDB ([OrientDB, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib31)), 一个开源的NoSQL 数据库。OrientDB具有基于文档的数据库的灵活性以及基于图数据库管理记录间关系的功能，具备 快速，可拓展，支持SQL风格查询 等优点。spam emails 在数据库中以文档的形式存储，不同的属性存在不同的域，比如 header fields,   文本正文，URLs 等。另外，spam campaigns, IP addresses, domain names 和 attachments 也被存储成不同的文档类型。对各每个文档类型，数据库维护一个 hash表来去重，数据库同时存储不同文档之间的关系，比如 spam emails 之间的关系， spam campaigns之间的关系，spams emails和 attachments 之间的关系或者 spam campaigns 和 IP addresses 之间的关系。

### 邮件解析和特征提取 

解析器获取spam emails 作为输入, email中包含 header 和 body。header 有很多个域，每个域都是一个：分割的键值对。body 可以由一个或多个部分。分析所需要的相关特征在这里被提取，主要考虑的特征有：

- **内容类型(Content Type)**: header 中的 Content-Type域描述了email 的MIME类型：text/plain, text/html, application/octet-stream, multipart/alternative, multipart/mixed, multipart/related 和 multipart/report.
- **字符集(Character Set)**: email 的编码格式可以从header 域或者 email正文中获取。编码格式大致表示了email的语言。spammer 很少混淆这个特征，因为如果解析器获取不到编码格式，email就无法被解码，收件人也就看不到email内容.
- **标题(Subject)**: 整个标题被看作一个特征，如果标题使用Q-encoding编码，将被解码成Unicode.
- **Email 布局( Email Layout)** : 
  - 在text/plain 的情况下，布局是字符串“T”, “N” 和 “U”, 代表文本，新的一行 和URL；
  - 在text/html 的情况下，布局是DOM 树的top 三层
  - 在multipart emails 的情况下，我们使用email body的树形结构来组合 布局
- **URL Tokens**:  email的 URL 被分解成各个Token: hostname, path, parameters, 每个token 当作一个特征
- **附件名(Attachment Name(s))**: 每个附件名当作一个特征.

### Spam campaign detection

Our spam campaign detection method is based on the *[Frequent-pattern](https://www.sciencedirect.com/topics/computer-science/frequent-patterns)* tree (FP-Tree) ([Han et al., 2000](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib15), [Han et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib16)). We employ the FP-Tree based on the premise that the more frequent an attribute is, the more it is shared among spam emails. Less frequent attributes usually correspond to the parts that are obfuscated by spammers. Thus, building an FP-Tree from email features allows spam emails to be grouped into campaigns. Furthermore, obfuscated features are discovered naturally within the FP-Tree and therefore exposing the strategies of spammers. Two scans of the dataset are needed to construct the FP-Tree. The cost of inserting a feature vector *fv* into the FP-Tree is O(*[Math Processing Error]|fv|*), where *[Math Processing Error]|fv|* indicates the number of features in *fv*. The FP-Tree usually has a smaller size than the dataset since feature vectors that share similar features are grouped together. Hence, this structure reduces the amount of data that needs to be processed. In the following, we present our FP-Tree-based spam campaign [detection algorithms](https://www.sciencedirect.com/topics/computer-science/detection-algorithm).

### 检测spam campaign 

我们的检测方法是基于FP-Tree(*[Frequent-pattern](https://www.sciencedirect.com/topics/computer-science/frequent-patterns)* tree , ([Han et al., 2000](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib15), [Han et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib16)))，我们使用FP-Tree的前提是，属性出现的越频繁，在spam emails 间共用的就越多。低频的属性通常与spammer做的混淆相关。因此，使用邮件特征构造 FP-Tree 可以将 spam email 分组到相应的 campaigns中；另外，混淆特征在 FP-Tree 中也会被发现，spammers各种混淆的诡计就被识破。构建 FP-Tree需要2次扫描数据集。将特征向量 fv 插入到 FP-Tree的成本是 O(|fv|)，其中， |fv|表示 fv 的特征值。FP-Tree 的大小通常小于数据集，因为共享相似特征的特征向量被分组到一起。因此，这种结构减少了要处理的数据量。接下来，我们将介绍基于 FP-Tree的 spam campaign 检测算法（ [detection algorithms](https://www.sciencedirect.com/topics/computer-science/detection-algorithm)）。

#### 构造 FP-Tree

我们定义2个数据结构: **FPTree** 和 **FPNode**

- *FPTree*  有一个 root 和一个方法
  - root 是一个 值为null 的 FPNode 
  - add 方法 接收一个特征向量 作为唯一的参数

```R
function add(fv)
	n <- root
    for each f in fv do
    	next n <- n.search(f)
        if next n is None then
        	next n <- New FPNode(f)
            Add next n as a child of n
        end if
        n <- next.n
    end for
end function
```

- FPNode 有8个属性和2个方法
  - tree  表示该FPNode 的 FPTree
  - item 表示 特征
  - root  bool, 标识 是否是root
  - leaf   bool, 标识 是否是leaf
  - parent 父节点
  - children 子节点列表   list<FPNodes>  
  - siblings  兄弟节点列表  list<FPNodes>  
  - add(node)  添加一个子节点
  - search(item) 查找本节点是否有item

我们通过创建  FPnode(null) 来构建 FP-Tree, FPnode(null) 是树的root, 然后一个个地添加特征向量。每个特征向量的特征按添加到数据集的时间降序排列。Algorithm 2  全面展示了这个过程

```R
#An empty list structure
Transactions <- () 
# An empty key,value map
items <- {}
for each spam email do
	# An empty list 
	feature_vector <-()
		Add feature to freature_vector
		item{feature} <- items[feature]+1
	end for
    Add spam_email_ID to the end of feature_vector
	Add feature_vector to Transactions
end for
the_fp_tree <- FPTree()
for each transaction in Transactions do
	if {transaction} > 2 then
		# THe content_type at the beginning and the spam_email_ID at the end are omitted
		Sort(descending) the items in translation[1:-1] base on their counts in items
		the_fp_tree(trans)
	end if
end for
```

最后，我们得到 FP-Tree， 每个节点代表一个从 spam emails中提取的特征。叶子节点到根节点的每个路径表示每个spam message的特征向量。从 FP-Tree 我们可以看到，拥有某些共同的特征 的两个 message，共享到根的路径的一部分。我们用这些特征来聚类spam emails，将拥有某些共同特征的spam emails放到同一个campaign中。

#### Spam campaign 标识(Spam campaign identification)

构建 FP-Tree 后，我们通过深度优先遍历提取 spam campaigns。当我们访问每个节点是，我们检查以下条件：

1. 子节点的个数必须大于 **min_num_children**
2. 子节点的平均频数必须大于 **freq_threshold**
3. 从当前节点到root节点的路径上，不能存在没有 **n_obf_features** 中特性 的节点。 
4. 以当前节点为根节点的子树，叶子节点个数必须大于 **min_num_messages**

如果一个节点满足以上所有条件，以该节点根节点的子树，子树上的所有叶子节点属于同一个 spam campaign：

1. 保存该campaign
2. 删除该子树
3. 继续遍历直到所有的节点都被访问。

FP-Free 算法 非常有效，可以很好地识别一个 campaign 中共有的特征和spammer加的混淆特征([Calais et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib5))。但是，这个算法的缺点是只能用在静态数据集中，比较适用于持续长时间的spam campaign。因此，能够掌握早期的spam campaign的相关证据 ，更适用于后期追查前期行为的场景([Pathak et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib32))。我们通过动态构造FP-Tree来提高 FP-Tree spam campaign 识别算法。一收到 spam emails,我们紧接着就提取特征向量并插入到FP_Tree。下面，我们将对这个流程详细介绍。

#### 增量 FP-Tree

自从  FP-Tree 方法 被提出后，很多研究都在致力于提高其功能([Kai-Sang Leung, 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib20)) 和性能([Ong et al., 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib30)) 。另外，也有一些基于 FP-Tree 的增量提取算法被提出 ([Cheung and Zaiane, 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib6), [Leung et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib26))。我们参考了 ([Cheung and Zaiane, 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib6)) 的思路，按如下流程构建增量FP-Tree：

1. 创建一个 null root
2. 每个新的事务被添加到 root 层 的开头
3. 在每一层：
   1. 新事务的items 与 子节点的items进行比较
   2. 如果拥有相同的items,  有更大count的节点将新事务上的这个items合并。所有，该节点的count +1 ；被合并到节点的items 从新事务中删除。如果 后代节点的count > 其祖先的count,则后代节点必须移到其祖先的前面，将 新事务的 item 合并。
   3. 这个流程递归地执行，直到新事务所有的item被比较。
4. 新事物中的所有剩余items 作为新分支添加到最后合并的节点。
5. spam campaign 检测流程 定期执行，以检测新的  spam campaign 或者 合并到已存在的 spam campaign。

### Spam campaign 特征 (Spam campaign characterization)

spam emails 分组到相应的campaigns  已经大大降低了需要分析的数据量。但是，当调查员需要针对一个特定的campaigns做分析时，仍然需要浏览一定数量的邮件来掌握基本信息。所以，spam的摘要信息对于调查员是有价值的。我们使用技术手段来自动给spam campaign 贴上标签，为调查员提供一个总览的视角。我们同时开发了一个打分机制给spam campaigns 作排名，这样调查员就可以专注于更高优先级的的campaign。另外，spam campaign 也可以揭露一些在对邮件单独分析时得不到的内在特征。例如，不同域名，不同IP地址或者不同服务器之间的关系。因此，我们聚合来自多方的数据来帮助调查员来追踪 spammer，比如WHOIS信息，被动DNS数据，geo-location 和 恶意软件数据库。另外，我们使用可视化工具来展示不同 spam emails, spam campaigns，domains, IP addresses 之间的关系。可视化帮助我们 突出 数据集中的 关键信息，比如一个活跃的 spamming IP地址。接下来，我们会详细论述我们的characterization 方法。

#### 关联分析模块

在这个模块，我们使用 来自 权威第三方的 WHOIS 信息 和被动 DNS 信息，来深入分析spam emails中的 域名。WHOIS信息与每个第二级域相关联，而被动DNS数据提供了主机名和IP地址之间的历史关系以及它们的时间戳。另外，我们使用地理位置信息和恶意软件信息来将更多有用的信息添加到spam campaign的IP地址上。IP地址可以从接收的header 域中提取，或者根据 被动DNS数据的信息。

#### Spam campaign 标签 (  Spam campaign labeling)

我们通过标记 spam campaigns 来提供对内容的概览。一开始，我们使用Lau et al. ([Lau et al., 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib25)) 提出的自动标记技术。尽管在某些场景下是有效的，但是因为需要大量的查询Wikipedia，拓展性极差。因此，我们使用基于 *Wikipedia Miner toolkit* ([Milne and Witten, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib28))的方法，在离线数据库存储Wikipedia 的概要信息。我们基于Wikipedia Miner toolkit 的库搭建了一个 *Spam Campaign Labeling Server*，该服务监听一个特定的端口。我们从同一个campaign 中的 spam emails 内容中提取高频词，并提交到 *Spam Campaign Labeling Server*，响应信息中，包含一个该spam campaign的相关topic的列表，我们将这个列表存储到中心数据库。

#### spam campaign 计分 (Spam campaign scoring)

大多数场景中，调查者只需要 探究 被调查者的一个特定的目标。因此，我们提出了一种方法来为每个spam campaign 计分，每个spam campaign的分数由一系列加权的因素累加得到。每个元素都是同一次campaign中的spam emails 可能出现的属性，特征或信号的计数。比如，一个调查者对攻击加拿大的spammers感兴趣，他可能希望重点关注收件人域名是 ".ca" 的 spam campaigns。我们根据推测和加拿大执法人员的建议来实现各种属性值，其中一些信号是加拿大的IP地址，“.ca” 顶级域名 和  加拿大企业的 IP 范围。

## 实验结果

在本节，我们主要展示 spam campaign分析 的成果，并且展示了一些提供给调查人员的可视化材料，展示spam campaign的宏观视角，帮助相关人员更好的理解spammers的行为习惯。

### 数据集

Table1 展示了我们从可信第三方拿到的 spam 数据集的统计数据。Table 2 和 Table 3 展示了  spam email  MIME 类型的分布情况。MIME type 和 字符集 一般很少会做混淆，但是也出现了少数非标准的MIME type。这个有用的信号可以被用来检测用自动化工具生成的 spam emails。准确的  MIME types 分布在 Table4 展示。

Table 1. Statistics of the dataset.

|                       | 1 year      | 1 month |
| --------------------- | ----------- | ------- |
| (Apr. 2012–Mar. 3013) | (Apr. 2013) |         |
| Messages              | 678,095     | 100,339 |
| Unique IP addresses   | 91,370      | 16,055  |
| Unique hostnames      | 321,549     | 74,833  |
| Unique attachments    | 5537        | 393     |

Table 2. Distribution of ‘raw’ MIME types (1 month).

| MIME type                             | Count  | Percentage |
| ------------------------------------- | ------ | ---------- |
| text/html                             | 52,596 | 50.2345%   |
| text/plain                            | 32,977 | 31.49.54%  |
| multipart/alternative                 | 16,270 | 15.5395%   |
| multipart/mixed                       | 2465   | 2.3543%    |
| multipart/related                     | 264    | 0.2521%    |
| multipart/report                      | 60     | 0.0573%    |
| text/html∖n∖tcharset = “windows-1250” | 19     | 0.0181%    |
| text/html∖n∖tcharset = “us-ascii”     | 15     | 0.0143%    |
| text/html∖n∖tcharset = “iso-8859-1”   | 14     | 0.0134%    |
| text/html∖n∖tcharset = “windows-1252” | 13     | 0.0124%    |
| text/html∖n∖tcharset = “iso-8859-2”   | 8      | 0.0076%    |
| application/octet-stream              | 1      | 0.0010%    |

Table 3. Distribution of ’raw’ MIME types (1 year).

| MIME type                              | Count   | Percentage |
| -------------------------------------- | ------- | ---------- |
| text/plain                             | 353,774 | 52.1431%   |
| text/html                              | 213,578 | 31.4795%   |
| multipart/alternative                  | 73,436  | 10.8238%   |
| multipart/mixed                        | 32,797  | 4.8340%    |
| multipart/related                      | 2,134   | 0.3145%    |
| multipart/report                       | 1,878   | 0.2768%    |
| text/html∖n∖tcharset = “windows-1252”  | 131     | 0.0193%    |
| text/html∖n∖tcharset = “windows-1250”  | 127     | 0.0187%    |
| text/html∖n∖tcharset = “iso-8859-2”    | 126     | 0.0186%    |
| text/html∖n∖tcharset = “us-ascii”      | 125     | 0.0184%    |
| text/html∖n∖tcharset = “iso-8859-1”    | 108     | 0.0159%    |
| application/octet-stream               | 75      | 0.0111%    |
| text/plain charset = utf-8             | 35      | 0.0052%    |
| text/plain charset = us-ascii          | 29      | 0.0043%    |
| text/plain charset = “us-ascii”        | 28      | 0.0041%    |
| text/plain charset = “utf-8”           | 26      | 0.0038%    |
| text/plain charset = “iso-8859-1”      | 25      | 0.0037%    |
| text/plain charset = iso-8859-1        | 25      | 0.0037%    |
| text/plain∖n∖tcharset = “windows-1252” | 4       | 0.0006%    |
| text/plain∖n∖tcharset = “us-ascii”     | 3       | 0.0004%    |
| text/plain∖n∖tcharset = “iso-8859-2”   | 1       | 0.0001%    |
| text/plain∖n∖tcharset = “iso-8859-1”   | 1       | 0.0001%    |
| text/plain∖n∖tcharset = “windows-1250” | 1       | 0.0001%    |

Table 4. Distribution of the correct MIME types.

| MIME type                | 1 year  | 1 month  |        |          |
| ------------------------ | ------- | -------- | ------ | -------- |
| text/html                | 353,952 | 52.1694% | 52,665 | 50.3004% |
| text/plain               | 214,195 | 31.5704% | 32,977 | 31.4954% |
| multipart/alternative    | 73,436  | 10.8238% | 16,270 | 15.5395% |
| multipart/mixed          | 32,797  | 4.8340%  | 2465   | 2.3543%  |
| multipart/related        | 2134    | 0.3145%  | 264    | 0.2521%  |
| multipart/report         | 1878    | 0.2768%  | 60     | 0.0573%  |
| application/octet-stream | 75      | 0.0111%  | 1      | 0.0010%  |

### Spam campaign detection

图2 展示了 一个完整 的 FP-Tree，它是由一天的 spam数据生成的。  ([Data-Driven Documents](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib10)) ([CodeFlower Source code visualization](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib7)). 这个图片清晰地展示了 节点的分组情况，证实了 spam emails 是可以用 FP-Tree 分组成  spam campaigns 的。节点  “*TTTTTNN*”  代表 emails 的局部，我们可以清晰的看到每个campaign下的所有spam emails，它们可能有相同的MIME type (“*text/plain*”)，相同的字符集 (“*windows-1250*”)  或者相同的布局  (“*TTTTTNN*”)。 虽然它们的标题不一样，但是都是表达一样的意思。

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr2.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr2.jpg)Fig. 2. Visualization of the FP-Tree constructed from a one-day spam data.

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr3.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr3.jpg)Fig. 3. Visualization of one part of the [frequent-pattern](https://www.sciencedirect.com/topics/computer-science/frequent-patterns) tree.

Table5 展示了从数据集中提炼 campaigns 需要的时间花销。该程序在一个  Intel® Xeon® X5660 (2.80 GHz) CPU 和 32 GB 内存的机器上运行。最耗时的任务是解析邮件并插入到主数据库中（一年的数据大约需要3小时，一个月的数据大约需要40分钟）。其他的步骤花费时间很少，一年的数据只需要5分钟，一个月的数据只需要30秒。我们使用一年的数据来展示 FP-Tree 方法的效率。接下来，我们将用一个月的数据来展示得到的结果。

Table 5. Processing time.

|                         | 1 year    | 1 month  |
| ----------------------- | --------- | -------- |
| Parsing time            | 10885.2 s | 2467.3 s |
| Feature extracting time | 204.6 s   | 78.0 s   |
| FP-Tree building time   | 36.7 s    | 7.8 s    |
| Campaign detecting time | 35.5 s    | 5.4 s    |

#### FP-tree features

Table6 展示了我们用来标识spam campaigns的主要特征的分布情况，另外，我们做了一个更深入的分析， 研究决定因素和我们的spam campaign 检测算法之间的关系。流程如下所示：

- 首先，*min_num_messages*  参数固定为5
- 在每一步中，*min_num_children*  参数自增1（2 到 20）。记录检测到的spam campaigns 数量与 决定性特征的关系，总共记录8次（当 *freq_threshold*  值为 1.5, 2.0, 2.5, 3.0, 3.5, 4.0, 4.5, 5.0 时）。
  - *freq_threshold*  = 1.5 意味着着 子节点的平均频数 >= 当前节点频数的1.5倍。



Table 6. Distribution of the decisive features used to identify spam campaigns.

| Feature       | 1 year | 1 month |
| ------------- | ------ | ------- |
| Layout        | 2,778  | 641     |
| Hostname      | 1,036  | 190     |
| Subject       | 1,160  | 134     |
| URL           | 602    | 106     |
| Attachment    | 108    | 15      |
| Character set | 3      | 2       |

The results of this analysis are shown in [Fig. 4](https://www.sciencedirect.com/science/article/pii/S1742287615000079#fig4).

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr4.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr4.jpg)Fig. 4. Relation between the FP-Tree features and the algorithm's parameters.

#### Text similarity

当spam campaigns 被 FP-Tree 标识之后，我们计算每个campaigns 的文本相似性值，我们使用三种衡量算法来生成每一个campaign中 所有 email 对的相似值：

*w-shingling* and the [Jaccard coefficient](https://www.sciencedirect.com/topics/computer-science/jaccard-coefficient) ([Broder et al., 1997](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib4))

*Context Triggered Piecewise Hashing* (CTPH)， ([Kornblum, 2006](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib23))

 *[Locality-Sensitive Hashing](https://www.sciencedirect.com/topics/computer-science/locality-sensitive-hashing)* (LSH) ， ([Damiani et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib9))

一个spam campaign有三个相似值，分别时每种方法的平均值。图5展示了处理的结果。纵坐标代表 每个指标的score，横坐标表示 按 score 排列的campaigns，Table 7 是对结果的汇总

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr5.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr5.jpg)Fig. 5. Spam campaign text similarity scores.

Table 7. Summary of campaign text similarity scores.

|         | w-s  |        | CTPH |        | LSH  |        |
| ------- | ---- | ------ | ---- | ------ | ---- | ------ |
| >=50%   | 598  | 54.96% | 595  | 54.69% | 859  | 78.95% |
| 100%    | 265  | 24.36% | 206  | 18.93% | 260  | 23.90% |
| 0%      | 20   | 1.84%  | 46   | 4.23%  | 0    | 0.00%  |
| No text | 10   | 0.92%  | 10   | 0.92%  | 10   | 0.92%  |

计算邮件间的文本相似度是非常耗时的，这三种方法分别花了 13,339.6 s (around 3 h 42 min), 3942.5 s (around 65 min) and 584,486 s (nearly one week) 。

### Spam campaign characterization

在本节，我们展示了一些spam campaigns的特征，这些spam campaigns的数据源是一个月的数据。图6展示了 spam campaigns 和其 spam messages，相关IP地址，相关域名。我们挑选了score最高的和少于1000封邮件的 top5  spam campaigns (图中用大数字标出)。score 值的计算方法是基于我们介绍过的 spam campaign scoring 方法。图7 是一个 词条云，展示了所有的 spam campaigns，每个 campaign 用 其topic 标出。

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr6.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr6.jpg)Fig. 6. Five spam campaigns that have the highest scores and less than 1000 emails

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr7.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr7.jpg)Fig. 7. Topics of spam campaigns.

## Conclusion

spam campaigns 是很多互联网犯罪活动的主要手段。所以，spam campaigns 分析是很多网络安全执法人员的关键任务。在本论文中，我们展示了一个用于 spam campaigns 检测，分析和调查的软件架构。该架构为执法人员提供了一个调查网络犯罪的平台，该系统通过将spam emails聚合到campaign，最大限度地降低调查人员的工作量。另外，它也努力避免给执法人员展示大量的spam campaign信息。 我们还通过标记 spam campaigns 和 关联不同的数据源，提炼出 spam campaign 的各种特征。另外，我们使用多功能和可拓展的数据库处理大量的spam emails,  使用计分机制来突出那些严重的spam campaigns，使用可视化工具来更好的辅助调查人员。我们的系统已经被一个政府组织采纳，被执法人员用以追查spammers，打掉了很多spamming 服务器，减少了垃圾邮件的数量。

## References

- [Anderson et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib1)

  D.S. Anderson, C. Fleizach, S. Savage, G.M. Voelker**Spamscatter: characterizing internet scam hosting infrastructure**Proceedings of 16th USENIX security symposium, SS'07 (2007)pp. 10:1–10:14[Google Scholar](https://scholar.google.com/scholar_lookup?title=Spamscatter%3A characterizing internet scam hosting infrastructure&publication_year=2007&author=D.S. Anderson&author=C. Fleizach&author=S. Savage&author=G.M. Voelker)

- [Bergholz et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib2)

  A. Bergholz, J.H. Chang, G. Paaß, F. Reichartz, S. Strobel**Improved phishing detection using model-based features**CEAS (2008)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Improved phishing detection using model-based features&publication_year=2008&author=A. Bergholz&author=J.H. Chang&author=G. Paaß&author=F. Reichartz&author=S. Strobel)

- [Bergholz et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib3)

  A. Bergholz, J. De Beer, S. Glahn, M.-F. Moens, G. Paaß, S. Strobel**New filtering approaches for phishing email**J Comput Secur, 18 (1) (2010), pp. 7-35[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-71749098791&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=New filtering approaches for phishing email&publication_year=2010&author=A. Bergholz&author=J. De Beer&author=S. Glahn&author=M.-F. Moens&author=G. Paaß&author=S. Strobel)

- [Broder et al., 1997](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib4)

  A.Z. Broder, S.C. Glassman, M.S. Manasse, G. Zweig**Syntactic clustering of the web**Comput Netw ISDN Syst, 29 (8) (1997), pp. 1157-1166[Article](https://www.sciencedirect.com/science/article/pii/S0169755297000317)[Download PDF](https://www.sciencedirect.com/science/article/pii/S0169755297000317/pdf?md5=0a24c3c880cb996321cdfad936d3195b&pid=1-s2.0-S0169755297000317-main.pdf)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-0010362121&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Syntactic clustering of the web&publication_year=1997&author=A.Z. Broder&author=S.C. Glassman&author=M.S. Manasse&author=G. Zweig)

- [Calais et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib5)

  P. Calais, D.E. Pires, D.O.G. Neto, W. Meira Jr., C. Hoepers, K. Steding-Jessen**A campaign-based characterization of spamming strategies**CEAS (2008)[Google Scholar](https://scholar.google.com/scholar_lookup?title=A campaign-based characterization of spamming strategies&publication_year=2008&author=P. Calais&author=D.E. Pires&author=D.O.G. Neto&author=W. Meira Jr.&author=C. Hoepers&author=K. Steding-Jessen)

- [Cheung and Zaiane, 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib6)

  W. Cheung, O.R. Zaiane**Incremental mining of frequent patterns without candidate generation or support constraint**Database engineering and applications symposium, 2003. Proceedings. Seventh international, IEEE (2003), pp. 111-116[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-84879079572&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Incremental mining of frequent patterns without candidate generation or support constraint&publication_year=2003&author=W. Cheung&author=O.R. Zaiane)

- [CodeFlower,](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib7)

  CodeFlower Source code visualizationhttp://redotheweb.com/CodeFlower/

- [Daigle, 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib8)

  L. Daigle**WHOIS protocol specification**Internet RFC, 2070-1721, 3912 (2004)[Google Scholar](https://scholar.google.com/scholar_lookup?title=WHOIS protocol specification&publication_year=2004&author=L. Daigle)

- [Damiani et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib9)

  E. Damiani, S.D.C. di Vimercati, S. Paraboschi, P. Samarati**An open digest-based technique for spam detection**ISCA PDCS (2004), pp. 559-564[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-33845952356&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=An open digest-based technique for spam detection&publication_year=2004&author=E. Damiani&author=S.D.C. di Vimercati&author=S. Paraboschi&author=P. Samarati)

- [Data-Driven Documents](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib10)

  Data-Driven Documents, http://d3js.org/.[Google Scholar](https://scholar.google.com/scholar?q=Data-Driven Documents, http:d3js.org.)

- [Fette et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib11)

  I. Fette, N. Sadeh, A. Tomasic**Learning to detect phishing emails**Proceedings of the 16th international conference on world wide Web, ACM (2007), pp. 649-656[CrossRef](https://doi.org/10.1145/1242572.1242660)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-35348913799&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Learning to detect phishing emails&publication_year=2007&author=I. Fette&author=N. Sadeh&author=A. Tomasic)

- [Gao et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib12)

  H. Gao, J. Hu, C. Wilson, Z. Li, Y. Chen, B.Y. Zhao**Detecting and characterizing social spam campaigns**Proceedings of the 10th ACM SIGCOMM conference on internet measurement, ACM (2010), pp. 35-47[CrossRef](https://doi.org/10.1145/1879141.1879147)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-78650880229&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Detecting and characterizing social spam campaigns&publication_year=2010&author=H. Gao&author=J. Hu&author=C. Wilson&author=Z. Li&author=Y. Chen&author=B.Y. Zhao)

- [Guerra et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib13)

  P. Guerra, D. Pires, D. Guedes, W. Meira Jr., C. Hoepers, K. Steding-Jessen**Spam miner: a platform for detecting and characterizing spam campaigns**Proc. 6th Conf. Email Anti-Spam (2008)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Spam miner%3A a platform for detecting and characterizing spam campaigns&publication_year=2008&author=P. Guerra&author=D. Pires&author=D. Guedes&author=W. Meira Jr.&author=C. Hoepers&author=K. Steding-Jessen)

- [Haider and Scheffer, 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib14)

  P. Haider, T. Scheffer**Bayesian clustering for email campaign detection**Proceedings of the 26th annual international conference on machine learning, ACM (2009), pp. 385-392[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-71149085753&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Bayesian clustering for email campaign detection&publication_year=2009&author=P. Haider&author=T. Scheffer)

- [Han et al., 2000](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib15)

  J. Han, J. Pei, Y. Yin**Mining frequent patterns without candidate generation**ACM SIGMOD Rec, 29 (2) (2000), pp. 1-12[CrossRef](https://doi.org/10.1145/335191.335372)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-0005378038&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Mining frequent patterns without candidate generation&publication_year=2000&author=J. Han&author=J. Pei&author=Y. Yin)

- [Han et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib16)

  J. Han, J. Pei, Y. Yin, R. Mao**Mining frequent patterns without candidate generation: a frequent-pattern tree approach**Data Min Knowl Discov, 8 (1) (2004), pp. 53-87[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-2442449952&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Mining frequent patterns without candidate generation%3A a frequent-pattern tree approach&publication_year=2004&author=J. Han&author=J. Pei&author=Y. Yin&author=R. Mao)

- [Harrenstien et al., 1985](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib17)

  K. Harrenstien, M. Stahl, E. Feinler**WHOIS protocol specification**Internet RFC, 2070-1721, 954 (1985)[Google Scholar](https://scholar.google.com/scholar_lookup?title=WHOIS protocol specification&publication_year=1985&author=K. Harrenstien&author=M. Stahl&author=E. Feinler)

- [Heller and Ghahramani, 2005](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib18)

  K.A. Heller, Z. Ghahramani**Bayesian hierarchical clustering**Proceedings of the 22nd international conference on machine learning, ACM (2005), pp. 297-304[CrossRef](https://doi.org/10.1145/1102351.1102389)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-31844448530&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Bayesian hierarchical clustering&publication_year=2005&author=K.A. Heller&author=Z. Ghahramani)

- [John et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib19)

  J.P. John, A. Moshchuk, S.D. Gribble, A. Krishnamurthy**Studying spamming botnets using botlab**NSDI, vol. 9 (2009), pp. 291-306[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-78649292639&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Studying spamming botnets using botlab&publication_year=2009&author=J.P. John&author=A. Moshchuk&author=S.D. Gribble&author=A. Krishnamurthy)

- [Kai-Sang Leung, 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib20)

  C. Kai-Sang Leung**Interactive constrained frequent-pattern mining system**Database engineering and applications symposium, 2004. IDEAS'04. Proceedings. International, IEEE (2004), pp. 49-58[Google Scholar](https://scholar.google.com/scholar_lookup?title=Interactive constrained frequent-pattern mining system&publication_year=2004&author=C. Kai-Sang Leung)

- [Kanich et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib21)

  C. Kanich, C. Kreibich, K. Levchenko, B. Enright, G.M. Voelker, V. Paxson, *et al.***Spamalytics: an empirical analysis of spam marketing conversion**Commun ACM, 52 (9) (2009), pp. 99-107[CrossRef](https://doi.org/10.1145/1562164.1562190)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-70149106104&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Spamalytics%3A an empirical analysis of spam marketing conversion&publication_year=2009&author=C. Kanich&author=C. Kreibich&author=K. Levchenko&author=B. Enright&author=G.M. Voelker&author=V. Paxson)

- [Konte et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib22)

  M. Konte, N. Feamster, J. Jung**Dynamics of online scam hosting infrastructure**Passive and active network measurement, Springer (2009), pp. 219-228[CrossRef](https://doi.org/10.1007/978-3-642-00975-4_22)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-67649946711&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Dynamics of online scam hosting infrastructure&publication_year=2009&author=M. Konte&author=N. Feamster&author=J. Jung)

- [Kornblum, 2006](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib23)

  J. Kornblum**Identifying almost identical files using context triggered piecewise hashing**Digit Investig, 3 (2006), pp. 91-97[Article](https://www.sciencedirect.com/science/article/pii/S1742287606000764)[Download PDF](https://www.sciencedirect.com/science/article/pii/S1742287606000764/pdfft?md5=6dbc5f184b66b60c3e245952108b03a7&pid=1-s2.0-S1742287606000764-main.pdf)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-33746191665&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Identifying almost identical files using context triggered piecewise hashing&publication_year=2006&author=J. Kornblum)

- [Landauer et al., 1998](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib24)

  T.K. Landauer, P.W. Foltz, D. Laham**An introduction to latent semantic analysis**Discourse Process, 25 (2–3) (1998), pp. 259-284[CrossRef](https://doi.org/10.1080/01638539809545028)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-35048842682&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=An introduction to latent semantic analysis&publication_year=1998&author=T.K. Landauer&author=P.W. Foltz&author=D. Laham)

- [Lau et al., 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib25)

  J.H. Lau, K. Grieser, D. Newman, T. Baldwin**Automatic labelling of topic models**ACL, 2011 (2011), pp. 1536-1545[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-84859022927&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Automatic labelling of topic models&publication_year=2011&author=J.H. Lau&author=K. Grieser&author=D. Newman&author=T. Baldwin)

- [Leung et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib26)

  C.K.-S. Leung, Q.I. Khan, Z. Li, T. Hoque**Cantree: a canonical-order tree for incremental frequent-pattern mining**Knowl Inform. Syst, 11 (3) (2007), pp. 287-311[CrossRef](https://doi.org/10.1007/s10115-006-0032-8)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-34147173382&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Cantree%3A a canonical-order tree for incremental frequent-pattern mining&publication_year=2007&author=C.K.-S. Leung&author=Q.I. Khan&author=Z. Li&author=T. Hoque)

- [Li and Hsieh, 2006](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib27)

  F. Li, M.-H. Hsieh**An empirical study of clustering behavior of spammers and group-based anti-spam strategies**CEAS (2006)[Google Scholar](https://scholar.google.com/scholar_lookup?title=An empirical study of clustering behavior of spammers and group-based anti-spam strategies&publication_year=2006&author=F. Li&author=M.-H. Hsieh)

- [Milne and Witten, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib28)

  D. Milne, I.H. Witten**An open-source toolkit for mining wikipedia**Artificial Intelligence (2013)[Google Scholar](https://scholar.google.com/scholar_lookup?title=An open-source toolkit for mining wikipedia&publication_year=2013&author=D. Milne&author=I.H. Witten)

- [Moore et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib29)

  T. Moore, R. Clayton, H. Stern**Temporal correlations between spam and phishing websites**Proc. of 2nd USENIX LEET (2009)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Temporal correlations between spam and phishing websites&publication_year=2009&author=T. Moore&author=R. Clayton&author=H. Stern)

- [Ong et al., 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib30)

  K.-L. Ong, W.-K. Ng, E.-P. Lim**Fssm: fast construction of the optimized segment support map**Data warehousing and knowledge discovery, Springer (2003), pp. 257-266[CrossRef](https://doi.org/10.1007/978-3-540-45228-7_26)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-35248817442&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Fssm%3A fast construction of the optimized segment support map&publication_year=2003&author=K.-L. Ong&author=W.-K. Ng&author=E.-P. Lim)

- [OrientDB, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib31)

  OrientDB, http://www.orientdb.org/, last accessed in August 2013.[Google Scholar](https://scholar.google.com/scholar?q=OrientDB, http:www.orientdb.org, last accessed in August 2013.)

- [Pathak et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib32)

  A. Pathak, F. Qian, Y.C. Hu, Z.M. Mao, S. Ranjan**Botnet spam campaigns can be long lasting: evidence, implications, and analysis**Proceedings of the eleventh international joint conference on measurement and modeling of computer systems, ACM (2009), pp. 13-24[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-70449652587&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Botnet spam campaigns can be long lasting%3A evidence%2C implications%2C and analysis&publication_year=2009&author=A. Pathak&author=F. Qian&author=Y.C. Hu&author=Z.M. Mao&author=S. Ranjan)

- [Pitsillidis et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib33)

  A. Pitsillidis, K. Levchenko, C. Kreibich, C. Kanich, G.M. Voelker, V. Paxson, *et al.***Botnet judo: fighting spam with itself**NDSS (2010)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Botnet judo%3A fighting spam with itself&publication_year=2010&author=A. Pitsillidis&author=K. Levchenko&author=C. Kreibich&author=C. Kanich&author=G.M. Voelker&author=V. Paxson)

- [Pitsillidis et al., 2012](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib34)

  A. Pitsillidis, C. Kanich, G.M. Voelker, K. Levchenko, S. Savage**Taster's choice: a comparative analysis of spam feeds**Proceedings of the 2012 ACM conference on internet measurement conference, IMC'12 (2012), pp. 427-440[CrossRef](https://doi.org/10.1145/2398776.2398821)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-84870882181&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar?q=Tasters choice: a comparative analysis of spam feeds)

- [Qian et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib35)

  F. Qian, A. Pathak, Y.C. Hu, Z.M. Mao, Y. Xie**A case for unsupervised-learning-based spam filtering**ACM SIGMETRICS Perform Eval Rev, 38 (1) (2010), pp. 367-368[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-77954913096&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=A case for unsupervised-learning-based spam filtering&publication_year=2010&author=F. Qian&author=A. Pathak&author=Y.C. Hu&author=Z.M. Mao&author=Y. Xie)

- [spamsum, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib36)

  spamsum, http://www.samba.org/ftp/unpacked/junkcode/spamsum/README, (last accessed in August 2013).[Google Scholar](https://scholar.google.com/scholar?q=spamsum, http:www.samba.orgftpunpackedjunkcodespamsumREADME, .)

- [Stringhini et al., 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib37)

  G. Stringhini, T. Holz, B. Stone-Gross, C. Kruegel, G. Vigna**Botmagnifier: locating spambots on the internet**USENIX security symposium (2011)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Botmagnifier%3A locating spambots on the internet&publication_year=2011&author=G. Stringhini&author=T. Holz&author=B. Stone-Gross&author=C. Kruegel&author=G. Vigna)

- [Symantec Intelligence Report, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib38)

  Symantec Intelligence Report**Report**(May 2013)http://www.symantec.com/content/en/us/enterprise/other_resources/b-intelligence_report_05-2013.en-us.pdf[Google Scholar](https://scholar.google.com/scholar?q=Report)

- [Thonnard and Dacier, 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib39)

  O. Thonnard, M. Dacier**A strategic analysis of spam botnets operations, in: proceedings of the 8th annual Collaboration**Electronic messaging, anti-abuse and spam conference, ACM (2011), pp. 162-171[CrossRef](https://doi.org/10.1145/2030376.2030395)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-80053642264&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=A strategic analysis of spam botnets operations%2C in%3A proceedings of the 8th annual Collaboration&publication_year=2011&author=O. Thonnard&author=M. Dacier)

- [Wei et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib40)

  C. Wei, A. Sprague, G. Warner, A. Skjellum**Mining spam email to identify common origins for forensic application**Proceedings of the 2008 ACM symposium on applied computing, ACM (2008), pp. 1433-1437[CrossRef](https://doi.org/10.1145/1363686.1364019)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-56749156896&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Mining spam email to identify common origins for forensic application&publication_year=2008&author=C. Wei&author=A. Sprague&author=G. Warner&author=A. Skjellum)

- [Wei et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib41)

  C. Wei, A. Sprague, G. Warner, A. Skjellum**Characterization of spam advertised website hosting strategy**Sixth conference on email and anti-spam, Mountain View, CA (2009)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Characterization of spam advertised website hosting strategy&publication_year=2009&author=C. Wei&author=A. Sprague&author=G. Warner&author=A. Skjellum)

- [Xie et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib42)

  Y. Xie, F. Yu, K. Achan, R. Panigrahy, G. Hulten, I. Osipkov**Spamming botnets: signatures and characteristics**ACM SIGCOMM Comput Commun Rev, 38 (4) (2008), pp. 171-182[CrossRef](https://doi.org/10.1145/1402946.1402979)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-58449122201&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Spamming botnets%3A signatures and characteristics&publication_year=2008&author=Y. Xie&author=F. Yu&author=K. Achan&author=R. Panigrahy&author=G. Hulten&author=I. Osipkov)

- [Zhuang et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib43)

  L. Zhuang, J. Dunagan, D.R. Simon, H.J. Wang, I. Osipkov, J.D. Tygar**Characterizing botnets from email spam records**LEET, 8 (2008), pp. 1-9[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-70349699173&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Characterizing botnets from email spam records&publication_year=2008&author=L. Zhuang&author=J. Dunagan&author=D.R. Simon&author=H.J. Wang&author=I. Osipkov&author=J.D. Tygar)

- [☆](https://www.sciencedirect.com/science/article/pii/S1742287615000079#baep-article-footnote-id9)

  The authors gratefully acknowledge support from the Canadian Radio-television and Telecommunications Commission (CRTC).

[View Abstract](https://www.sciencedirect.com/science/article/abs/pii/S1742287615000079)

Copyright © 2015 The Authors. Published by Elsevier Ltd.