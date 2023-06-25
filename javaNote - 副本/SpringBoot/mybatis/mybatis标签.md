

# 1.mybatis提取共用代码

mybatis项目dao层中很多sql语句都会拥有某些相同的查询条件，以<where><if test=""></if></where>的形式拼接在sql语句后，一个两个的sql语句感觉不到什么，但是如果查询语句特别多，但是查询的条件总是类似的，那就可以考虑把<where><if>这部分代码抽取出来，封装一下，然后需要条件搜索的sql语句直接引用就可以了。

　先来看下没有抽取代码之前的条件sql语句

```sql
第一条
<select id = "getUserEmailByProvinceAndOrderType" resultType="String">
        select DISTINCT(wo_responsibility) from t_view_workorder
        <where>
            <if test="province != '全国' and province != null">
                wo_province = #{province}
            </if>
            <if test="orderType != '全部' and orderType != null">
                and wo_type = #{orderType}
            </if>
            <if test="email != ''">
                and wo_responsibility = #{email}
            </if>
        </where>
</select>
第二条
<select id = "getUndoneDelayOrderByProvinceAndOrderTypeAndUserEmail" resultType="com.chinamobile.sias.workorder.po.Workorder">
        select * from t_view_workorder
        <where>
　　　　　　　　<if test="province != '全国' and province != null">
                wo_province = #{province}
            </if>
            <if test="orderType != '全部' and orderType != null">
                and wo_type = #{orderType}
            </if>
            <if test="email != ''">
                and wo_responsibility = #{email}
            </if>
　　　　　　　　<if test="true"> and (wo_complete_time is null or wo_complete_time='') and (select curdate()) >= wo_regulations_time </if>

 　　　　</where> 

</select>
```



以上是两条sql语句，可以看出，两个sql语句中有某些查询条件是相同的

```sql
  <if test="province != '全国' and province != null">
            wo_province = #{province}
        </if>
        <if test="orderType != '全部' and orderType != null">
            and wo_type = #{orderType}
        </if>
        <if test="email != ''">
            and wo_responsibility = #{email}
        </if>    
```





此时我们就可以对此段判断条件进行提取。如下：

```sql
<sql id="common_where_if">
        <if test="province != '全国' and province != null">
            wo_province = #{province}
        </if>
        <if test="orderType != '全部' and orderType != null">
            and wo_type = #{orderType}
        </if>
        <if test="email != ''">
            and wo_responsibility = #{email}
        </if>
</sql>
```

此时把<where>标签下相同的判断条件提去了出来，id自己取，这里定为 common_where_if.

那么如何使用这段代码呢，如下：

```sql
<include refid="common_where_if"/>
```

格式如下：

```sql
<select id = "getUserEmailByProvinceAndOrderType" resultType="String">
        select DISTINCT(wo_responsibility) from t_view_workorder
        <where>
            <include refid="common_where_if"/>
        </where>

</select>
```

> include也可以放在where标签外面





# resultmap和resultType

# param

>   param注解理解
>
> 参数传入到sql中，负责将这些参数和sql里的对应起来

只有一个参数的时候，加不加都行

```
        <where>
            <if test="basicLibraryId != null "> and bmi_basic_library.id = #{basicLibraryId}</if>
            and bmi_basic_library.deleted=1
        </where>
        像basicLibraryId只有这一个参数  所以mapper里的参数名字取什么都行
```







# parameterType

#  mybatis  判断参数不为为0的情况（详细见第七节）

```sql
  /**
if test 标签   用来指定条件查询    两个判断条件非空和非默认值  
！=''  非默认值        但是mybatis会把0当作默认值   state字段只有0和1两个值   所以如果加非默认值判断的话，程序就不会通过判断条件，不执行里面的sql
*/

当是数字0时，这个判断就会把0过滤掉，所以如果要判断数字，我们一般会再加上一个0的判断

  <if test="basicLibrary.state != null"> and bmi_basic_library.state =  #{basicLibrary.state}</if>
  
  或者
  <if test="basicLibrary.state != null and basicLibrary.state !='' or and basicLibrary.state ==0"> and bmi_basic_library.state =  #{basicLibrary.state}</if>
  
  

```

# 忽略数据库表中的字段  但是实体类要有属性

做了一模块涉及到连表查询，不需要保存到这个模块的表中

```java
@TableField(exist = false)：表示该属性不为数据库表字段，但又是必须使用的。

@TableField(exist = true)：表示该属性为数据库表字段。
```

# 关于根据条件连不同的表的sql

需要根据 moduletype的值 （soa、middleware、basicLibrary）去查询对应的模块版本关系表





我刚开始想着 case      when  语句去         连表                                 写了一大串 sql  还报错了

其实直接用if  test 语句判断就好了



## Mybatis的mapper.xml中if标签test判断的用法 



1. 字符串等于条件的两种写法

 ①将双引号和单引号的位置互换

```xml
<if test=' testString != null and testString == "A" '>
   AND 表字段 = #{testString}
</if>
```

② 加上.toString()

```xml
<if test=" testString != null and testString == 'A'.toString() ">
　　AND 表字段 = #{testString}
</if>
```

2. 非空条件的判断

```xml
<if test="xxx !=null and xxx !=''">
```

但是这样的判断只是针对String的，如果是别的类型，这个条件就不一定成立了，比如最经典的：当是数字0时，这个判断就会把0过滤掉，所以如果要判断数字，我们一般会再加上一个0的判断（这和[mybatis](https://so.csdn.net/so/search?q=mybatis&spm=1001.2101.3001.7020)的源码逻辑有关，有兴趣的可以去看看源码）

```xml
<if test="xxx !=null and xxx !='' or xxx == 0">
```

## 也可以用if语句判断

# 



# mybatis的坑点

除上面章节的0的问题  还有很多别的地方需要注意

- 

```
这里有必要再提一个“坑”，如果你有类似于String str ="A"; <if test="str!= null and str == 'A'">这样的写法时，你要小心了。因为单引号内如果为单个字符时，OGNL将会识别为Java 中的 char类型，显然String 类型与char类型做==运算会返回false，从而导致表达式不成立。解决方法很简单，修改为<if test='str!= null and str == "A"'>即可。
```

