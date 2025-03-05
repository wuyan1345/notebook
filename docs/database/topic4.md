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
    + 使用 `using students.id` 或 `on students.id = takes.id` 限定合并的连接列。
+ 外连接(outer join)
+ 内连接(inner join)