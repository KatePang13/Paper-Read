# Lessons from Building Observability Tools at Netflix

**Netflix在搭建可观测工具上的经验分享**

origin: https://netflixtechblog.com/lessons-from-building-observability-tools-at-netflix-7cfafed6ab17



Netflix 的使命是通过提供高质量的内容和优质的体验为客户带来欢乐，为此我们的产品一直在快速的发展和创新，包括 个性化的标题建议，基础架构和应用功能(视频下载，用户信息设置等)。我们目前拥有1.25亿的全球用户，他们可以在数千种的设备上享受我们的服务，另一方面考虑到视频内容的种类和数量，维护我们用户的体验质量是一个有趣的挑战。为了应对这个挑战，我们开发了可观测的工具和基础架构来衡量客户的体验，并分析这些观测度量，以便从原始数据种获取更有意义，更深入的信息。可观测性即：analysis of logs, traces, metrics 。在本文，我们分享我们从这学到的 收获：

-  在业务增长的一些时间点，我们意识到存储原始的程序日志无法拓展。为了解决可拓展性，我们选择切换到流日志系统，然后通过一些可配置的规则来过滤日志，将他们转换到内存中，并根据需要进行可持久化
- 当应用迁移到微服务架构，我们需要有方法来对微服务正在做的复杂决策做更深入的观察。分布式请求追踪是一个开始，但是它不足以完全理解应用的行为和问题的根因，通过应用的上下文和智能分析来增加分布式请求追踪也是必须的。
- 除了日志分析和请求追踪，可观测性还包括指标分析。通过探索异常检测和指标相关性，我们学会了如何定义可行性告警，而不仅仅是阈值告警。
- 可观测性工具需要访问很多种类的持久化数据。选择哪种数据库来存储给定的数据类型，取决于这种数据的写入和检索方式。
- 团队和用户之间的数据表示要求差异很大。了解您的用户并提供针对用户个人资料的视图至关重要。

# Scaling Log Ingestion

We started our tooling efforts with providing visibility into device and server logs, so that our users can go to one tool instead of having to use separate data-specific tools or logging into servers. Providing visibility into logs is valuable because log messages include important contextual information, especially when errors occur.

我们的日志系统最开始是用于为设备和服务器日志提供可视化，这样用户可以到一个工具上查看日志，而不是必须使用单独的数据专用工具，甚至登录服务器上查看日志。为日志提供可视化是很有价值的，因为日志中包含了重要的上下文信息，特别是在发生故障的时候。

However, at some point in our business growth, storing device and server logs didn’t scale because the increasing volume of log data caused our storage cost to balloon and query times to increase. Besides reducing our storage retention time period, we addressed scalability by implementing a real-time stream processing platform called [Mantis](https://medium.com/netflix-techblog/stream-processing-with-mantis-78af913f51a6). Instead of saving all logs to persistent storage, Mantis enables our users to stream logs into memory, and keep only those logs that match SQL-like query criteria. Users also have the choice to transform and save matching logs to persistent storage. A query that retrieves a sample of playback start events for the Apple iPad is shown in the following screenshot:

但是，在业务增长的一些关键点，存储设备和服务器日志没法做横线拓展，因为日志数据的数据量的增加导致存储成本和查询事件的增加。除了减少存储的留存周期，我们通过实现一个实时流处理平台[Mantis](https://medium.com/netflix-techblog/stream-processing-with-mantis-78af913f51a6) 来应对可拓展性。有别于以前将所有的日志存储在持久化存储中，Mantis允许用户将日志以流的形式放在内存中，只保留那些匹配类SQL的日志，用户还可以对匹配的日志做转换和持久化等操作。一个 检索【Apple iPad 回退播放开始事件】 的查询如下图所示：

![Image for post](https://miro.medium.com/max/60/0*05M3O_9OoM8lc3rt?q=20)

![Image for post](https://miro.medium.com/max/3200/0*05M3O_9OoM8lc3rt)

Once a user obtains an initial set of samples, they can iteratively refine their queries to narrow down the specific set of samples. For example, perhaps the root cause of an issue is found from only samples in a specific country. In this case, the user can submit another query to retrieve samples from that country.

The key takeaway is that storing all logs in persistent storage won’t scale in terms of cost and acceptable query response time. An architecture that leverages real-time event streams and provides the ability to quickly and iteratively identify the relevant subset of logs is one way to address this problem.

用户获得初始样本集后，他们可以迭代地优化其查询以缩小特定样本集的范围。比如，某个问题的根源只有在一个特定的国家才能发现，这时，用户可以 提交一个新的查询，只检索这个国家的样本数据。



# Distributed Request Tracing

As applications migrated to a microservices architecture, we needed insight into the complex decisions that microservices are making, and an approach that would correlate those decisions. Inspired by Google’s [Dapper paper](https://research.google.com/pubs/pub36356.html) on distributed request tracing, we embarked on implementing request tracing as a way to address this need. Since most inter-process communication uses HTTP and [gRPC](https://grpc.io/docs/guides/) (with the trend for newer services to use gRPC to benefit from its binary protocol), we implemented request interceptors for HTTP and gRPC calls. These interceptors publish trace data to Apache Kafka, and a consuming process writes trace data to persistent storage.

当应用迁移到微服务框架，我们需要深入的视角来观察微服务内部正在做的复杂决策，和一个方法可以对这些决策做关联。收到 Google [Dapper paper](https://research.google.com/pubs/pub36356.html) 分布式请求追踪 的启发，我们开始着手实现请求追踪来解决我们的需求，由于大部分的进程间通信都是使用 HTTP 和 [gRPC](https://grpc.io/docs/guides/) （趋势是更新的服务倾向于使用gRPC,因为它的二进制协议的优点），我们为 HTTP 和 gPRC调用实现了 请求拦截器，拦截器 将 追踪数据上报到 Apache Kafka, 消费进程将追踪数据写入到持久化存储。

The following screenshot shows a sample request trace in which a single request results in calling a second tier of servers, one of which calls a third-tier of servers:

下面的截图展示了一个请求追踪的例子：一个单独的请求结果，这里一个请求触发第二层服务，其中一个第二次服务调用了很多第三层服务。

![Image for post](https://miro.medium.com/max/60/1*ENqhPgT6utTs56XmIu8WLw.png?q=20)

![Image for post](https://miro.medium.com/max/2214/1*ENqhPgT6utTs56XmIu8WLw.png)

Sample request trace

The smaller squares beneath a server indicate individual operations. Gray-colored servers don’t have tracing enabled.

服务下方的较小正方形表示单独的操作，灰色服务器未启用调用跟踪。

A distributed request trace provides only basic utility in terms of showing a call graph and basic latency information. What is unique in our approach is that we allow applications to add additional identifiers to trace data so that multiple traces can be grouped together across services. For example, for playback request traces, all the requests relevant to a given playback session are grouped together by using a playback session identifier. We also implemented additional logic modules called ***analyzers\*** to answer common troubleshooting questions. Continuing with the above example, questions about a playback session might be why a given session did or did not receive 4K video, or why video was or wasn’t offered with High Dynamic Range.

一般的分布式请求追踪只提供了基本的调用图展示和基本延迟信息。我们方法的特点是允许应用程序在追踪数据上添加额外的标识，以便 多个服务间的 多个 追踪 可以被聚合在一起。比如回放请求的追踪，一个给定的回放会话中的所有请求可以通过**回放会话标识**聚合在一起。我们还可以实现额外的逻辑模块来解决常见故障排除问题（我们称这些模块为 **analyzers**）。 对于回放的例子，常见的问题可能是为什么一个给定的会话没有接收到4K视频，或者为什么没有提供 高动态范围成像。

Our goal is to increase the effectiveness of our tools by providing richer and more relevant context. We have started implementing machine learning analysis on error logs associated with playback sessions. This analysis does some basic clustering to display any common log attributes, such as Netflix application version number, and we display this information along with the request trace. For example, if a given playback session has an error log, and we’ve noticed that other similar devices have had the same error with the same Netflix application version number, we will display that application version number. Users have found this additional contextual information helpful in finding the root cause of a playback error.

我们的目标是通过提供更丰富和更相关的上下文来提高分布式追踪工具的效能。我们已经开始在实现基于机器学习的日志分析，比如分析一个回放会话的错误。该分析会对常见的日志特性做简单的聚合，比如 Netflix 应用程序版本号，并在 请求追踪中展示该信息。比如，一个给定的回放会话中有一条错误日志，我们注意到其他相似的设备和相同的应用版本号中也出现了同样的错误，我们就会在追踪中展示该应用的版本号。使用者们已经发现，这种额外的上下文信息对于分析问题的根因是很有用的。

In summary, the key learnings from our effort are that tying multiple request traces into a logical concept, a playback session in this case, and providing additional context based on constituent traces enables our users to quickly determine the root cause of a streaming issue that may involve multiple systems. In some cases, we are able to take this a step further by adding logic that determines the root cause and provides an English explanation in the user interface.

总的来说，我们主要的收获是，尝试将多个请求追踪绑定到一个逻辑概念中，比如案例中的 回放会话，并且提供额外的上下文，帮助用户快速的定位一个可能涉及多个系统的流媒体问题。在某些情况下，我们可以更进一步，添加额外的逻辑来诊断根因，并将其展示在用户界面上。

# Analysis of Metrics

Besides analysis of logging and request traces, observability also involves analysis of metrics. Because having users examine many logs is overwhelming, we extended our offering by publishing log error counts to our metrics monitoring system called [Atlas](https://medium.com/netflix-techblog/introducing-atlas-netflixs-primary-telemetry-platform-bd31f4d8ed9a), which enables our users to quickly see macro-level error trends using multiple dimensions, such as device type and customer geographical location. An alerting system also allows users to receive alerts if a given metric exceeds a defined threshold. In addition, when using Mantis, a user can define metrics derived from matching logs and publish them to Atlas.

Next, we have implemented statistical algorithms to detect anomalies in metrics trends, by comparing the current trend with a baseline trend. We are also working on correlating metrics for related microservices. From our work with anomaly detection and metrics correlation, we’ve learned how to define actionable alerting beyond just basic threshold alerting. In a future blog post, we’ll discuss these efforts.

除了日志分析和请求追踪，可观测性来涉及指标分析。因为让用户检查很多日志是很难做到的，我们提供了上报日志错误计数的方案，上报到我们指标监控系统 [Atlas](https://medium.com/netflix-techblog/introducing-atlas-netflixs-primary-telemetry-platform-bd31f4d8ed9a)，这样我们的用户快速地在多个维度上查看宏观的错误趋势，比如设备类型和用户地位位置。同时有一个可以设置阈值的告警系统，当某个指标达到阈值时，会向用户发送告警。另外，当使用Mantis时，用户可以定义规则，从匹配的日志中产生指标，并将指标上报到 Atlas。

# Data Persistence

We store data used by our tools in Cassandra, Elasticsearch, and Hive. We chose a specific database based primarily on how our users want to retrieve a given data type, and the write rate. For observability data that is always retrieved by primary key and a time range, we use Cassandra. When data needs to be queried by one or more fields, we use Elasticsearch since multiple fields within a given record can be easily indexed. Finally, we observed that recent data, such as up to the last week, is accessed more frequently than older data, since most of our users troubleshoot recent issues. To serve the use case where someone wants to access older data, we also persist the same logs in Hive but for a longer time period.

我们使用自研的工具将数据存储在 Cassandra，Elasticsearch，Hive。我们选择数据库的依据是，这种类型的数据我们要如何检索，以及数据的写的频率。

- 对于可观测数据，总是使用主键和事件范围 检索，我们使用Cassandra；
- 当一个数据需要使用多个域查询时，我们使用Elasticsearch，因为它对于一个记录包含多个域的这种场景更加容易检索。
- 我们经常观察的是最近的数据，比如上一个周的数据，而更早的数据访问频率明显较少，因为我们的用户总是在排查大多时候是在排查近期的问题。对于某些需要排查更早数据的场景，我们将相同的日志存储在Hive中，保存更长的时间。

Cassandra, Elasticsearch, and Hive have their own advantages and disadvantages in terms of cost, latency, and query ability. [Cassandra](https://docs.datastax.com/en/cassandra/3.0/cassandra/dml/dmlIntro.html) provides the best, highest per-record write and read rates, but is restrictive for reads because you must decide what to use for a row key (a unique identifier for a given record) and within each row, what to use for a column key, such as a timestamp. In contrast, Elasticsearch and Hive provide more flexibility with reads because Elasticsearch allows you to index any field within a record, and Hive’s SQL-like query language allows you to match against any field within a record. However, since Elasticsearch is primarily optimized for free text search, its indexing overhead during writes will demand more computing nodes as write rate increases. For example, for one of our observability data sets, we initially stored data in Elasticsearch to be able to easily index more than one field per record, but as the write rate increased, indexing time became long enough that either the data wasn’t available when users queried for it, or it took too long for data to be returned. As a result, we migrated to Cassandra, which had shorter write ingestion time and shorter data retrieval time, but we defined data retrieval for the three unique keys that serve our current data retrieval use cases.

Cassandra, Elasticsearch, and Hive  在开销，延迟和查询能力上 有各自的优缺点：

- Cassandra 提供最佳的读写速度，但是读操作是比较严格的，因为你必须决定对行键使用什么（给定记录的唯一标识符）以及每行中对列键使用什么（例如时间戳）；
- Elasticsearch, and Hive 提供了更灵活的读操作，
  - Elasticsearch 允许对一个记录上任何域的索引；
  - Hive 的 类SQL 查询语言允许对记录上任意域的匹配
- 但是Elasticsearch主要是对文本搜索做优化，随着写入速率的增加，其写入期间的索引的增加将需要更多的计算节点。对于一个观测数据集，Elasticsearch允许我们简单地索引多个域，但当写速率增加时，索引时间会变得很长，要么用户查询时数据不可用，要么数据很长时间才返回。因为，我们最后迁移到Cassandra，Cassandra的写入时间较短，数据检索时间较短，不过我们为这个数据检索定义了三个主键。

For Hive, since records are stored in files, reads are relatively much slower than Cassandra and Elasticsearch because Hive must scan files. Regarding storage and computing cost, Hive is the cheapest because multiple records can be kept in a single file, and data isn’t replicated. Elasticsearch is most likely the next more expensive option, depending on the write ingestion rate. Elasticsearch can also be configured to have [replica shards](https://www.elastic.co/guide/en/elasticsearch/guide/current/replica-shards.html) to enable higher read throughput. Cassandra is most likely the most expensive, since it encourages replicating each record to [more than one replica](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archDataDistributeReplication.html?hl=data%2Creplication) in order to ensure reliability and fault tolerance.

对于Hive，由于记录存储在多个文件中，读也相应地比Cassandra and Elasticsearch 慢，因为Hive必须要扫描多个文件。在存储和计算成本上：

- Hive 是最便宜的
  - 因为多个记录可以被保存在同规格文件中，数据也不需要复制。
- Elasticsearch 次之
  - 写速度相对较快。
  - 可以配置 副本分片，以得到更高的读吞吐量。
- Cassandra 最贵
  - 因为它建议 将记录复制多个副本，以保证可靠性和容错。

# Tailoring User Interfaces for Different User Groups

As usage of our observability tools grows, users have been continually asking for new features. Some of those new feature requests involve displaying data in a view customized for specific user groups, such as device developers, server developers, and Customer Service. On a given page in one of our tools, some users want to see all types of data that the page offers, whereas other users want to see only a subset of the total data set. We addressed this requirement by making the page customizable via persisted user preferences. For example, in a given table of data, users want the ability to choose which columns they want to see. To meet this requirement, for each user, we store a list of visible columns for that table. Another example involves a log type with large payloads. Loading those logs for a customer account increases the page loading time. Since only a subset of users are interested in this log type, we made loading these logs a user preference.

随着我们的可观测工具的发展，用户也在持续提出新的需求。某些需求涉及数据的定制化展示（针对不同团队有不同团队的展示需求，比如设备开发者团队，服务端开发者团队，用户服务团队等）。对于同一个页面，某些用户需要展示页面提供的所有数据类型，而另一些用户只需要看到其中的一部分。针对这个需求，我们实现了基于用户配置的界面定制化。比如，对于一个数据表，用户希望可以选择哪些列展示，哪些列不展示。另一个例子是关于大数据量的日志类型，加载全量的数据会大大增加页面加载时间，如果可以只对某一个子集感兴趣，页面可以根据用户的配置进行加载。

Examining a given log type may require domain expertise that not all users may have. For example, for a given log from a Netflix device, understanding the data in the log requires knowledge of some identifiers, error codes, and some string keys. Our tools try to minimize the specialized knowledge required to effectively diagnose problems by joining identifiers with the data they refer to, and providing descriptions of error codes and string keys.

检查一个日志类型需要相关的领域经验，会遇到一些用户刚好不熟悉这个领域的情况。比如，一个来源于特定设备的日志，理解这个日志的数据需要对某些标识，错误码和错误描述有所了解。我们的工具尝试让用户在排查问题时，所需要依赖的知识最小化，通过 将标识符与其引用的数据结合在一起，提供易于理解的错误码和错误描述等措施来实现。

In short, our learning here is that customized views and helpful context provided by visualizations that surface relevant information are critical in communicating insights effectively to our users.

总的来说，我们在这方面的经验是：在可视化上提供定制化页面和有帮助的上下文信息（显示相关信息），对于有效地向用户传递信息是非常重要的。

# Conclusion

Our observability tools have empowered many teams within Netflix to better understand the experience we are delivering to our customers and quickly troubleshoot issues across various facets such as devices, titles, geographical location, and client app version. Our tools are now an essential part of the operational and debugging toolkit for our engineers. As Netflix evolves and grows, we want to continue to provide our engineers with the ability to innovate rapidly and bring joy to our customers. In future blog posts, we will dive into technical architecture, and we will share our results from some of our ongoing efforts such as metrics analysis and using machine learning for log analysis.

我们的可观察性工具使Netflix中的许多团队有能力更好地了解我们为客户提供的体验，并快速解决各个方面(设备，标题，地理位置和客户端版本)的问题。现在，对于我们的工程师而言，我们的工具已成为运维和调试工具包的重要组成部分。随着Netflix的发展和壮大，我们希望继续为我们的工程师提供快速创新的能力，并为我们的客户带来欢乐。在未来的博客文章中，我们将深入探讨技术架构，并且我们将分享我们正在进行的一些工作的成果，例如指标分析和使用机器学习进行日志分析。