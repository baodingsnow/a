# MyBatis-Plus QueryWrapper及LambdaQueryWrapper的使用

###### QueryWrapper最基础的使用方式

```java
QueryWrapper<T> wrapper= new QueryWrapper<>();
wrapper.eq("xx_id",id);
List<T> obj=mapper.selectList(wrapper);

/**
查询单条
*/
QueryWrapper<T> wrapper= new QueryWrapper<>();
wrapper.eq("xx_id",id);
Obj obj=mapper.selectOne(wrapper)
```

###### 

LambdaQueryWrapper的好处是写实体属性，而不是数据库字段，这样就不用担心数据库字段改了需要修改java中的代码,到时候直接在实体类修改就好了

```java
QueryWrapper<BannerItem> wrapper = new QueryWrapper<>();
wrapper.lambda().eq(BannerItem::getBannerId, id);
List<BannerItem> bannerItems = bannerItemMapper.selectList(wrapper);
```

###### LambdaQueryWrapper

为了简化lambda的使用，我们可以改写成LambdaQueryWrapper构造器，语法如下：

```Java
LambdaQueryWrapper<BannerItem> wrapper = new QueryWrapper<BannerItem>().lambda();
wrapper.eq(BannerItem::getBannerId, id);
List<BannerItem> bannerItems = bannerItemMapper.selectList(wrapper);

```

再次简化

```java
LambdaQueryWrapper<BannerItem> wrapper = new LambdaQueryWrapper<>();
wrapper.eq(BannerItem::getBannerId, id);
List<BannerItem> bannerItems = bannerItemMapper.selectList(wrapper);

```

###### 链式查询

```java
List<BannerItem> bannerItems = new LambdaQueryChainWrapper<>(bannerItemMapper)
                        .eq(BannerItem::getBannerId, id)
                        .list();
```

# lambda查询功能

一般写查询语句的时候，将wrapper直接作为参数去查询，例如

```java
 soaPublishWebhooktriggerService.remove(Wrappers.<SoaPublishWebhooktrigger>lambdaQuery().eq(SoaPublishWebhooktrigger::getPublishId,soaPublishId));

```



错误写法

```java
LambdaQueryChainWrapper<SoaPublishWebhooktrigger>     wrapper= soaPublishWebhooktriggerService.lambdaQuery().eq(SoaPublishWebhooktrigger::getPublishId,soaPublishId);
           
//构造完wrapper  再删除
soaPublishWebhooktriggerService.remove(wrapper);
```

