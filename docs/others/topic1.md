---
count: true
---

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

+ 在星号运算符和加号运算符后追加问号表示非贪婪匹配。
  
    > REGULAR EXPRESSION : 2+3*?

    > TEST STRING : <font color="red">2</font>3333333<font color="red">2</font>

+ 同理表示真正的星号加号问号需要转义。

#### 大括号{ }
+ 如果要匹配在其之前的字符出现等于特定次数或在某些次数范围内，上面三种字符就远远不够了，使用{x,y}代表出现大于等于x次小于等于y次（y可以为空表示无上限，大括号内可以只填一个数字代表等于某个特定次数）。

    > REGULAR EXPRESSION : [a-z]{4,}

    > TEST STRING : The car is <font color="red">parked</font> in the <font color="red">garage</font>.

+ 同理表示真正的大括号需要转义。

#### 或运算符 |
+ 或运算符用作判断条件使用，优先级最低。

    > REGULAR EXPRESSION : (T|t)he|car

    > TEST STRING : <font color="red">The</font> <font color="red">car</font> is parked in <font color="red">the</font> garage.

+ 同理表示真正的或运算符需要转义。

#### 锚点 ^与$
+ ^用来匹配指定句子开头（与在字符集[ ]中的否定是同一个符号），$用来匹配指定句子结尾。

    > REGULAR EXPRESSION : ^(T|t)he|[a-z.]*$

    > TEST STRING : <font color="red">The</font> car is parked in the <font color="red">garage.</font>

+ 同理表示真正的^或$运算符需要转义。

#### 分组/捕获组/锚点断言 小括号( )
+ 使用小括号指定一个分组，用或运算符分隔。
+ 小括号也是捕获分组，将括号内匹配的内容进行缓存（详见后文的sed指令用法），若不想进行缓存则使用`(?:)`进行匹配。
+ 前面有匹配句子开头与结尾的锚点，也可以通过小括号来规定自己想要的锚点，因为锚点只匹配位置不匹配内容，也叫零宽断言（前后预查）。
    + 先行断言和后行断言的四种形式:

        |语法|名称|规则|
        |--|--|--|
        |(?=pattern)|零宽正向先行断言|匹配后面可以匹配pattern的位置|
        |(?!pattern)|零宽负向先行断言|匹配后面无法匹配pattern的位置|
        |(?<=pattern)|零宽正向后行断言|匹配前面可以匹配pattern的位置|
        |(?<!pattern)|零宽负向后行断言|匹配前面无法匹配pattern的位置|
    
    + 例子：

        <img src="../1.png" style="max-width: 80%; height: auto;">

### 简写字符集
+ 正则表达式提供一些常用的字符集简写。

|简写|描述|
|--|--|
|\w|匹配所有字母数字，等同于[a-zA-Z0-9_]|
|\W|匹配所有非字母数字，等同于[^\w]|
|\d|匹配数字，等同于[0-9]|
|\D|匹配非数字，等同于[^\d]|
|\s|匹配所有空格字符，等同于[ \f\n\r\t\v]|
|\S|匹配所有非空格字符，等同于[^\s]|
|\p|匹配DOS行终止符，等同于[\r\n]|
|\cx|匹配由 x 指明的控制字符，x 必须属于 [a-zA-Z]，否则 \c 直接视为 c，如 \cM 匹配 Ctrl-M 即回车符|

### 修饰符
+ 修饰符不属于表达式的内容，但是指定了匹配的规则，js 中的正则写法为 /pattern/flags，其中 flags 就是修饰符。

    |标志|描述|
    |--|--|
    |i|匹配忽略大小写|
    |g|全局匹配(Don't return after first match)|
    |m|多行匹配，使 ^$ 匹配每行的开头和结尾|
    |s|单行匹配，使 . 可以匹配换行符|

??? note "BRE与ERE的区别"
    + 在BRE中，元字符是需要转义后再使用的，而与元字符冲突的正常字符不需要转义（但是*是意外？）。
    + 在ERE中，元字符不需要转义便可以使用，而与元字符冲突的正常字符需要转义。
    + 因此如下两条指令执行的结果是相同的：
        ```shell hl_lines="1 3" title="diff"
        echo "aaabb(ab)bbcccc" | sed "s/b\+(ab)/_/g"
        res: aaa_bbcccc
        echo "aaabb(ab)bbcccc" | sed -E "s/b+\(ab\)/_/g"
        res: aaa_bbcccc
        ```

## 相关应用

### sed指令替换
+ sed指令有多种模式，这里只讨论`s/pattern/replace/flags`，即使用 pattern 扫描整个 string，将符合匹配的替换成 replace，flags 为修饰符。
!!! tip 
    + 笔者曾需要将 markdown 文件中复杂的图片名称重新命名，因此接触到了 sed 命令。在 markdown 文件中的图片引用主要有两种形式（以 png 图片为例，提取图片名后缀也可以用正则表达式）：
        + `<img ... src="oldname.png" ...>`
        + `![...](oldname.png)`
        + 这其中省略部分都是要原样保留的，因此要使用小括号的捕获组特性进行保留。
    + 需要将图片的`oldname`替换为`newname`。
    + 正则表达式如下(因为bash会执行!所以也要转义)：

        ```shell hl_lines="1 3" title="bash"
        echo "<img alt='test' src='oldname.png' width=50px>" | sed -E "s/<img ([^>]*)src='[^\']*'([^>]*)>/<img \1src='newname.png'\2>/g"
        res: <img alt='test' src='newname.png' width=50px>
        echo "\![alt text](oldname.png)" | sed -E "s/\!\[([^]]*)\]\(oldname.png\)/\![\1](newname.png)/g"
        res: ![alt text](newname.png)
        ```

### 其他
+ Python的re模块
    + re.match(pattern, string, flags=0)：使用 pattern 从头匹配 string，flags 为修饰符。
    + re.search(pattern, string, flags=0)：使用 pattern 扫描整个 string，返回第一个匹配的 re.Match。
    + re.sub(pattern, repl, string, count=0, flags=0)：使用 pattern 扫描，将匹配串替换成repl。
    + re.findall(pattern, string, flags=0)：在 string 中查找所有匹配 pattern 的结果，返回列表。
    + re.compile(pattern, flags=0)：编译一个正则表达式，返回一个 re.Pattern。
+ php的`preg_match()`函数