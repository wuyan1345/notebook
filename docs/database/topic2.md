---
count: true
---

# Intro to Relational Model

!!! tip
    1';CREATE TABLE s(a text);--

## Relation Schema and Instance

+ Attributes
    + required to be atomic
    + 属性的取值集合称作域(domain)
    + 空值为Null
+ schema
    + logical structure of the database
+ instance
    + a snapshot of the data in the database at a given instant in time
??? example
    <img src="../5.png" style="max-width: 80%; height: auto;">

## Keys
+ superkey
    + values for K are sufficient to identify a unique tuple of each possoble relation
    + e.g. {ID}/{ID,name}
+ candidate key
    + superkey K is a candidate key if K is minimal
    + e.g. {ID}
!!! note 
    如果超键只有两个K1={A,B}，K2={B,C,D}，两者都是候选键。
+ primary key
    + choose one from candidate key
+ foreign key
    + value in one relation must appear in another

## Relational Query Languanges

+ Relational Algebra
    + 6种基本算子

        |名称|符号|用法|
        |--|--|--|
        |select|$ \sigma $|$ \sigma_p(r) $，p是选择条件|
        |project|$ \prod $|$\prod_{A1,A2...}(r)$，相当于挑选列进行重组|
        |union|$ \cup $|$ A\cup B$，兼容关系合并|
        |set difference|$ - $|$ A-B$，兼容关系筛减|
        |cartesian product（笛卡尔乘积）|$ \times $|$R\times S$，两种关系组合成一种关系形成一个新的表|
        |rename|$ \rho $|$ \rho_{S(A1,A2...)}(r)$，将关系与属性重命名|
    
    + 连接算子$ \theta $
        + 连接运算从R和S的笛卡尔积$R\times S$中选取（R关系）在A属性组上的值与(S关系)在B属性组上值满足比较条件的元组。
        + 多种连接算子[详解](https://blog.csdn.net/Candle_light/article/details/84424034)
    
    + 交算子可以用并算子和差算子表示，$ A \cap B=A-((A \cup B)-B)$。

    + 赋值算子($ \leftarrow $)

+ Equivalent Queries
    + 有些查询是等价的，也可以作为优化的思路。