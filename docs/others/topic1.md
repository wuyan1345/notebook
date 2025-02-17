# 正则表达式

!!! abstract
    正则表达式在匹配某类型的字符串并进行检查或者替换是一个非常有用的工具，在CTF比赛中也极为常见，多出现在网页PHP代码审计和脚本sed指令绕过。

    学会它，这辈子有了（笑

## 简介
+ 基本正则表达式(BRE)是一组由字母和符号组成的特殊文本，它可以用来从文本中找出满足你想要的格式的句子，是一种从左到右匹配主体字符串的模式。
+ 还有一种正则表达式的扩展语法，对于sed命令来说要添加`-E`参数(Extension)，笔者曾在初学阶段将二者混淆不清😅，因此会在文章中会穿插介绍一下两者的具体区别,实际上大部分用的都是正则表达式的扩展语法(ERE)，包括下面这个网站，因为扩展语法其实更好理解。
+ 练习扩展正则表达式的[在线网站](https://regex101.com/)。

## 语法
### 基本匹配
+ 所有不是元字符的字符都可以直接写出来用来匹配。

    > REGULAR EXPRESSION : 234

    > TEST STRING : 1<font color="red">234</font>5

!!! warning
    BRE与ERE的区别就在于对元字符的转义处理不同，接下来介绍的语法以扩展正则表达式(ERE)为准，两者区别详见后文。

### 元字符
#### 点运算符.
+ 匹配任意单个非换行的字符。

    > REGULAR EXPRESSION : 2.4

    > TEST STRING : 1<font color="red">234</font>5

+ 如果想要匹配文本中的点，需要对输入的元字符进行转义，比如要匹配浮点数2.4，则对应的扩展正则表达式为`2\.4`。

#### 字符集[ ]
+ 方括号用来指定一个字符集，在方括号中使用连字符来指定字符集的范围（不关心字符顺序）。

    > REGULAR EXPRESSION : [Tt]he

    > TEST STRING : <font color="red">The</font> car is parked in <font color="red">the</font> garage.

+ 同点运算符，如果想要匹配文本中的中括号也需要对其进行转义。
+ 值得注意的是，如果想要在字符集中加入中括号或者点，则转义或者不转义的效果是相同的，代表真正的文本字符。因为字符集只匹配单个字符，所以在字符集中使用点运算符号或者继续嵌套字符集是没有意义的，即`[.[]]`与`[\.\[\]]`效果是相同的，匹配三种单个字符。

#### 否定字符集^ 与 顺序匹配 -
+ 在字符集中第一个字符若为^，则代表在方括号中使用连字符来指定不能在字符集的范围出现的字符。

    > REGULAR EXPRESSION : [^c]ar

    > TEST STRING : The car is <font color="red">par</font>ked in the <font color="red">gar</font>age.

+ `[x-y]`表示按顺序匹配从x到y的所有字符（ASCII码比较）。

    > REGULAR EXPRESSION : [a-z]ar

    > TEST STRING : The <font color="red">car</font> is <font color="red">par</font>ked in the <font color="red">gar</font>age.

+ 如果想在字符集中表示真正的文本^或-符号，则需要转义。
    
    > REGULAR EXPRESSION : [\^]

    > TEST STRING : 123<font color="red">^</font>456

#### 星号运算符* 、 加号运算符+ 与 问号运算符?
+ 星号运算符表示匹配在其之前的字符出现大于等于0次，加号运算符表示匹配在其之前的字符出现大于等于1次，问号运算符表示匹配在其之前的字符出现0次或1次。

    > REGULAR EXPRESSION : 2*3+

    > TEST STRING : <font color="red">23333333</font>2

    > REGULAR EXPRESSION : T?he

    > TEST STRING : <font color="red">The</font> car is parked in t<font color="red">he</font> garage.
    
+ 同理表示真正的星号加号问号需要转义。

#### 大括号{ }
+ 如果要匹配在其之前的字符出现等于特定次数或在某些次数范围内，上面三种字符就远远不够了，使用{x,y}代表出现大于等于x次小于等于y次（y可以为空表示无上限，大括号内可以只填一个数字代表等于某个特定次数）。

    > REGULAR EXPRESSION : [a-z]{4,}

    > TEST STRING : The car is <font color="red">parked</font> in the <font color="red">garage</font>.

+ 同理表示真正的大括号需要转义。