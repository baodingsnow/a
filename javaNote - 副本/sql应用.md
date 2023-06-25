```sql
SELECT  student.*  from student //返回student表全部数据，多表查询需要返回全量查询的时候用到
/**
分组查询  比如学生成绩表有多个科目的查询 用分组查询可以把学生名字相同的分数归类成一组并相加
分组查询  涉及到聚合函数和原表字段同时出现时 就要用到group by      
*/
SELECT Customer,SUM(OrderPrice) FROM Orders
GROUP BY Customer

/**
查出最大值
*/
select s_id , max(s_score) AS MAXIMUM 
FROM score 
GROUP BY s_id  


HAVING类似于WHERE（唯一的差别是WHERE过滤行，HAVING过滤组）HAVING支持所有WHERE操作符。
/**
 * 、查询平均成绩大于等于60分的同学的学生编号和学生姓名和平均成绩
 */

select s.s_id ,s.s_name  ,round(avg(s2.s_score),2) as avg
from   student s 
left join score s2 
on s.s_id =s2.s_id 
group by s .s_name having  avg >60  


UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

SQL查询不重复记录方法  distinct

case函数  是一种多分支的函数，可以根据条件列表的值返回多个可能的结果表达式中的一个。
可用在任何允许使用表达式的地方，但不能单独作为一个语句执行。
分为：
简单CASE函数
搜索CASE函数


```





```sql
/**
 * 查询01课程比02课程成绩高的学生信息及课程分数
*/

select a.* ,b.s_score as 02_score,c.s_score as 03_score from 
student a 
	join score b on a.s_id=b.s_id and b.c_id='01'
	left join score c on a.s_id=c.s_id and c.c_id='02' where b.s_score>c.s_score

	
	SELECT  student.*  from student

	
/**
 *查询平均成绩大于等于60分的同学的学生编号和学生姓名和平均成绩
*/
select student.s_id,student.s_name,round(avg(score.s_score),2)as avg_score from 
student 
join score on student.s_id =score.s_id 
group by s_id,s_name 
having avg_score >=60


/**
子查询快捷
*/
select  device_id,gender,age,university,gpa  from user_profile
where  device_id in(select device_id from user_profile where gpa>3.5 and university ='山东大学')
or     device_id in(select device_id from user_profile  where>GPA>3.8 and university= '厦门大学') 


/**
case查询
*/
SELECT
  CASE
    WHEN age < 25
    or age is null then '25岁以下'
    WHEN age >= 25 then '25岁及以上'
  end age_cut,
  count(*) number
from
  user_profile
group by
  age_cut
  
```

# 问题

```sql
select s_id,c_id,min(s_score) as mINimum 
from score 
group by c_id;
```

```
 like concat('%', #{soa.searchTag}, '%')
```

like



```


一般形式为：

列名 [NOT ] LIKE

匹配串中可包含如下四种通配符：
_：匹配任意一个字符；
%：匹配0个或多个字符；
[ ]：匹配[ ]中的任意一个字符(若要比较的字符是连续的，则可以用连字符“-”表 达 )；
[^ ]：不匹配[ ]中的任意一个字符。
```

```java
   private void parseConfigJson(Long someIpUploadId, Long exeConfigUploadId, Soa soa, SoaVersion soaVersion) {
        String someIpJsonString = null;
        try {
            someIpJsonString = fileInfoService.getFileStringContentById(someIpUploadId);
            parseSomeIpConfig(soa, soaVersion, someIpJsonString);
        } catch (Exception e) {
            log.error(e.getMessage());
        }
        String exeConfigString = null;
        try {
            exeConfigString = fileInfoService.getFileStringContentById(exeConfigUploadId);
            parseExeConfigJson(soa, soaVersion, exeConfigString);
        } catch (Exception e) {
            log.error(e.getMessage());
        }
```











```
    public TableDataInfo<ModuleOperLog> list(ModuleOperLog moduleOperLog){
        startPage();
        List<ModuleOperLog> list = moduleOperLogService.selectModuleOperLogList(moduleOperLog);
        return getDataTable(list);
    }
```



