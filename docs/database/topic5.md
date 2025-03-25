---
count: true
---

# Advanced SQL

!!! tip
    select benchmark( 5000000, md5( 'test' ));​


## Accessing SQL From a Programming Language
+ Non-declarative（非声明式） actions cannot be done from within SQL.
+ Two approaches to access SQL from a gereral-purpose programming language
    + Dynamic SQL: Can connect to and communicate with a database server using a collection of functions.
        - ODBC/JDBC 控制C和Java的API接口
    + Embedded SQL: provides a means by which a program can interact with a database server.
        - SQLJ
### JDBC
+ 数据更新: `stmt.executeUpdate`
+ 数据查询：`stmt.executeQuery`
+ 提取结果：`rs.getString`
    + 是否为空 `rs.wasNull`
+ 提前准备声明：
```java
PreparedStatement pStmt = conn.prepareStatement(
        "insert into instructor values(?,?,?,?)");
pStmt.setString(1, "88877");
pStmt.setString(2, "Perry");
pStmt.setString(3, "Finance");
pStmt.setInt(4, 125000);
pStmt.executeUpdate();
pStmt.setString(1, "88878");
pStmt.executeUpdate();
```
    + 不要使用字符串拼接，如果输入数据中存在引号会造成错误
+ SQL 注入
+ Metadata features
!!! example "Example for JDBC"
    ```java
    public static void JDBCexample(String dbid, String userid, String passwd){
        try (Connection conn = DriverManager.getConnection(
            "jdbc:oracle:thin:@db.yale.edu:2000:univdb", userid, passwd);
            Statement stmt = conn.createStatement();
        ）{
            … Do Actual Work …
            ResultSet rset = stmt.executeQuery(
                    "select dept_name, avg (salary)
                    from instructor
                    group by dept_name");
            while(rset.next()){
                System.out.printIn(rset.getString("dept_name") + " " + rset.getFloat(2));
            }
        }
        catch (SQLException sqle){
            System.out.printIn("SQLException:" + sqle);
        }
    }
    ```

### Embedded SQL
+ 语句格式 `EXEC SQL <embedded SQL statement >`
+ 相当于内嵌在代码之中
```
EXEC SQL
    declare c cursor for
    select ID, name
    from student
    wheretot_cred>:credit_amount
END_EXEC
```

## Functions and Procedures
+ 类似于代码中的自定义函数
```sql
create function dept_count (dept_name varchar(20))
    returns integer
    begin
        declare d_count integer;
        select count (*) into d_count
            from instructor
            where instructor.dept_name = dept_name
    return d_count;
end
```
+ 过程函数相当于在函数声明时定义返回值
```sql
create procedure dept_count_proc (in dept_name varchar(20), out d_count integer)
begin
    select count(*) into d_count
        from instructor
        where instructor.dept_name = dept_count_proc.dept_name
end
```