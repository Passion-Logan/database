### 基础复习

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

~~~sql
update student set stuname = '张三' where stuid = '1'
~~~

###### delete语句

~~~sql
delete from student where stuid = '1'
--删除所有数据,比delete快很多
trunchar table student
--删除表结构
drop table student
~~~

### 用户和表空间



### 常用语句

