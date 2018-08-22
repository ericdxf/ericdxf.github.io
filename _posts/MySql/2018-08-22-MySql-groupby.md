---
layout: post
title: GROUP BY小记
category: MySql
---
### 概述
“Group By”从字面意义上理解就是根据“By”指定的规则对数据进行分组，所谓的分组就是将一个“数据集”划分成若干个“小区域”，然后针对若干个“小区域”进行数据处理。  
下面的表是原始数据，接下来的例子都是基于这张表演示的。
![test](https://github.com/ericdxf/ericdxf.github.io/raw/master/assets/images/git_new_repository.png)
***
1. 第一行
2. 第二行
3. 第三行
`` 包裹起来 ``
1、CentOS安装MySql8.0之后，部分配置和之前的5.7有比较大的差异，在这里整理一下  
使用的安装方式是rpm安装
```SQL
#wget http://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm  
rpm -ivh mysql80-community-release-el7-1.noarch.rpm  
```

```css
body{font-size:12px}
```