[第三章 过程式编程 | 谷雨同学的 C++ 教程](https://learn-cpp.guyutongxue.site/ch03/)

Markdown 学习文档、软件安装包及 Excel 图表都在百度网盘内：
https://pan.baidu.com/s/1P6YtX8PKpyq05g2CWI8yCQ?pwd=1234 提取码: 1234



# 函数

函数需要返回值，函数值的类型有多种，函数的参数也可以为多种

1.函数的返回值可以为void(空)，int(整型),char等常用的，也有指针，结构体，结构体指针

2.函数的参数可以为int,char,double等常见的变量，也可以为指针，引用，结构体，结构体指针，数组，

## 字符串的常用操作

```C++
#include <iostream>
#include <string>
using namespace std;
int main(){
    //1.
    // string s1;    // 初始化一个空字符串
    // string s2 = s1;   // 初始化s2，并用s1初始化
    // string s3(s2);    // 作用同上
    // string s4 = "hello world";   // 用 "hello world" 初始化 s4，除了最后的空字符外其他都拷贝到s4中
    // string s5("hello world");    // 作用同上
    // string s6(6,'a');  // 初始化s6为：aaaaaa
    // string s7(s6, 3);  // s7 是从 s6 的下标 3 开始的字符拷贝
    // //string s8(s6, pos, len);  // s7 是从 s6 的下标 pos 开始的 len 个字符的拷贝
    // string s8(s6,2,4);
    // cout <<s8 <<endl;


    //2.可以用 empty size/length 查询字符串状态及长度，可以用下标操作提取字符串中的字符。
    // string a1 = "abc";    // 初始化一个字符串
    // cout << a1.empty() << endl;  // s 为空返回 true，否则返回 false
    // cout << a1.size() << endl;   // 返回 s 中字符个数，不包含空字符
    // cout << a1.length() << endl;   // 作用同上
    // cout << a1[1] << endl;  // 字符串本质是字符数组
    // cout << a1[3] << endl;  // 空字符还是存在的

    //3.
    string s1 = "Hello";
    string s2 = "World";
    string s3 = s1 + s2;
    cout << s3 << endl;  // 输出：HelloWorld

    // 正确：string + 字面值
    string s4 = s1 + " C++";
    cout << s4 << endl;  // 输出：Hello C++

    // 正确：字面值 + string
    string s5 = "Hi " + s2;
    cout << s5 << endl;  // 输出：Hi World
    // // string s6 = "Hello" + "World"; // 错误！
    //4. 
    // string s1 = "apple";
    // string s2 = "apple";
    // string s3 = "Apple";
    // string s4 = "banana";

    // // 相等比较（区分大小写）
    // cout << boolalpha;  // 输出true/false而非1/0
    // cout << (s1 == s2) << endl;  // 输出：true
    // cout << (s1 == s3) << endl;  // 输出：false (大小写不同)
    // cout << (s1 != s4) << endl;  // 输出：true

    // // 字面值比较
    // cout << (s1 == "apple") << endl;  // 输出：true
    // cout << (s1 == "APPLE") << endl;  // 输出：false
    // 5.
    //
    // string s = "hello";
    
    // // 在位置2插入字符串
    // s.insert(2, "xyz");  // 原字符串: "hello" → "hexyzl lo"
    
    // // 在字符串末尾插入另一个字符串
    // s.insert(s.size(), " world");  // → "hexyzl lo world"
    
    // // 在指定位置插入多个相同字符
    // s.insert(5, 3, '!');  // 在位置5插入3个'!' → "hexyz!!!l lo world"
    
    // cout << s << endl;  // 输出: "hexyz!!!l lo world"

    //6.
    string s = "hello world";
    
    // 从位置1开始删除3个字符
    s.erase(1, 3);  // 原字符串: "hello world" → "ho world"
    
    // 删除单个字符（位置5的字符）
    s.erase(5);  // → "ho worl"
    // // 使用迭代器删除区间（删除从位置2到末尾的所有字符）
    // s.erase(s.begin() + 2, s.end());  // → "ho"
    
    // cout << s << endl;  // 输出: "ho"
    //替换
    // string s = "hello";
    
    // // 清空字符串
    // s.clear();  // s变为空字符串 ""
    
    // // 检查是否为空
    // cout << "Is s empty? " << (s.empty() ? "Yes" : "No") << endl;  // 输出: "Yes"
    
    // // 清空后仍可添加内容
    // s += "world";
    // cout << s << endl;  // 输出: "world"
    // return 0;

}
```

# 结构体

```C++
#include <iostream>
using namespace std;
struct Student{
    int S_id;
    char Class[100];
    double grade[3];
};
void printByPointer(Student* s) {
    cout << "指针传递打印：" << endl;
    cout << "ID: " << s->S_id << endl;  // 使用箭头操作符
    cout << "班级: " << s->Class << endl;
    cout << "成绩: ";
    for (int i = 0; i < 3; i++) {
        cout << s->grade[i] << " ";  // 或 (*s).grade[i]
    }
    cout << endl << "----------------" << endl;
}
int main(){
    Student Sd_a={250301,"初一一班",{99,98,56}};
    cout <<"id的值为:"<<Sd_a.S_id <<" "<<"班级"<<Sd_a.Class <<" ";
    cout << "成绩: ";
    for(int i=0; i<3; i++){
        cout << Sd_a.grade[i] << " ";
    }
    //  2、创建结构体数组
    Student students[3] = {
        {250301, "初一一班", {99, 98, 56}},
        {250302, "初一一班", {85, 76, 92}},
        {250303, "初一二班", {78, 88, 82}}
    };
    // 输出数组内容
    for (int i = 0; i < 3; i++) {
        cout << "学生" << i + 1 << "信息：" << endl;
        cout << "ID：" << students[i].S_id << endl;
        cout << "班级：" << students[i].Class << endl;
        cout << "成绩：语文 " << students[i].grade[0]
             << "，数学 " << students[i].grade[1]
             << "，英语 " << students[i].grade[2] << endl;
        cout << "------------------------" << endl;
    }
    //3.结构体指针
    Student *a=&Sd_a;
    cout <<a <<endl;
    cout <<"id的值为:"<< a->Class<<" "<<"班级"<<a->S_id <<" ";
    cout << "成绩: ";
    for(int i=0; i<3; i++){
        cout << a->grade[i] << " ";
    }
    cout <<endl;
    printByPointer(&Sd_a);
    return 0;
}

```



# 字符数组与整型数组的区别

```C++
#include <iostream>
using namespace std;

int main() {
    // 声明一个字符数组（C风格字符串）
    char str[] = "Hello";

    // 实验1：直接输出 - 字符数组被视为字符串
    cout << "实验1：直接输出" << endl;
    cout << "str:      " << str << endl;       // 输出字符串内容
    cout << "&str[0]:  " << &str[0] << endl;   // 仍被解释为字符串（首元素地址）
    cout << "--------------------------------" << endl;

    // 实验2：输出地址 - 需要强制转换为 void* 类型
    cout << "实验2：地址输出" << endl;
    cout << "str 的地址:      " << (void*)str << endl;      // 正确输出地址
    cout << "&str[0] 的地址:  " << (void*)&str[0] << endl;  // 同样输出首元素地址
    cout << "整个数组的地址:  " << &str << endl;           // 自动识别为 void* 类型
    cout << "--------------------------------" << endl;

    // 实验3：对比非字符类型的数组
    int numbers[] = {1, 2, 3};
    cout << "实验3：对比int数组" << endl;
    cout << "numbers:    " << numbers << endl;    // 自动输出地址（非char类型）
    cout << "&numbers:   " << &numbers << endl;   // 输出整个数组的地址
    cout << "&numbers[0]   " <<&numbers[0] <<endl;
    cout << "--------------------------------" << endl;

    // 实验4：指针变量演示
    char* p = str;
    cout << "实验4：字符指针变量" << endl;
    cout << "p:         " << p << endl;           // 输出字符串内容
    cout << "(void*)p:  " << (void*)p << endl;    // 输出指针存储的地址
    cout << "&p:        " << &p << endl;          // 输出指针变量本身的地址
    cout << "--------------------------------" << endl;

    return 0;
}
```

# 数组不能赋值

###  1.静态数组

C++ 原生数组不支持整体赋值，因为数组名会隐式转换为指向首元素的指针，赋值操作会导致指针地址的复制，而非数组内容的复制。

### 2.动态数组

不可以直接复制，动态数组本质是指针,赋值会导致指针指向同一地址，而非赋值内容

### 3. array

可以直接复制

#### 4.vector

自动管理内存，支持赋值和复制构造

# 函数指针

指针指向函数

int (*函数名)(参数1，参数2)

```C++
int add(int a, int b) {
    return a + b;
}

int subtract(int a, int b) {
    return a - b;
}

int multiply(int a, int b) {
    return a * b;
}

int main() {
    // 函数指针（C++语法，与C相同）
    int (*operation)(int, int);
    
    int x = 10, y = 5;
    
    // 使用函数指针调用不同函数
    operation = add;
    cout << x << " + " << y << " = " << operation(x, y) << endl;
    
    operation = subtract;
    cout << x << " - " << y << " = " << operation(x, y) << endl;
    
    operation = multiply;
   cout << x << " * " << y << " = " << operation(x, y) << endl;
    
    return 0;
}
```



# 指针函数

返回值是指针

int *函数名(参数1，参数2)

```C++
#include <iostream>
using namespace std;

// 指针函数：返回指向动态分配的int数组的指针
int* createArray(int size, int initialValue) {
    int* arr = new int[size];  // 动态分配数组
    
    // 初始化数组元素
    for (int i = 0; i < size; i++) {
        arr[i] = initialValue + i;
    }
    
    return arr;  // 返回指针
}

int main() {
    int size = 5;
    int* myArray = createArray(size, 10);  // 调用指针函数
    
    // 输出数组元素
    for (int i = 0; i < size; i++) {
        cout << "myArray[" << i << "] = " << myArray[i] << endl;
    }
    
    delete[] myArray;  // 释放内存，避免泄漏
    return 0;
}
```



# 函数指针的类型定义

typedef  函数返回   (*函数名字)(参数1，参数2)

类似于int类型，函数名字就变为类型名称了，以后可以使用函数名字定义变量，该变量可以被赋值为其他函数名，该变量就可以指向该函数。

```C++
// 定义MathOperation为函数指针类型
typedef int (*MathOperation)(int, int);

// 使用该类型定义变量
MathOperation op;

// 指向具体函数
int subtract(int a, int b) { return a - b; }
op = subtract;
```

```C++
#include <stdio.h>

// 1. 基础函数指针类型定义
typedef int (*BinaryOperation)(int, int);

// 数学运算函数
int add(int a, int b) {
    return a + b;
}

int subtract(int a, int b) {
    return a - b;
}

int main() {
    // 使用函数指针类型
    BinaryOperation operation = add;
    printf("10 + 5 = %d\n", operation(10, 5));
    
    operation = subtract;
    printf("10 - 5 = %d\n", operation(10, 5));
    
    return 0;
}

```



```C++
#include <stdio.h>
#include <math.h>

// 2. 不同参数类型的函数指针
typedef float (*TrigonometricFunc)(float);   // 三角函数类型
typedef double (*MathFunction)(double, int); // 幂运算类型

// 实现函数
float sine(float x) {
    return sinf(x);
}

float cosine(float x) {
    return cosf(x);
}

double power(double base, int exponent) {
    return pow(base, exponent);
}

int main() {
    // 使用三角函数类型
    TrigonometricFunc trig = sine;
    printf("sin(π/2) = %.2f\n", trig(3.14159f/2));
    
    trig = cosine;
    printf("cos(0) = %.2f\n", trig(0.0f));
    
    // 使用幂运算类型
    MathFunction powerFunc = power;
    printf("2^10 = %.0f\n", powerFunc(2.0, 10));
    
    return 0;
}

```



# 函数指针数组 

[函数指针1,函数指针2，函数指针3]

typedef 函数返回(*函数名[])(参数1，参数2)       函数名={}。  该方法直接定义函数名[]类型，下面初始化是就可以不需要[]

```C++
#include <iostream>
typedef int (*MathOp[])(int, int);  // 定义函数指针数组类型

int add(int a, int b) { return a + b; }
int sub(int a, int b) { return a - b; }

int main() {
    MathOp ops = {add, sub};  // 声明变量时无需[]
    std::cout << ops[0](5, 3) << std::endl;  // 输出8 (5+3)
    std::cout << ops[1](5, 3) << std::endl;  // 输出2 (5-3)
    return 0;
}
```



typedef 函数返回(*函数名)(参数1，参数2)           函数名[]={}    该方法定义函数指针的类型，类似于int 在下方声明数组是需要int[] 即函数名[]

```C++
#include <iostream>
typedef int (*MathOp)(int, int);  // 定义函数指针类型

int add(int a, int b) { return a + b; }
int sub(int a, int b) { return a - b; }

int main() {
    MathOp ops[] = {add, sub};  // 声明数组时需[]
    std::cout << ops[0](5, 3) << std::endl;  // 输出8
    std::cout << ops[1](5, 3) << std::endl;  // 输出2
    return 0;
}
```



# 结构体数组

```C++
struct 结构体名{
    int a;
    char b;
    double c;

}变量名;
```

结构体数组

```
结构体名 数组名[]={{},{},{}}
```

结构体是可以被结构体赋值的，类似于变量

```C++
#include <iostream>
using namespace std;
struct Student{
    int S_id;
    char Class[100];
    double grade[3];
};
void printByPointer(Student *s) {
    cout << "指针传递打印：" << endl;
    cout << "ID: " << s->S_id << endl;  // 使用箭头操作符
    cout << "班级: " << s->Class << endl;
    cout << "成绩: ";
    for (int i = 0; i < 3; i++) {
        cout << s->grade[i] << " ";  // 或 (*s).grade[i]
    }
    cout << endl << "----------------" << endl;
}
int main(){
    Student Sd_a={250301,"初一一班",{99,98,56}};
    cout <<"id的值为:"<<Sd_a.S_id <<" "<<"班级"<<Sd_a.Class <<" ";
    cout << "成绩: ";
    for(int i=0; i<3; i++){
        cout << Sd_a.grade[i] << " ";
    }
    //  2、创建结构体数组
    Student students[3] = {
        {250301, "初一一班", {99, 98, 56}},
        {250302, "初一一班", {85, 76, 92}},
        {250303, "初一二班", {78, 88, 82}}
    };
    // 输出数组内容
    for (int i = 0; i < 3; i++) {
        cout << "学生" << i + 1 << "信息：" << endl;
        cout << "ID：" << students[i].S_id << endl;
        cout << "班级：" << students[i].Class << endl;
        for (int j=0;j<3;j++){
             cout << "成绩：语文 " << students[i].grade[j];
        cout << "------------------------" << endl;
        }
        cout << "成绩：语文 " << students[i].grade[0]
             << "，数学 " << students[i].grade[1]
             << "，英语 " << students[i].grade[2] << endl;
        cout << "------------------------" << endl;
    }
    //3.结构体指针
    Student *a=&Sd_a;
    cout <<a <<endl;
    cout <<"id的值为:"<< a->Class<<" "<<"班级"<<a->S_id <<" ";
    cout << "成绩: ";
    for(int i=0; i<3; i++){
        cout << a->grade[i] << " ";
    }
    cout <<endl;
    printByPointer(&Sd_a);
    return 0;
}
```



# 结构体嵌套



当结构体内部嵌套了其他结构体时，可以直接进行赋值操作，系统会自动完成成员的逐位复制。{}代表对结构体里面的变量赋值。

如果结构体中包含数组，在赋值时需要注意数组不能直接赋值，不过整个结构体是可以赋值的。

访问结构体中的结构体数组时，可以使用一个变量存储结构体数组。

```C++
struct Point{
    double x,y;  //圆点
};
struct Circle{
    Point pt;
    int radius; //圆的半径
};
struct Circles{
    int size;
    Circle c[100]; //圆的几何

};
int main(){
    Circle c;
    c.pt.x=1;
    c.pt.y=2;
    c.radius=3;
    Circles cs={
        2,
        {
            {{8,8},2},
            {{2,1},3}
        }
    };
    for (int i =0;i<cs.size;i++){
        Circle tmp = cs.c[i];
        cout <<"(" <<tmp.pt.x <<"," <<tmp.pt.y <<")" << tmp.radius <<endl;
    }

}
```

## 结构体构造函数



```C++
#include <iostream>
#include <string>
using namespace std;
struct Rectangle {
    int width;
    int height;
    int length;

    // 自定义构造函数
    Rectangle(int w, int h) {
        width = w;
        height = h;
        length =0;
    }
    Rectangle(int w, int h,int l) {
        width = w;
        height = h;
        length =l;
    }
};
int main(){
    Rectangle r(10, 20);  // 调用构造函数初始化
    cout <<r.length<<endl;
    Rectangle w(10, 20,80);
    cout <<w.length <<endl;
}
```



# 联合体

将结构体的struct改为union即可变为联合体，结构体中每一个成员之间是单独的内存，相互之间没有影响。联合体所有成员都是共享内存的，修改一个成员会影响其他成员。所有成员都从一个地址开始。

```C++
union MyUnion {
    int intValue;     // 4字节
    double doubleValue; // 8字节
    char charArray[8];  // 8字节
};
```

```C++
#include <iostream>
#include <cstring>

union Data {
    int i;
    float f;
    char str[20];
};

int main() {
    Data data;
    
    // 使用整数成员
    data.i = 10;
    std::cout << "data.i: " << data.i << std::endl;
    
    // 使用浮点数成员（覆盖整数数据）
    data.f = 3.14f;
    std::cout << "data.f: " << data.f << std::endl;
    
    // 使用字符串成员（覆盖浮点数数据）
    strcpy(data.str, "Hello, union!");
    std::cout << "data.str: " << data.str << std::endl;
    
    // 注意：此时访问i或f会得到无意义的值
    std::cout << "data.i after strcpy: " << data.i << std::endl;  // 未定义行为
    
    return 0;
}
```

当执行以下操作时：

1. 先给 `data.i = 10`：此时内存中存储的是整数 `10` 的二进制（假设为 `0x0000000A`）。
2. 再给 `data.f = 3.14f`：`float` 类型的 `3.14` 会以其特有的二进制格式（IEEE 754 标准）覆盖这 4 字节内存，原来的整数 `10` 被彻底覆盖。
3. 最后给 `data.str = "Hello"`：字符串的字符会逐个写入内存，覆盖掉 `3.14f` 占用的内存（前几个字节被 `'H'、'e'、'l'` 等字符的 ASCII 码覆盖）。

此时，如果你再访问 `data.i` 或 `data.f`，读取的是被字符串覆盖后的内存数据。这些数据原本是字符串的 ASCII 码，强行解析为整数或浮点数时，自然会得到与预期无关的 “无意义数字”。

# new数组空间的生成和释放

语法：

```C++
int *ptr {new int}      int* ptr_init = new int(100); //初始化为100    delete ptr

int *ptr{new int[4]}      int* arrInit = new int[5]{1,2,3}            delete[]  ptr
```

# 引用

```C++
int &a=b   //给b取了一个别名，a的地址和b的地址一样
```

引用可以当函数参数，也可以当函数返回值，

## 指针引用

```C++
*&
    char*b
    char *&a=b,为b指针变量取别名
```



# 求字符串长度

```C++
int main(){
    char a[]{"Hello World"};
    cout <<strlen(a);
    string b{"Hello"};
    cout <<endl;
    cout <<b.length()  <<endl;
    cout <<b.size() <<endl;
}
```





## 递归算法

## 什么是递归？

我们习惯将函数（方法）调用自身的过程称为递归，调用自身的函数称为递归函数，用递归方式解决问题的算法称为递归算法

递归，在计算机科学中是指一种通过重复将问题分解为同类的子问题而解决问题的方法。简单来说，递归表现为函数调用函数本身。在知乎看到一个比喻递归的例子，个人觉得非常形象，大家看一下：

> 递归最恰当的比喻，就是查词典。我们使用的词典，本身就是递归，为了解释一个句子，需要使用更多的词。当你查一个句子，发现这个句子的解释中某个词仍然不懂，于是你开始查这第二个词，可惜，第二个词里仍然有不懂的词，于是查第三个词，这样查下去，直到有一个词的解释是你完全能看懂的，那么递归走到了尽头，然后你开始后退，逐个明白之前查过的每一个词，最终，你明白了最开始那个词的意思。

来试试水，看一个递归的代码例子吧，如下：

```text
int sum(int n) {
    if (n <= 1) {
        return 1;
    } 
    return sum(n - 1) + n; 
}
```

## 递归的特点

实际上，递归有两个显著的特征,终止条件和自身调用:

- 自身调用：原问题可以分解为子问题，子问题和原问题的求解方法是一致的，即都是调用自身的同一个函数。
- 终止条件：递归必须有一个终止的条件，即不能无限循环地调用本身。

结合以上demo代码例子，看下递归的特点：

![img](https://pic1.zhimg.com/v2-2f23016591a7cd0467683c60c42fb574_1440w.jpg)

### **题目 1：递归计算 n 的阶乘**

**描述**：阶乘定义为 `n! = n × (n-1) × ... × 1`，且 `0! = 1`，用递归实现计算 `n!`。

n!=n*(n-1)!       (n-1)!=(n-1)*(n-2)!   1!=1

```C++
#include <iostream>
using namespace std;

// 递归计算n的阶乘
long long factorial(int n) {
    // 基线条件：n=0时返回1
    if (n == 0) {
        return 1;
    }
    // 递归关系：n! = n × (n-1)!
    return n * factorial(n - 1);
}

int main() {
    int n;
    cout << "输入n（n≥0）：";
    cin >> n;
    
    if (n < 0) {
        cout << "n不能为负数！" << endl;
        return 1;
    }
    
    cout << n << "! = " << factorial(n) << endl;
    // 示例：输入5 → 输出120（5×4×3×2×1×1）
    return 0;
}
```

## 汉诺塔问题

**问题描述**：有三根柱子（A、B、C）和 `n` 个大小不同的盘子，初始时所有盘子按大小顺序堆叠在 A 柱上（大盘在下，小盘在上）。目标是将所有盘子移动到 C 柱，每次只能移动一个盘子，且任何时候都不能将大盘放在小盘之上。

- 递归步骤
  1. ​	将 `n-1` 个盘子从 A移动到 B（借助 C作为辅助）。
     1. 将第 `n` 个盘子从 A移动到 C。
     2. 将 `n-1` 个盘子从 B移动到 C（借助 A 作为辅助）。

```C++
#include <iostream>
using namespace std;

// 递归函数：解决汉诺塔问题
// 参数：n-盘子数量，A-起始柱子，B-辅助柱子，C-目标柱子
void hanoi(int n, char A, char B, char C) {
    if (n == 1) {
        // 基线条件：只有一个盘子时，直接从A移动到C
        cout << "Move disk 1 from " << A << " to " << C << endl;
        return;
    }
    
    // 递归步骤1：将n-1个盘子从A移动到B（借助C作为辅助）
    hanoi(n - 1, A, C, B);
    
    // 移动第n个盘子从A到C
    cout << "Move disk " << n << " from " << A << " to " << C << endl;
    
    // 递归步骤2：将n-1个盘子从B移动到C（借助A作为辅助）
    hanoi(n - 1, B, A, C);
}

int main() {
    int n;
    cout << "请输入盘子数量：";
    cin >> n;
    
    if (n <= 0) {
        cout << "盘子数量必须为正整数！" << endl;
        return 1;
    }
    
    cout << "解决" << n << "个盘子的汉诺塔步骤：" << endl;
    hanoi(n, 'A', 'B', 'C');  // 明确指定三根柱子为A、B、C
    
    return 0;
}
```

