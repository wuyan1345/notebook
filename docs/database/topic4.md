---
count: true
---

# Intermediate SQL

!!! tip
    ```sql
    SELECT a.name  
    FROM employees a  
    WHERE a.department_id IN (  
        SELECT b.department_id  
        FROM departments b  
        WHERE b.location_id IN (  
            SELECT c.location_id  
            FROM locations c  
            WHERE c.country_id = (  
                SELECT d.country_id  
                FROM countries d  
                WHERE d.country_name = 'USA'  
            )  
        )  
    ) 
    ``` 

## Joined Relations
+ 自然连接(Natural Join)
    + 如果要两张表自然连接（去除重复的列元素）
    ```sql
    select name,course_id
    from students,takes
    where students.id = takes.id
    ```
    等同于
    ```sql
    select name,course_id
    from students natural join takes
    ```
    + 相当于合并两张表并且去除相关的属性列。
+ 内连接(inner join)
    + 内连接基本与自然连接相同，不同之处在于自然连接的是同名属性列的连接，而内连接则不要求两属性列同名，可以用using或on来指定某两列字段相同的连接条件。
    + 使用 `using students.id` 或 `on students.id = takes.id` 限定合并的连接列，但是不同属性列仍会保留。
    ```sql
    select name,course_id
    from students inner join takes on students.id = takes.id
    ```
    !!! warning 
        mysql不支持全外连接，using和on的区别在于需要连接的两个表的属性名相同的时候使用using和on效果一样，而属性名不同的时候必须使用on。
+ 外连接(outer join)
    + 自然连接时某些属性值不同则会导致这些元组会被舍弃，而外连接正是处理这些元组的好方法。
    + left outer join
        + 保留左表中不同属性的值，在右表中填入对应的Null字段。
    + right outer join
    + full outer join

## Views
+ A view provides a mechanism to hide certain data from the view of certain users.
### View Definition
+ 定义语句: `create view v as < query expression >`
+ 例子：
```sql
create view faculty as
    select ID, name, dept_name
    from instructor
```
### Materialized Views
+ Physical copy created when the view is defined.
!!! note
    相较于虚拟视图的优缺点：

    + 更快的查询
    + 需要更多的存储空间
    + 代价更大的维护成本
        + 原相关表中的数据更新，对应的视图也应该更新
### Update of a view
+ 为了避免视图与原数据的定义冲突，尽量避免直接在视图中插入更新数据。
### Index Creation
+ 创建一个更便利的索引
```sql
create index studentID on student(ID)
```
### Constraints（限制）
+ Single Relation Constraints
    + not null
    + primary key
    + unique
    + check(P) , where P is a predicate
        + e.g. check(semester in ('Fall','Winter','Spring','Summer'))
+ Referential Integrity Constraints
    + cascade
        + 在父表上update/delete记录时，同步update/delete掉子表的匹配记录
    + set null
        + 在父表上update/delete记录时，将子表上匹配记录的列设为null (要注意子表的外键列不能为not null)
    + set default
        + 父表有变更时,子表将外键列设置成一个默认的值
+ Assertions
```sql
create assertion <assertion-name> check (<predicate>);
```

## Triggers
+ A trigger is a statement that is executed automatically by the system as a side effect of a modification to the database.
```sql
create trigger setnull_trigger before update of takes
referencing new row as nrow
for each row
    when (nrow.grade = '')
    begin atomic
        set nrow.grade = null;
end;
```
+ Triggers can serve a very useful purpose, but they are best avoided when alternatives exist.

## Authorization Specification in SQL
+ The grant statement is used to confer authorization
```sql
grant <privilege list>
on <relation or view> to <user list/public/role>
```

+ The revoke statement is used to revoke authorization