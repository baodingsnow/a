# 什么是 Spring WebFlux

**Spring MVC 构建于 Servlet API 之上，使用的是同步阻塞式 I/O 模型，什么是同步阻塞式 I/O 模型呢？就是说，每一个请求对应一个线程去处理。**

**Spring WebFlux 是一个异步非阻塞式的 Web 框架，它能够充分利用多核 CPU 的硬件资源去处理大量的并发请求。**

# WebFlux 的优势&提升性能?



**WebFlux 并不能使接口的响应时间缩短，它仅仅能够提升吞吐量和伸缩性**。

# 简单看看 WebFlux 是如何分发请求的
