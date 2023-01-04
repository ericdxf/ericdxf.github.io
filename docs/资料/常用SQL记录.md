## 常用SQL记录

#### 按密码出现次数排序，并查询出现次数

```sql
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

```sql
select name,is_disabled from sys.foreign_keys
```

#### 启用或禁用所有数据库外键

```sql
-- 禁用某个数据库的所有表的外键约束
EXEC sp_MSforeachtable @command1='alter table ? NOCHECK constraint all;'
-- 启用某个数据库的所有表的外键约束
EXEC sp_MSforeachtable @command1='alter table ? CHECK constraint all;'
```

#### 查询某个表的所有外键

```sql
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

```sql
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

```sql
set identity_insert dbo.apss_material_management_module on
set identity_insert dbo.apss_material_management_module off
```

#### 修改列长度

```sql
-- 修改列 auth_code 长度为255
alter table [dbo].[membership_auth_record] alter column auth_code varchar(255)
```

#### 替换列内容

```sql
alter table [dbo].[apss_material_management_module] add substance_n ntext;
UPDATE [dbo].[apss_material_management_module] SET substance_n = substance;
alter table [dbo].[apss_material_management_module] DROP COLUMN substance;
-- 替换回来
exec sp_rename '[dbo].[apss_material_management_module].substance_n','substance','COLUMN'
```

#### MYSQL中group_concat长度限制！默认1024

```yml
1).在MySQL配置文件中加上
	group_concat_max_len = 102400 #你要的最大长度
2).可以简单一点，执行语句,可以设置作用范围
    SET GLOBAL group_concat_max_len=102400;
    SET SESSION group_concat_max_len=102400;
```

#### SqlServer设置主键自增

```sql
ALTER TABLE dbo.psms_flight_airport DROP COLUMN id;

ALTER TABLE dbo.psms_flight_airport ADD id BIGINT IDENTITY ( 1, 1 ) NOT NULL;

ALTER TABLE dbo.psms_flight_airport ADD CONSTRAINT prim_Id PRIMARY KEY ( id );
```

#### SqlServer查询唯一键

```sql
--查询唯一约束
SELECT
  tab.name AS [表名],
  idx.name AS [约束名称],
  col.name AS [约束列名]
FROM
  sys.indexes idx
    JOIN sys.index_columns idxCol
      ON (idx.object_id = idxCol.object_id
          AND idx.index_id = idxCol.index_id
          AND idx.is_unique_constraint = 1)
    JOIN sys.tables tab
      ON (idx.object_id = tab.object_id)
    JOIN sys.columns col
      ON (idx.object_id = col.object_id
          AND idxCol.column_id = col.column_id)
WHERE tab.name = 'apss_light_app_order'
```

#### 添加删除唯一约束

```sql
-- 添加唯一约束
-- alter table 表名 add constraint 约束名称 UNIQUE(字段1,字段2)
alter table apss_light_app_order add constraint UK_8ivvekyuhbgw5cge6m6mnrt00 UNIQUE('lapp_id')
-- 删除唯一约束
alter table apss_light_app_order DROP constraint UK_8ivvekyuhbgw5cge6m6mnrt92
```

