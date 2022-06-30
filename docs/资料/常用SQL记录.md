## 常用SQL记录

#### 按密码出现次数排序，并查询出现次数

```
SELECT password,
	count( 1 ) AS number 
FROM
	sys_account_info 
GROUP BY
	password 
ORDER BY
	count( 1 ) DESC
```

#### 查询数据库所有外键及启用情况

```
select name,is_disabled from sys.foreign_keys
```

#### 启用或禁用所有数据库外键

```
-- 禁用某个数据库的所有表的外键约束
EXEC sp_MSforeachtable @command1='alter table ? NOCHECK constraint all;'
-- 启用某个数据库的所有表的外键约束
EXEC sp_MSforeachtable @command1='alter table ? CHECK constraint all;'
```

#### 查询某个表的所有外键

```
SELECT
	fk.name,
	fk.object_id,
	OBJECT_NAME( fk.parent_object_id ) AS referenceTableName 
FROM
	sys.foreign_keys AS fk
	JOIN sys.objects AS o ON fk.referenced_object_id= o.object_id 
WHERE
	o.name= 'apss_light_application'
```

#### replace函数

```
-- 更新apss_light_application，替换entry_address字段中的192.16.1.144和192.16.1.151为10.235.68.12
UPDATE [dbo].[apss_light_application] 
SET entry_address = REPLACE( entry_address, '192.16.1.144', '10.235.68.12' );

UPDATE [dbo].[apss_light_application] 
SET entry_address = REPLACE( entry_address, '192.16.1.151', '10.235.68.12' );

-- 修改text 或 ntext类型数据
UPDATE [dbo].[apss_material_management_module] 
SET substance = REPLACE( cast(substance as varchar(max)), 'http://10.235.68.11:6001', 'https://wap.hzairport.com/simple-file-service' )
WHERE substance LIKE '%10.235.68.%';
```

#### 设置id插入允许

```
set identity_insert dbo.apss_material_management_module on
set identity_insert dbo.apss_material_management_module off
```

#### 修改列长度

```
-- 修改列 auth_code 长度为255
alter table [dbo].[membership_auth_record] alter column auth_code varchar(255)
```

#### 替换列内容

```
alter table [dbo].[apss_material_management_module] add substance_n ntext;
UPDATE [dbo].[apss_material_management_module] SET substance_n = substance;
alter table [dbo].[apss_material_management_module] DROP COLUMN substance;
-- 替换回来
exec sp_rename '[dbo].[apss_material_management_module].substance_n','substance','COLUMN'
```

#### MYSQL中group_concat长度限制！默认1024

```
1).在MySQL配置文件中加上
	group_concat_max_len = 102400 #你要的最大长度
2).可以简单一点，执行语句,可以设置作用范围
    SET GLOBAL group_concat_max_len=102400;
    SET SESSION group_concat_max_len=102400;
```

