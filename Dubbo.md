Apache Dubbo：基于 java 的高性能 RPC 框架，其理念 定义服务接口及其方法的出入参类型供远程调用。

在服务端，需要服务接口的具体逻辑，并通过启动 dubbo 服务来处理客户端的调用，而客户端需要拥有与服务端相同的接口



功能：基于接口的远程调用，容错性&负载均衡，服务的自动注册与发现



架构：注册中心（Registry）、消费方（Consumer）、提供方（Provider）、监控（Monitor）、容器（Container）



0. start 服务提供方应用启动，将提供的服务初始化到容器中
1. register 提供方在应用启动后将服务注册至注册中心
2. subscribe 消费方在应用启动后会向注册中心发起服务预定，同时获取注册中心里已经存在的服务列表并缓存至本地
3. notify 注册中心会通过异步通知方式，实时将提供方的最新状态推送至消费方
4. invoke 消费方可以根据本地服务内容进行服务调用
5. count 消费方和服务方会将服务调用状态、次数等内容推送至监控端





高性能RPC：

协议栈：dubbo 支持自定义 RPC 协议，冗余字段少，通信性能高；序列化协议支持 hessian2、dubbo 自定义序列化等高性能协议



线程模型：依赖 netty4 的非阻塞线程模型，支持 I/O、业务逻辑分离，通过 handler 链处理请求



自动注册/发现、负载均衡等服务化特性：

注册中心：

zookeeper和nacos

dubbo支持 redis作为注册中心，要求服务器时间同步，性能消耗过大