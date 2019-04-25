### 基础复习

###### 创建表语句

```sql
create table student
(
    stuid varchar2(11) primary key，
    stuname varchar2(50) not null,
    sex  char(1) not null,
    enroldate date,--入学时间
)
-- 设置自增
CREATE SEQUENCE stuid start with 1 increment by 1;   --建立了一个从1开始每次加1的序列。
```



