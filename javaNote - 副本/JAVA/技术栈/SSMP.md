# 什么是SSMP

spirngmvc+Spring+mybatisplus

## 什么是springMVC

Spring MVC 是 Spring 提供的一个基于 MVC 设计模式的轻量级 Web 开发框架，本质上相当于 Servlet。

Spring MVC 是结构最清晰的 Servlet+JSP+JavaBean 的实现，是一个典型的教科书式的 MVC 构架

> 个人理解：SpringMVC开发模式中Controller代替servlet起到控制器的作用，接收前端发送过来的请求，并调用相应的Model进行处理，处理器完成业务处理后返回处理结果，Controller调用相应的view进行视图渲染，最终客户端得到响应信息。

### 什么是MVC

view  视图层  model 模型层 controller控制层

model 模型表示业务规则    应用于模型的代码可以被多个视图重用

> MVC三层架构是一种设计模式  三层架构是一种软件结构，三层架构基于逻辑来区分，mvc基于页面来分
>
> 个人理解：mvc中的view controller对应三层架构中的表现层
>
> mvc中的model为一种概念不是具体体现，具体开发中，包括实体类，service层，mapper层
>
>   三层架构中的model由业务逻辑、数据访问层组成

## 什么是Spring

spring是一个开源的，轻量级的开发框架，核心是控制反转(IOC)和面向切面（AOP）



