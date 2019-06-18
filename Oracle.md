### 基础复习

#### CRUD

###### create语句

```sql
create table student
(
    stuid varchar2(11) primary key，
    stuname varchar2(50) not null,
    sex  char(1) not null,
    enroldate date,--入学时间
)
-- 设置自增
create sequence stuid start with 1 increment by 1;   --建立了一个从1开始每次加1的序列。
```

###### insert语句

```sql
insert into student (字段名，默认所有字段) values (对应字段的值多个逗号隔开)
```

###### update语句

```sql
update student set stuname = '张三' where stuid = '1'
```

###### delete语句

```sql
delete from student where stuid = '1'
--删除所有数据,比delete快很多
trunchar table student
--删除表结构
drop table student
```

#### 复制数据

```sql
--在建表时复制
create table table_name as select column...|* from table_old
--在添加时复制
insert into table_new select column...|* from table_old
```

#### 约束

包含：非空约束、主键约束、外键约束、唯一约束、检查约束

数据字典`user_costraints`

##### 非空约束

```sql
--创建表的时候 添加 非空约束：在字段类型后添加 not null
--修改表的时候 添加 非空约束：
alter table table_name modify column_name datatype not null
--修改表的时候 去除 非空约束：
alter table table_name modify column_name datatype null
```

##### 主键约束

```sql
--一张表只能有一个主键，可以由一个或多个字段能构成，称为联合主键或者复合主键
-- 查看表的主键约束 可以查看 user_constraint 字典表
select constraint_name from user_constraint where table_name = ''
--创建表的时候设置 主键约束：在字段那类型后添加关键字 primary key
constraint 约束名字 primary key(一个或多个字段名称)
--修改表的时候设置 主键约束：
alter table table_name add constraint 约束名字 primary key(一个或多个字段名称)
--修改约束的名称
alter table table_name rename constraint old_name to new_name
--删除主键约束
alter table table_name disable(禁用约束)|enable(启用约束) constraint 约束名字
--直接删除约束，不禁用
alter table table_name drop constraint 约束名字
alter table table_name drop primary key[cascade] -- cascade 级联删除，删除其余有关联的约束
```

##### 外键约束

```sql
--创建时添加外键约束
-- 在类型后 添加 关键字 references table_name(字段名), 关键字后的是主表，前面的是从表，主表中的字段 必须是主键，主从表相应的字段必须是同一数据类型
-- 方法二
constraint 外键名称 foreign key(字段名) references table_name(字段名)[on delete cascade]--级联删除
-- 在修改表的时候添加外键约束
alter table table_name add constraint 外键名称 foreign key(字段名) references table_name(字段名)[on delete cascade]
-- 删除外键约束
-- 方法一:禁用
alter table table_name disable(禁用约束)|enable(启用约束) constraint 约束名字
-- 方法二:删除约束
alter table table_name drop constraint 约束名字
```

##### 唯一约束

```sql
--唯一约束与主键约束的区别：主键非空，唯一允许有一个可为空；主键只能有一个，唯一约束可以有多个
-- 创建表的时候设置唯一约束
-- 在字段类型后添加关键字 unique
constraint 约束名字 unique(字段名称)
-- 修改表的时候设置唯一约束
alter table table_name add constraint 外键名称 unique(字段名)
-- 删除唯一约束
-- 方法一:禁用
alter table table_name disable(禁用约束)|enable(启用约束) constraint 约束名字
-- 方法二:删除约束
alter table table_name drop constraint 约束名字
```

##### 检查约束

```sql
-- 创建表的时候设置检查约束
-- 在字段类型后添加关键字 check(约束条件，可为表达式)
constraint 约束名字 check(约束条件)
-- 修改表的时候设置检查约束
alter table table_name add constraint 约束名称 check(约束条件)
-- 删除检查约束
-- 方法一:禁用
alter table table_name disable(禁用约束)|enable(启用约束) constraint 约束名字
-- 方法二:删除约束
alter table table_name drop constraint 约束名字
```



#### 用户和表空间

```sql
--查看当前登录用户
show user
--系统用户表
desc dba_users
--表空间分为:永久表空间，临时表空间，UNDO表空间
--dba_tablespaces、user_tablespaces数据字典
--dba_users、user_users数据字典
```

### 常用函数及知识点

- select 中 去重 关键字 `distinct`

- decode(colunm_name, value1, result1, ..., defaultvalue)

  ```sql
  select decode(stuname, 'admin', '管理员', 'teacher', '教师', '其他用户') from student
  ```

- `wm_concat` 行转列

- `NVL(obj, return)` 替换空值

- having 子句过滤分组的条件，与where的区别：不能在where中使用分组函数，可以在having中使用；

  应该优先使用where 后使用having，因为where是先过滤后分组，having是先分组后过滤

- where 必须在 group by 之前，order by 在group by之后

- group by rollup(a,b) == group a,b + group by a + group by b,比较适用于报表统计

- Oracle中的四种连接方式：等值连接 ，不等值连接，外连接，自连接

  - `左外连接`当连接条件不成立的时候，等号左边的表依然被包含e.id=b.id(+)
  - `右外连接`当连接条件不成立的时候，等号右边的表依然被包含e.id(+)=b.id
  - `自连接`通过别名把一张表视为多张表

- group by之后不可使用子查询

- 