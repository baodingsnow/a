 join查询

tablefield

resultMap是结果集映射的配置标签

在深入ResultMap标签前，我们需要了解从SQL查询结果集到[JavaBean](https://so.csdn.net/so/search?q=JavaBean&spm=1001.2101.3001.7020)或POJO实体的过程。

typeHandler

## SQL LEFT JOIN 关键字

LEFT JOIN 关键字会从左表 (table_name1) 那里返回所有的行，即使在右表 (table_name2) 中没有匹配的行。

```sql
SELECT column_name(s)
FROM table_name1
LEFT JOIN table_name2
ON table_name1.column_name=table_name2.column_name
```

```sql
SELECT  bmi_basic_library.id,
bmi_basic_library.library_name,
bmi_basic_library.latest_version_id,
bmi_basic_library_version.remark,
bmi_basic_library_version.is_os_necessary,
bmi_basic_library_version.function_description,
bmi_basic_library.update_time,
bmi_basic_library.create_time,
bmi_basic_library_version.serial_number
FROM  bmi_basic_library 
LEFT JOIN  bmi_basic_library_version
ON  bmi_basic_library.latest_version_id=bmi_basic_library_version.id
```





```sql
SELECT bmi_basic_library_version_tag.tag_id,bmi_tag.tag_name
FROM bmi_basic_library_version_tag
LEFT  JOIN bmi_tag
ON bmi_basic_library_version_tag.id=bmi_tag.id
```

LEFT JOIN 关键字会从左表 (table_name1) 那里返回所有的行，即使在右表 (table_name2) 中没有匹配的行。

## LEFT JOIN 关键字语法

```
SELECT column_name(s)
FROM table_name1
LEFT JOIN table_name2
ON table_name1.column_name=table_name2.column_name
```

**注释：**在某些数据库中， LEFT JOIN 称为 LEFT OUTER JOIN。

## 原始的表 (用在例子中的)：

"Persons" 表：

| Id_P | LastName | FirstName | Address        | City     |
| :--- | :------- | :-------- | :------------- | :------- |
| 1    | Adams    | John      | Oxford Street  | London   |
| 2    | Bush     | George    | Fifth Avenue   | New York |
| 3    | Carter   | Thomas    | Changan Street | Beijing  |

"Orders" 表：

| Id_O | OrderNo | Id_P |
| :--- | :------ | :--- |
| 1    | 77895   | 3    |
| 2    | 44678   | 3    |
| 3    | 22456   | 1    |
| 4    | 24562   | 1    |
| 5    | 34764   | 65   |

## 左连接（LEFT JOIN）实例

现在，我们希望列出所有的人，以及他们的定购 - 如果有的话。

您可以使用下面的 SELECT 语句：

```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
From  Persons
LEFT  JOIN   Orders
ON  Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```



结果集：

| LastName | FirstName | OrderNo |
| :------- | :-------- | :------ |
| Adams    | John      | 22456   |
| Adams    | John      | 24562   |
| Carter   | Thomas    | 77895   |
| Carter   | Thomas    | 44678   |
| Bush     | George    |         |

LEFT JOIN 关键字会从左表 (Persons) 那里返回所有的行，即使在右表 (Orders) 中没有匹配的行。

# mybatisplus 编写复杂sql

### 标签

####    resultMap是结果集映射的配置标签

```sql
<select id="getStudent" resultMap="getStudentRM">
  SELECT ID, Name, Age
    FROM TStudent
</select>
<resultMap id="getStudentRM" type="EStudnet">
  <id property="id" column="ID"/>
  <result property="studentName" column="Name"/>
  <result property="studentAge" column="Age"/>
</resultMap>
```

#### 子元素说明：

- id元素 ，用于设置主键字段与领域模型属性的映射关系
- result元素 ，用于设置普通字段与领域模型属性的映射关系

id、result语句属性配置细节：

| 属性        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| property    | 需要映射到JavaBean 的属性名称。                              |
| column      | 数据表的列名或者标签别名。                                   |
| javaType    | 一个完整的类名，或者是一个类型别名。如果你匹配的是一个JavaBean，那MyBatis 通常会自行检测到。然后，如果你是要映射到一个HashMap，那你需要指定javaType 要达到的目的。 |
| jdbcType    | 数据表支持的类型列表。这个属性只在insert,update 或delete 的时候针对允许空的列有用。JDBC 需要这项，但MyBatis 不需要。如果你是直接针对JDBC 编码，且有允许空的列，而你要指定这项。 |
| typeHandler | 使用这个属性可以覆写类型处理器。这项值可以是一个完整的类名，也可以是一个类型别名。 |

#### **collection聚集**







```
基础库名称 bmi_basic_library      library_name
* 版本号: bmi_basic_library_version       version_number
描述      bmi_basic_library_version     function_description
标签      bmi_tag     tag_name
OS必备库  bmi_basic_library_version      is_os_necessary
备注    bmi_basic_library_version       remark

```





```SQL
/**
* 首先 basic——library表里添加 library name   会自动生成id
将id  传给library——version 里的version_number    library_version自动生成id
tag表里 新建 标签名字  自动生成 id(tag_id)
version_tag表里添加library_id library_version_id  tag_id 自动生成id
/
```



# WHERE 和HAVING 的区别

> “Where” 是一个约束声明，使用Where来约束来之[数据库](http://lib.csdn.net/base/mysql)的数据，Where是在结果返回之前起作用的，且Where中不能使用聚合函数。
>
> “Having”是一个过滤声明，是在查询返回结果集以后对查询结果进行的过滤操作，在Having中可以使用聚合函数。







# SELECT IF 查询

~~~sql
```
IF( expr1 , expr2 , expr3 )
```

expr1 的值为 TRUE，则返回值为 expr2 
expr1 的值为FALSE，则返回值为 expr3
~~~

