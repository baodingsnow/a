## 为什么使用elasticsearch

直接使用mysql的like查询语句会影响系统的性能，最重要的是非关系型数据不能模糊查询，而且遍历数据库查询很慢，所以采用elasticsearch来实现站内搜索

除了要搜索还要分析

## 技术栈

Elasticsearch kibanna beats logstash  

全文搜索引擎



## 2.springboot整合elasticsearch

本文使用的SpringBoot版本为**2.3.7.RELEASE**
对应的Elasticsearch版本为**7.6.2**

```xml
```````````<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>



<dependency>     

<groupId>org.springframework.boot</groupId>    

  <artifactId>spring-boot-starter-data-jpa</artifactId> 

</dependency> 
```

## 3.elasticsearch（apipost版本）

elasticsearch面向文档型数据库      

### 倒排索引

根据关键字查询   (一般关系型数据库查询要遍历数据库)

一般没有表的概念，创建索引等于创建数据库 

### 修改文档

完全覆盖 幂等性 put操作

```xml
//请求地址
localhost:9200/shopping/_doc/100
//body
{
    "title": "黑米手机",
    "category": "香蕉",
    "price": 3999
}
```

局部更新 非幂等性 post方式

```
//请求地址
localhost:9200/shopping/_update/100
//body
{
    "doc" : {
         "title" : "华为手机"
    }
}
```

全部更新，是直接把之前的老数据，标记为删除状态，然后，再添加一条更新的。

 局域更新，只是修改某个字段。

### 创建索引



创建文档 相当于表数据

> 创建文档必须要用post请求 因为put是幂等性请求   我们创建的文档每次id都是随机的
>
> 在请求路径中添加id就是幂等性操作了，就可以使用put方法了

正向索引 

根据编号设置主键和索引  再去查找信息

### 查询（重要）

#### 分页查询 

```yml
{
    "query":{
        "match_all" :{
            
        }
    },
    "from" : 1,
    "size" : 1
}
```

from  类似数组下标 从0开始数  

size 每页显示的数量

加上"_source"：["title"]    指定数据查询

"sort" : { 

"price" : {"order " : desc}

} //对查询结果进行排序

#### 多条件查询

```yml
{
    "query" :{
        "bool" : {
            "must" : [
                {
                    "match" : {
                        "category" : "香蕉"
                    }
                },
                {
                    "match" : {
                        "price" : 2999
                    }
                }
            ]
        }
    }
}
```

must是and should是or 条件

#### 范围查询

```yml
{
    "query" :{
        "bool" : {
            "must" : [
                {
                    "match" : {
                        "category" : "香蕉"
                    }
                }
            ],
            "filter" : {
                "range" : {
                    "price" : {
                        "gt" : 3000
                    }
                }
            }
        }
    }
}
```

## 4.elasticsearch(javaApi版本)

springboot项目中添加相关依赖

```java
<dependencies>
    <dependency>
        <groupId>org.elasticsearch</groupId>
        <artifactId>elasticsearch</artifactId>
        <version>7.6.1</version>
    </dependency>
    <!-- elasticsearch 的客户端 -->
    <dependency>
        <groupId>org.elasticsearch.client</groupId>
        <artifactId>elasticsearch-rest-high-level-client</artifactId>
        <version>7.6.1</version>
    </dependency>
    <!-- elasticsearch 依赖 2.x 的 log4j -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.8.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.8.2</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.9</version>
    </dependency>
    <!-- junit 单元测试 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
</dependencies>
```

基于elasticsearch的crud操作都需要创建客户端对象

以创建索引为例

```java
/**
首先创建客户端对象
创建索引请求对象
发送请求
获取响应结果
*/
public static void main(String[] args) throws  Exception{
    //首先创建客户端对象
      RestHighLevelClient esClient = new RestHighLevelClient(RestClient.builder(new HttpHost("localhost",9200,"http")));
    //创建索引请求对象
           CreateIndexRequest request= new CreateIndexRequest("user1");
    //发送请求
//获取响应结果
           CreateIndexResponse createIndexResponse = esClient.indices().create(request, RequestOptions.DEFAULT);




}
```

### elasticsearch中一些方法和类简要记录

NativeSearchQuery  查询结果高亮显示

bool query（组合查询）是把任意多个简单查询组合在一起，使用 **must**、**should**、**must_not**、**filter** 选项来表示简单查询之间的逻辑，每个选项都可以出现 0 次到多次。它是为了满足现实中比较复杂的查询需求，如需要在多个字段上查询多种多样的文本，并且根据一系列的标准来过滤。

```java
    @Autowired
    private ElasticsearchRestTemplate restTemplate;
    @Autowired
    private EmployeeRepository repository;

操作索引和数据时需要用到这两个数据
```

BulkRequest对象可以用来在一次请求中，执行多个索引、更新或删除操作

### 批量操作
