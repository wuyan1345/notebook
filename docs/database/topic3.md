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
    + 与下面的代码是等同的
    ```sql
    select name
    from instructor
    where salary > some (select salary
                        from instructor
                        where dept_name = "Computer")
    ```

+ 字符串操作
    + `Intro%` 匹配任意以 Intro 开头的字符串。
    + `_`匹配任意一个字符，因此 `_%` 会匹配任意一个长度大于等于1的字符串。

+ 对结果排序 `order by`
    + 按照姓名降序排列，未指定顺序时默认为升序(asc)

        ```sql hl_lines="3"
        select distinct name
        from instructor
        order by name desc
        ```

    + 可以指定多个排列顺序（即主顺序与次顺序）

+ 集合操作
    + 找到在2017年或2018年开课的课程 `A || B`:
        ```sql hl_lines="2"
        select course_id from section where year = 2017
        union
        select course_id from section where year = 2018
        ```
    + `A && B`: 使用`intersect`
    + `A && not B`: 使用`except`

+ 空值(Null Values)
    + 涉及空值的任何比较都返回 unknown
    + 在 where 语句里，unknown 会被视作 false

+ 计算数据特征
    + 可以使用如下函数 avg/min/max/sum/count，返回对一个多重数据的集合操作后得到的值。
    
    !!! example
        + 计算某个部门平均工资：
        ```sql 
        select avg(salary)
        from instructor
        where dept_name="Comp%"
        ```
        + 使用 `group by` 语句进行分类：
        ```sql 
        select dept_name,avg(salary) as avg_salary
        from instructor
        group by dept_name
        ```

        + 使用 `having` 追加条件：
        ```sql hl_lines="4"
        select dept_name,avg(salary) as avg_salary
        from instructor
        group by dept_name
        having avg(salary) > 42000
        ```
    
+ 一些集合操作
    + some
        + `5 < some (0,5,6)` 为真；
        + `5 = some (0,5,6)` 为真；
    + all
    + exists
    + unique

+ with 语句
    + find all departments with the maximum budget.
    ```sql hl_lines="1 2 3"
    with max_budget (value) as
        (select max(budget)
        from department)
    select department.name 
    from department,max_budget
    where department.budget = max_budget.value
    ```
    + 感觉相当于是在执行语句前建立一个新的临时表。

## Modification of the Database
+ 删除
    + 删除部分数据
    ```sql
    delete from instructor
    where salary < (select avg(salary)
                    from instructor)
    ```
+ 插入
    + 插入数据指定列
    ```mysql
    insert into course (course_id,title,dept_name,credits)
        values ('...','...','...',114514)
    ```
    !!! danger "误区"
        `insert into table1 select * from table1`是错误的，因为 MYSQL 无法在不重复使用源数据的情况下同时进行读取和写入。
+ 更新
    + 更新部分数据
    ```sql
    update instructor
    set salary = salary*1.05
    where salary < (select avg(salary)
                    from instructor)
    ```
    + 使用 case 语句进行多条件判断：
    ```sql
    update instructor
    set salary = case 
                when salary < (select avg(salary) from instructor) 
                then salary*1.05 else salary*1.03
                end
    ```