# What is Envoy

Envoy is an L7 proxy and communication bus designed for large modern service oriented architectures. The project was born out of the belief that:

> *The network should be transparent to applications. When network and application problems do occur it should be easy to determine the source of the problem.*

Envoy 是一个 为 **大规模面向服务框架** 设计**7层代理** 和 **通信总线**。该项目是基于这个理念诞生的：

> * *网络对应用程序应该是透明的。当网络和应用程序出现问题时，问题的根源应该是容易定位的.*

In practice, achieving the previously stated goal is incredibly difficult. Envoy attempts to do so by providing the following high level features:

在工程实践中，实现以上的目标是非常困难的，Envoy 尝试提供以下高级特性来实现这个目标：

**Out of process architecture:** Envoy is a self contained process that is designed to run alongside every application server. All of the Envoys form a transparent communication mesh in which each application sends and receives messages to and from localhost and is unaware of the network topology. The out of process architecture has two substantial benefits over the traditional library approach to service to service communication:

- Envoy works with any application language. A single Envoy deployment can form a mesh between Java, C++, Go, PHP, Python, etc. It is becoming increasingly common for service oriented architectures to use multiple application frameworks and languages. Envoy transparently bridges the gap.
- As anyone that has worked with a large service oriented architecture knows, deploying library upgrades can be incredibly painful. Envoy can be deployed and upgraded quickly across an entire infrastructure transparently.

**独立进程**: Envoy 是一个跑在每个应用服务器旁边的独立进程。所有的Envoy 组成一个透明的通信网格，每个应用程序都与localhost 收发数据包，对网络拓扑结构是无感知的。独立进程框架与传统的library方法比较，有2个主要的优点：

- Envoy 可以作用于任何语言。一个Envoy集群可以组成一个由 java,C++,Go, PHP, Python 等语言应用程序构成的网格。微服务架构中，使用多框架和编程语言变得越来越普遍，而Envoy 可以透明地弥补语言差异。 
- 任何参与过大规模微服务架构的人都知道， 基础库的部署升级时非常痛苦的，Envoy 可以做到快速的全网升级部署，这对于应用是无感知的。 

**Modern C++11 code base:** Envoy is written in C++11. Native code was chosen because we believe that an architectural component such as Envoy should get out of the way as much as possible. Modern application developers already deal with tail latencies that are difficult to reason about due to deployments in shared cloud environments and the use of very productive but not particularly well performing languages such as PHP, Python, Ruby, Scala, etc. Native code provides generally excellent latency properties that don’t add additional confusion to an already confusing situation. Unlike other native code proxy solutions written in C, C++11 provides both excellent developer productivity and performance.

**基于现代C++**: Envoy 基于C++11。之所以选择原生语言，是因为我们认为像Envoy这样的底层组件应该尽可能的不产生额外开销。由于部署在共享的云环境和使用高生产力但是性能不佳的编程语言(如PHP,Python,Ruby,Scala等)，应用开发者处理长尾延迟问题非常困难。原生语言在延迟方面表现出色，不会让本就复杂的情况雪上加霜。不同意其他的原生proxy解决方案使用C语言编写，Envoy使用C++,可以兼顾开发者生产力和性能。

**L3/L4 filter architecture:** At its core, Envoy is an L3/L4 network proxy. A pluggable [filter](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/listeners/network_filters#arch-overview-network-filters) chain mechanism allows filters to be written to perform different TCP proxy tasks and inserted into the main server. Filters have already been written to support various tasks such as raw [TCP proxy](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/listeners/tcp_proxy#arch-overview-tcp-proxy), [HTTP proxy](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/http_connection_management#arch-overview-http-conn-man), [TLS client certificate authentication](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/security/ssl#arch-overview-ssl-auth-filter), etc.

**L3/L4 filter architecture:**  本质上，Envoy 是一个 L3/L4 网络代理。插件化的 Filter 链机制允许编写额外的Filters来处理不同的TCP 代理任务，并插入到主服务中。已经实现了若干个Filter ，比如 raw TCP proxy, HTTP proxy, TLS client certificate authentication 等。

**HTTP L7 filter architecture:** HTTP is such a critical component of modern application architectures that Envoy [supports](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/http_filters#arch-overview-http-filters) an additional HTTP L7 filter layer. HTTP filters can be plugged into the HTTP connection management subsystem that perform different tasks such as [buffering](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/buffer_filter#config-http-filters-buffer), [rate limiting](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/other_features/global_rate_limiting#arch-overview-global-rate-limit), [routing/forwarding](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/http_routing#arch-overview-http-routing), sniffing Amazon’s [DynamoDB](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/other_protocols/dynamo#arch-overview-dynamo), etc.

**HTTP L7 filter architecture:**  HTTP 是现代应用体系中的核心模块。Envoy 支持额外的 HTTP L7 filter layer。HTTP Filters 可以以插件形式插入到HTTP连接管理子系统，处理不同的任务，比如： 缓存, 限速，路由/转发，探测 Amazon's DynamoDB 等。

**First class HTTP/2 support:** When operating in HTTP mode, Envoy [supports](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/http_connection_management#arch-overview-http-protocols) both HTTP/1.1 and HTTP/2. Envoy can operate as a transparent HTTP/1.1 to HTTP/2 proxy in both directions. This means that any combination of HTTP/1.1 and HTTP/2 clients and target servers can be bridged. The recommended service to service configuration uses HTTP/2 between all Envoys to create a mesh of persistent connections that requests and responses can be multiplexed over. Envoy does not support SPDY as the protocol is being phased out.

**一流的HTTP/2 支持：** 在HTTP模型下，Envoy支持 HTTP/1.1 和 HTTP/2。Envoy 可以透明地充当HTTP/1.1 和 HTTP/2 之间的双向代理（注：应用程序不需要关心对端的HTTP版本，Envoy可以自动适配），HTTP/1.1 和 HTTP/2 两端的任意组合都可以相互通信。服务端之间推荐使用Envoy之间的HTTP / 2来创建持久连接的网格，这样请求和响应可以多路复用。Envoy不支持SPDY，因为SPDY协议正在慢慢淘汰。

**HTTP L7 routing:** When operating in HTTP mode, Envoy supports a [routing](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/http_routing#arch-overview-http-routing) subsystem that is capable of routing and redirecting requests based on path, authority, content type, [runtime](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/operations/runtime#arch-overview-runtime) values, etc. This functionality is most useful when using Envoy as a front/edge proxy but is also leveraged when building a service to service mesh.

**HTTP L7 routing:**  在HTTP模式下， Envoy 有一个 路由子系统来提供路由和重定向，可以基于 path, authority, content type, [runtime](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/operations/runtime#arch-overview-runtime) values 等策略做路由。Envoy这个功能在用于前端/边缘代理时能最好的发挥作用，当然在搭建服务网格时也时能发挥一定的作用。

**gRPC support:** [gRPC](https://www.grpc.io/) is an RPC framework from Google that uses HTTP/2 as the underlying multiplexed transport. Envoy [supports](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/other_protocols/grpc#arch-overview-grpc) all of the HTTP/2 features required to be used as the routing and load balancing substrate for gRPC requests and responses. The two systems are very complementary.

**gRPC support:**  [gRPC](https://www.grpc.io/)  是一个 Google开源的 RPC框架，使用 HTTP/2 作为底层的多路复用传输。gPRC 路由和负载均衡 所需的全部 HTTP/2 特性，Envoy 全部支持。HTTP/2 和 gRPC 是非常互补的。

**MongoDB L7 support:** [MongoDB](https://www.mongodb.com/) is a popular database used in modern web applications. Envoy [supports](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/other_protocols/mongo#arch-overview-mongo) L7 sniffing, statistics production, and logging for MongoDB connections.

**MongoDB L7 support:** [MongoDB](https://www.mongodb.com/) 是现代 web 应用的常用数据库，Envoy 支持 L7 嗅探，生成统计数据和MongoDB 连接日志。

**DynamoDB L7 support**: [DynamoDB](https://aws.amazon.com/dynamodb/) is Amazon’s hosted key/value NOSQL datastore. Envoy [supports](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/other_protocols/dynamo#arch-overview-dynamo) L7 sniffing and statistics production for DynamoDB connections.

**DynamoDB L7 support**: [DynamoDB](https://aws.amazon.com/dynamodb/) is Amazon托管的键值数据库。Envoy 支持 L7 嗅探 和 生成 DynamoDB 连接 统计数据。

**Service discovery and dynamic configuration:** Envoy optionally consumes a layered set of [dynamic configuration APIs](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/operations/dynamic_configuration#arch-overview-dynamic-config) for centralized management. The layers provide an Envoy with dynamic updates about: hosts within a backend cluster, the backend clusters themselves, HTTP routing, listening sockets, and cryptographic material. For a simpler deployment, backend host discovery can be [done through DNS resolution](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/service_discovery#arch-overview-service-discovery-types-strict-dns) (or even [skipped entirely](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/service_discovery#arch-overview-service-discovery-types-static)), with the further layers replaced by static config files.

**Service discovery and dynamic configuration:**  .Envoy可以选择使用一组分层的[动态配置API](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/operations/dynamic_configuration#arch-overview-dynamic-config)进行集中管理 ，这样可以提供一个可以动态更新的Envoy，动态更新内容包括：后端集群，后端集群的主机列表，HTTP路由，监听的套接字和加密套件。对于更简单的部署，后端主机发现可以基于DNS提供，并将其他层替换为静态配置文件。

**Health checking:** The [recommended](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/service_discovery#arch-overview-service-discovery-eventually-consistent) way of building an Envoy mesh is to treat service discovery as an eventually consistent process. Envoy includes a [health checking](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/health_checking#arch-overview-health-checking) subsystem which can optionally perform active health checking of upstream service clusters. Envoy then uses the union of service discovery and health checking information to determine healthy load balancing targets. Envoy also supports passive health checking via an [outlier detection](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/outlier#arch-overview-outlier-detection) subsystem.

**Health checking:**   搭建一个Envoy网格，推荐的方式是将服务发现当作一个最终一致的过程。Envoy 包含 一个  [health checking](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/health_checking#arch-overview-health-checking) 子系统，可以选择 对 上游服务集群 做 主动健康检查。Envoy 结合 服务发现和健康检查信息，确定负载均衡的目标机器。Envoy 同样支持 被动 监控检查，基于  [outlier detection](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/outlier#arch-overview-outlier-detection)  子系统。

**Advanced load balancing:** [Load balancing](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/load_balancing/overview#arch-overview-load-balancing) among different components in a distributed system is a complex problem. Because Envoy is a self contained proxy instead of a library, it is able to implement advanced load balancing techniques in a single place and have them be accessible to any application. Currently Envoy includes support for [automatic retries](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/http_routing#arch-overview-http-routing-retry), [circuit breaking](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/circuit_breaking#arch-overview-circuit-break), [global rate limiting](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/other_features/global_rate_limiting#arch-overview-global-rate-limit) via an external rate limiting service, [request shadowing](https://www.envoyproxy.io/docs/envoy/latest/api-v3/config/route/v3/route_components.proto#envoy-v3-api-msg-config-route-v3-routeaction-requestmirrorpolicy), and [outlier detection](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/outlier#arch-overview-outlier-detection). Future support is planned for request racing.

**Advanced load balancing:** 在分布式系统中，不同组件间的负载均衡是一个复杂的问题。因为Envoy 不是一个函数库，而是一个独立的进程，可以只在Envoy内 实现高级的 负载均衡技术，并提供给所有的应用 使用。目前，Envoy 支持 自动重试，熔断，基于外部限流服务的全局流量控制，request shadowing(对请求做镜像？) ，外部探测 等。request racing 计划在后续支持（ request racing: 请求竞争，一个请求，fork出多个请求，分发给多个后端，取最快的响应返回，该技术只要用以避免较大的长尾延迟对服务整体响应的影响  https://github.com/envoyproxy/envoy/issues/2784 ）。

**Front/edge proxy support:** Although Envoy is primarily designed as a service to service communication system, there is benefit in using the same software at the edge (observability, management, identical service discovery and load balancing algorithms, etc.). Envoy includes enough features to make it usable as an edge proxy for most modern web application use cases. This includes [TLS](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/security/ssl#arch-overview-ssl) termination, HTTP/1.1 and HTTP/2 [support](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/http_connection_management#arch-overview-http-protocols), as well as HTTP L7 [routing](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/http_routing#arch-overview-http-routing).

**Front/edge proxy support:**  虽然 Envoy 主要角色是 充当 服务之间的 通信系统，但它同样可以充当边缘节点（提供可观测性，管理，服务发现，负载均衡算法等）。Envoy 拥有足够的特性，可以胜任大多数web应用场景的 边缘 代理，这些特性包括 TLS 终端，HTTP/1.1， HTTP/2 和 HTTP L7 路由。

**Best in class observability:** As stated above, the primary goal of Envoy is to make the network transparent. However, problems occur both at the network level and at the application level. Envoy includes robust [statistics](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/observability/statistics#arch-overview-statistics) support for all subsystems. [statsd](https://github.com/etsy/statsd) (and compatible providers) is the currently supported statistics sink, though plugging in a different one would not be difficult. Statistics are also viewable via the [administration](https://www.envoyproxy.io/docs/envoy/latest/operations/admin#operations-admin-interface) port. Envoy also supports distributed [tracing](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/observability/tracing#arch-overview-tracing) via thirdparty providers.

**优秀的可观测性:**   如上所述，Envoy的主要目标是 做到网络对应用是透明。但是，在网络层和应用层都会出现问题。Envoy 为所有 子系统提供了健壮的统计支持。 [statsd](https://github.com/etsy/statsd) (和其他兼容的第三方) 是当前支持的 统计 sink,  当然使用其他的统计sink 也不难。统计信息可以通过管理端口查看。Envoy 同样支持基于第三方的分布式追踪。

## Design goals

A short note on the design goals of the code itself: Although Envoy is by no means slow (we have spent considerable time optimizing certain fast paths), the code has been written to be modular and easy to test versus aiming for the greatest possible absolute performance. It’s our view that this is a more efficient use of time given that typical deployments will be alongside languages and runtimes many times slower and with many times greater memory usage.

简单总结本工程的设计目标：尽管Envoy并不意味着慢（我们花费大量的时间优化某些关键路径的速度），但是本工程更追求的是模块化和易于测试，而不是追求极致的性能。我们认为，这是一种更有效的时间利用方式(把时间花在提高模块化和易测试性上)，因为envoy几乎总是与应用程序的语言和运行时部署在一起，主要的时间消耗和空间消耗是在这些应用程序的语言和运行时。