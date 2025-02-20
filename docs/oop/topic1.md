# Introduction

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
+ 开头`usingnamespace std`使得`std::cout`可以简写为`cout`

## 字符串(String)
+ 导入类：`#include <string>`
+ 定义：`string str="Hello"`

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