# Listeners

Envoy 配置中，单个线程可支持的Listener 数量是不限的 。通常，我们推荐每个机器单独运行一个 Envoy，不管配置了多大数量的Listener。这使得运维更容易，且只有一个统计数据的来源。Envoy 支持 TCP 和 UCP Listener。

## TCP

- TCP Listener
  - Filter Chains (Network level (L3/L4) Filters)   【后执行】
  - Listener Filters  【先执行】



每个Listener可以单独配置若干个 [Filter链](https://www.envoyproxy.io/docs/envoy/latest/api-v3/config/listener/v3/listener_components.proto#envoy-v3-api-msg-config-listener-v3-filterchain),  会根据[匹配条件](https://www.envoyproxy.io/docs/envoy/latest/api-v3/config/listener/v3/listener_components.proto#envoy-v3-api-msg-config-listener-v3-filterchainmatch)选择相应的链。一个Filter链由一个或多个 Network level Filter 组成 。当Listener接收到一个新连接，相应的Filter链被选择，配置的本地Filter堆栈被实例化，并开始处理后续事件。大多数Envoy的代理任务都使用通用Listener结构来完成（比如：  [rate limiting](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/other_features/global_rate_limiting#arch-overview-global-rate-limit), [TLS client authentication](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/security/ssl#arch-overview-ssl-auth-filter), [HTTP connection management](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/http_connection_management#arch-overview-http-conn-man), MongoDB [sniffing](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/other_protocols/mongo#arch-overview-mongo), raw [TCP proxy](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/listeners/tcp_proxy#arch-overview-tcp-proxy)）。

Listener 也可以配置 若干个 Listener Filters。Listener Filters 在 Network Level Filter 之前执行，可以修改 连接元数据，通常会影响后续 Listener Filters 和 集群的处理。

Listener 也可以通过   [listener discovery service (LDS)](https://www.envoyproxy.io/docs/envoy/latest/configuration/listeners/lds#config-listeners-lds) 动态获取。

Listener [configuration](https://www.envoyproxy.io/docs/envoy/latest/configuration/listeners/listeners#config-listeners).

## UDP

Envoy 同样支持  UDP Listener 和 [UDP listener filters](https://www.envoyproxy.io/docs/envoy/latest/configuration/listeners/udp_filters/udp_filters#config-udp-listener-filters)。每个worker线程 实例化一次 UDP listener filters，在该worker内是全局的。listener filter 处理 worker监听端口接收到的UDP数据报。  实践中， UDP listener 会配置 SO_REUSEPORT 内核选项，然后 kernel 总是会将 一个 UDP4元组 hash 给同一个 worker。这使得有一个 UDP listener filters 可以做到 面向会话。我们有一个该功能的内建示例： [UDP proxy](https://www.envoyproxy.io/docs/envoy/latest/configuration/listeners/udp_filters/udp_proxy#config-udp-listener-filters-udp-proxy) listener filter。

---

## Listener Filters

在上面我们谈到，listener filters 可以用来修改连接的元数据。listener filters 的主要目的是 

- 使添加其他系统集成功能更容易，而不需要修改 Envoy的核心功能；
- 使多个功能之间的交互更清晰明确。

listener filters的API相对简单，因为最终这些filter将在新接受的套接字上运行。链中的filter可以停止，然后继续迭代到后续的Filters，这就允许我们处理更复杂的场景，比如调用一个 [rate limiting service](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/other_features/global_rate_limiting#arch-overview-global-rate-limit)。Envoy已经包含的Listener Filters 会在后续介绍。

## Network(L3/L4) FIlters

前面谈到，Network(L3/L4) FIlters 是 Envoy 连接处理的主体部分。Filter API 允许不同集合的Filters 混合，匹配，并附加到给定的Listener。有三种类型的network Filters:

- **Read:**   当Envoy 收到来自 downstream 连接的数据时采用 
- **Write:**  当Envoy 发送数据到 downstream 时采用
- **Read/Write:**  Envoy 与 downstream 收发数据时，均会采用

network Filters API 相关简单，因为最终 Filter 将对 raw字节 和少数事件进行处理。（比如: TLS握手完成，连接本地断开或远程断开）   

一个链上的 Filters 可以停止，并迭代到下一个Filters。这允许我们处理比较复杂的场景，比如调用一个   [rate limiting service](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/other_features/global_rate_limiting#arch-overview-global-rate-limit)  。在同一个 downstream connection 中， Network Level Filters 之间可以共享状态（静态和动态），详细内容可以参考  [data sharing between filters](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/advanced/data_sharing_between_filters#arch-overview-data-sharing-between-filters) 。 Envoy已经包含的 Network Level Filters 请参考 [configuration reference](https://www.envoyproxy.io/docs/envoy/latest/configuration/listeners/network_filters/network_filters#config-network-filters) 。



## TCP Proxy

因为 Envoy 基本上是以一个 L3/L4 服务器 编写的，所以，基本的 L3/L4 proxy 是很容易实现的。TCP Proxy Filter  执行 1对1 网络连接代理 ，代理 downstream clients 与 upstream clusters 之间的 连接。它可以单独用作通道替换，也可以与其他Filters（ [MongoDB filter](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/other_protocols/mongo#arch-overview-mongo) or the [rate limit](https://www.envoyproxy.io/docs/envoy/latest/configuration/listeners/network_filters/rate_limit_filter#config-network-filters-rate-limit) filter）结合使用。

TCP Proxy Filter 将遵守 upstream cluster 全局资源管理器上设置的连接限制。TCP Proxy Filter 会检查 upstream cluster 的资源管理器，如果没有超过 cluster的最大连接数则创建，否则取消创建。

TCP proxy filter [configuration reference](https://www.envoyproxy.io/docs/envoy/latest/configuration/listeners/network_filters/tcp_proxy_filter#config-network-filters-tcp-proxy).



## UDP Proxy

Envoy 对UDP代理的支持基于 [UDP proxy listener filter](https://www.envoyproxy.io/docs/envoy/latest/configuration/listeners/udp_filters/udp_proxy#config-udp-listener-filters-udp-proxy).



## DNS Filter

Envoy 支持通过配置  [UDP listener DNS Filter](https://www.envoyproxy.io/docs/envoy/latest/configuration/listeners/udp_filters/dns_filter#config-udp-listener-filters-dns-filter) 来响应DNS请求。

DNS Filter 支持 响应 A记录 和 AAAA记录 的前向查询。DNS Answer 来源于 静态配置的资源，集群或者外部DNS服务器。Filter 将返回最多512字节的DNS响应。如果域名配置了多个地址，或者集群配置了多个终端，Envoy 将返回所有发现的地址，每个地址的长度限制为512字节。

