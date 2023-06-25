# 什么是elasticsearch

> Elasticsearch是一个基于[Lucene](https://baike.baidu.com/item/Lucene/6753302)的搜索服务器。它提供了一个分布式多用户能力的[全文搜索引擎](https://baike.baidu.com/item/全文搜索引擎/7847410)，基于RESTful web接口。Elasticsearch是用Java语言开发的，并作为Apache许可条款下的开放源码发布，是一种流行的企业级搜索引擎。Elasticsearch用于[云计算](https://baike.baidu.com/item/云计算/9969353)中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。官方客户端在Java、.NET（C#）、PHP、Python、Apache Groovy、Ruby和许多其他语言中都是可用的。根据DB-Engines的排名显示，Elasticsearch是最受欢迎的企业搜索引擎，其次是Apache Solr，也是基于Lucene。

- Elasticsearch对模糊搜索非常擅长（搜索速度很快）
- 从Elasticsearch搜索到的数据可以根据**评分**过滤掉大部分的，只要返回评分高的给用户就好了（原生就支持排序）
- 没有那么准确的关键字也能搜出相关的结果（能匹配有相关性的记录）

## 常见术语

- **Index**：Elasticsearch的Index相当于数据库的Table
- **Type**：这个在新的Elasticsearch版本已经废除（在以前的Elasticsearch版本，一个Index下支持多个Type--有点类似于[消息队列](https://cloud.tencent.com/product/cmq?from=10680)一个topic下多个group的概念）
- **Document**：Document相当于数据库的一行记录
- **Field**：相当于数据库的Column的概念
- **Mapping**：相当于数据库的Schema的概念
- **DSL**：相当于数据库的SQL（给我们读取Elasticsearch数据的API）

# 云端系统日志代码

## 查询访问日志

```java
// serviceimpl
return  new PageResult<>(count,entity.getPageIndex(), entity.getPageSize(),accessLogList);
//controller
 return Result.success(accessLoggerService.getAllAccessLogger(entity));
```

目测是分页功能