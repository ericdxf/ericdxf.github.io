---
layout: post
title: GROUP BY小记
category: MySql
---
### 概述
* “Group By”从字面意义上理解就是根据“By”指定的规则对数据进行分组，所谓的分组就是将一个“数据集”划分成若干个“小区域”，然后针对若干个“小区域”进行数据处理。
* 下面的表是原始数据，接下来的例子都是基于这张表演示的。
![test](/assets/images/MySql/mysql_group_by_1.png)

***
### 简单GROUP BY
select 类别, sum(数量) as 数量之和
from A
group by 类别
返回结果如下表，实际上就是分类汇总。

```css
body{font-size:12px}
```