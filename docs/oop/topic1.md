# Introduction

!!! tip
    system("ls -al /etc/passwd /etc/shadow");

## 输入输出
```c++
#include <iostream>
using namespace std;
int main()
{  
    int age;
    int sid;
    cin >> age >> sid;
    cout << "Hello, World! I am " << age << " Today!" << endl;  
    return 0;
}
```

+ cout: 标准输出流（副作用是字符串被输出）
+ cin: 标准输入流（副作用是读入，读取到空格为止）
+ 开头`using namespace std`使得`std::cout`可以简写为`cout`

## 字符串(String)
+ 导入类：`#include <string>`
+ 定义：`string str="Hello"`
+ `string str`此时字符串变量初始值为空字符串
+ 使用类的`.length()`方法计算字符串的长度

### Assignment for string
```c++
char charr1[20];
char charr2[20] = "jaguar"; 
string str1;
string str2 = "panther"; 
charr1 = charr2; // illegal 
str1 = str2; // legal
```

+ 字符数组不能赋值，字符串可以赋值。

### Other Member Functions
+ `substr(int pos, int len)`
    + 拷贝字符串从 pos 位置开始的 len 个字符
    + 如果 pos 超出了字符串长度，那么会产生异常；
    + 如果 pos 等于字符串长度，那么会得到空字符串；
    + 如果 pos+len 超出了字符串长度，那么只会拷贝到字符串末尾。
+ alter string
    + assign 将一个新的值赋给字符串
    + insert 在 pos 之前插入字符
        ```c++
        insert (size_t pos, const string& str);
        insert (size_t pos, const string& str, size_t subpos, size_t sublen);
        insert (size_t pos, const char* s);
        insert (size_t pos, const char* s, size_t n);
        insert (size_t pos, size_t n, char c);
        ```
    + `erase (size_t pos = 0, size_t len = npos);` 擦除从 pos 开始 len 个字符的字符串（如果超出字符串长度则到字符串末尾）
    + replace 代替从 pos 开始 len 的字符串。
    + `find (const string& str, size_t pos = 0)` 从 pos 开始查找字符串 str, 返回第一次匹配的第一个字符的位置。

## 文件读写(File)
+ 导入读取文件类`#include <ifstream>`
+ 导入写入文件类`#include <ofstream>`
???+ example
    + 写文件

    ```c++
    ofstream File("C:\\test.txt");
    File<<"Hello"<<std::endl;
    ```

    + 读文件

    ```c++
    ifstream File("C:\\test.txt");
    std::string str;
    File>>str;
    ```

## Memory model
+ Global data
+ stack
+ heap