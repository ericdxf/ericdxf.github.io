### 概述
* “Group By”从字面意义上理解就是根据“By”指定的规则对数据进行分组，所谓的分组就是将一个“数据集”划分成若干个“小区域”，然后针对若干个“小区域”进行数据处理。
* 下面的表是原始数据，接下来的例子都是基于这张表演示的。
![原始表](/assets/images/MySql/mysql_group_by_1.png)

***
### 简单Group By
```sql
select 类别, sum(数量) as 数量之和  
from A  
group by 类别  
```
返回结果如下表，实际上就是分类汇总。  
![简单GROUP BY](/assets/images/MySql/mysql_group_by_2.png)
### Group By 和 Order By
```sql
select 类别, sum(数量) AS 数量之和
from A
group by 类别
order by sum(数量) desc
```
返回结果如下表  
![添加 Order By](/assets/images/MySql/mysql_group_by_3.png)
### Group By All
```sql
select 类别, 摘要, sum(数量) as 数量之和
from A
group by all 类别, 摘要
```
这种方式可以多指定一个列，多个列都相同才会被分到同一组。  
SQL中多指定“摘要”字段，其原因在于“多列分组”中包含了“摘要字段”，其执行结果如下表  
![Group By All](/assets/images/MySql/mysql_group_by_4.png)
### Group By与聚合函数
![Group By All](/assets/images/MySql/mysql_group_by_8.jpg)
### Having与Where的区别
* where 子句的作用是在对查询结果进行分组前，将不符合where条件的行去掉，即在分组之前过滤数据，where条件中不能包含聚组函数，使用where条件过滤出特定的行。
* having 子句的作用是筛选满足条件的组，即在分组之后过滤数据，条件中经常包含聚组函数，使用having 条件过滤出特定的组，也可以使用多个分组标准进行分组。
```sql
select 类别, sum(数量) as 数量之和 from A
group by 类别
having sum(数量) > 18
```
上面的SQL语句执行时会正确筛选出接过来，如果吧having替换成where则会报错
### Compute
```sql
select *
from A
where 数量>8
compute max(数量),min(数量),avg(数量)
```
执行效果如下  
![Compute](/assets/images/MySql/mysql_group_by_6.png)
### Compute By
```sql
select *
from A
where 数量>8
order by 类别
compute max(数量),min(数量),avg(数量) by 类别
```
执行效果如下  
![Compute](/assets/images/MySql/mysql_group_by_7.png)