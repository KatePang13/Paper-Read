# Terminology

### Host

一个能够进行网络通信的个体（比如，手机上的一个应用）。在本文档中，host 是一个逻辑网络应用。一个物体机器上可以有多个host，每个 host 都可以单独寻址

### Downstream

一个Downstream host 连接到Envoy, 发送请求并接收响应

### Upstream

一个upstream host 接收来自 Envoy 的连接和请求，并返回响应。

### Listener

一个 Listener 是一个已命名的网络位置（比如 port，unix domain socket 等 ），可以被 Downstream clients 连接。Envoy 暴露 一个或多个Envoy 给 Downstream host

### Cluster

Cluster 是一组逻辑相似的 upstream host。Envoy  通过服务发现来感知Cluster的成员，可以基于主动健康检测来判断Cluster成员的健康状况。Envoy 根据负载均衡策略，将请求路给Cluster中的某一台host, 

### Mesh

一组host共同协作，提供一个一致的网络拓扑。 在本文档中，一个 Envoy Mesh  由一组 Envoy proxy组成，组成一个供分布式系统使用的消息传输平面，分布式系统包括不同的服务和不同的应用平台。

### Runtime Configuration

与Envoy一起部署的外部实时配置系统。可修改的配置可以实时生效，不需要重启Envoy或者改变主配置文件。