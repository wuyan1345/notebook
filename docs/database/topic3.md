---
count: true
---

# Introduction to SQL

!!! tip
    ' or if('manbo' like 'm%',sleep(114514),0)#

## SQL Data Definition
+ create table construct
    ```sql
    create table r(
        ID char(5),
        name varchar(20) not null,
        dept_name varchar(20),
        salary numeric(8,2),
        primary key (ID),
        foreign key (dept_name) references department);
    ```
    + 设置主键保证ID输入不能为空。
    + 定义`not null`保证name输入不能为空。
+ updates to tables
    + insert: `insert into instructor values('10211','Smith','Biology',66000);`
    + delete: `delete from student;`
    + drop table
    + Alter: `alter table r add A D;` or `alter table r drop A;`

## Basic Query Structure
+ select
    ```sql
    select A1,A2
    from r1,r2,...
    where P
    ```
    + P是约束条件
    + SQL names are case insensitive(不区分大小写)
    + 可以加入约束(distinct/all)
+ 选取后重定义
    + `select ID,name,salary/12 as monthly_salary`
+ 多张表的操作
    ```sql
    select distinct name, course_id
    from instructor, teaches
    where instructor.ID = teaches.ID
        and instructor.dept_name = 'Art'
        and instructor.salary > 70000
    ```
??? example "复杂示例"

    ```sql hl_lines="2"
    SELECT DISTINCT T.name
    FROM instructor AS T, instructor AS S
    WHERE T.salary > S.salary
    AND S.dept_name = "Computer";
    ```
    
    + 将 instructor 表赋值给T和S表
    + 比较两表中的工资高低
    + 找出所有工资比某个 Computer 部门的讲师更高的讲师的姓名