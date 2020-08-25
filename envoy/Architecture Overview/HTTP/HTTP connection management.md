# HTTP connection management

HTTP 是现代微服务体系中的核心组件，Envoy 实现了大量的HTTP特定功能。

Envoy 有一个内建的 Network Level Filter 叫 [HTTP connection manager](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_conn_man/http_conn_man#config-http-conn-man)。

- 该Filter 将 Raw Bytes 解析成 HTTP 报文和事件（比如：header收到事件，body data收到事件，Trailer 收到事件）。

- 该Filter 还 处理所有连接和请求通用的功能，比如：访问日志，请求ID的生成与追踪，请求头响应头修改，路由表管理，统计信息。

  [access logging](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/observability/access_logging#arch-overview-access-logs), [request ID generation and tracing](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/observability/tracing#arch-overview-tracing), [request/response header manipulation](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_conn_man/headers#config-http-conn-man-headers), [route table](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/http_routing#arch-overview-http-routing) management, and [statistics](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_conn_man/stats#config-http-conn-man-stats).

HTTP connection manager [configuration](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_conn_man/http_conn_man#config-http-conn-man).

## HTTP Protocols

Envoy 的HTTP连接管理器 已经原生支持 HTTP/1.1, WebSockets，HTTP/2，不支持 SPDY。

Envoy的HTTP支持，首先保证的是是支持 HTTP/2 多路复用代理。在内部，HTTP/2 的术语被用以描述 系统组件，比如，HTTP请求和响应发生在一个流。 编解码器 API 用于 将不同的协议报文 编码成 流，请求，响应等的协议无关形式。在HTTP/1.1 的场景下，编解码器将协议的串行/流水线功能转换为类似于HTTP/2 的上层协议。这意味着大多数代码是与 HTTP协议版本无关的，代码不需要关注一个流是 HTTP/1.1还是HTTP/2



## HTTP Header Sanitizing

## Route Table Configuration

## Retry Plugin Configuration

## Internal Redirects

## TimeOut

