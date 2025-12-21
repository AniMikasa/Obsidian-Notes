# 常用头文件
\<iostream\> cin cout
\<iomanip\> setw setprecision
\<cstring\> strcpy strlen
\<cctype\> toupper
\<cstdio\> printf scanf
\<cmath\> [[Summary#cmath| CMATH]]
\<random\>
\<ctime\>
\<vector\> 动态数组
万能头文件：#include \<bits/stdc++.h\>
# cmath

| 函数类型              | cmath 中对应的函数                                                                  |
| ----------------- | ----------------------------------------------------------------------------- |
| 绝对值函数($\|x\|$)    | `int abs(int x)`<br>`double fabs(double x)`                                   |
| 指数函数($e^x$)       | `double exp(double x)`                                                        |
| 幂函数($x^y$)        | `double pow(double x, double y)`                                              |
| 平方根函数($\sqrt{x}$) | `double sqrt(double x)`                                                       |
| 自然对数函数($ln x$)    | `double log(double x)`                                                        |
| 对数函数($lg x$)      | `double log10(double x)`                                                      |
| 三角函数              | `double sin(double x)`<br>`double cos(double x)`<br>`double tan(double x)`    |
| 反三角函数             | `double asin(double x)`<br>`double acos(double x)`<br>`double atan(double x)` |

# 保留词

- **基本关键字**
`alignas` `alignof` `and` `and_eq` `asm` `auto` `bitand` `bitor` `bool` 
`break` `case` `catch` `char` `char8_t` `char16_t` `char32_t` `class` 
`compl` `concept` `const` `consteval` `constexpr` `const_cast` `continue` 
`co_await` `co_return` `co_yield` `decltype` `default` `delete` `do` 
`double` `dynamic_cast` `else` `enum` `explicit` `export` `extern` `false` 
`float` `for` `friend` `goto` `if` `inline` `int` `long` `mutable` `namespace` 
`new` `noexcept` `not` `not_eq` `nullptr` `operator` `or` `or_eq` `private` 
`protected` `public` `register` `reinterpret_cast` `requires` `return` 
`short` `signed` `sizeof` `static` `static_assert` `static_cast` `struct` 
`switch` `template` `this` `thread_local` `throw` `true` `try` `typedef` 
`typeid` `typename` `union` `unsigned` `using` `virtual` `void` `volatile` 
`wchar_t` `while` `xor` `xor_eq`
- **替代表示符**
`and` (代替 `&&`)   `or` (代替 `||`)    `not` (代替 `!`)    `bitand` (代替 `&`)
`bitor` (代替 `|`)    `compl` (代替 `~`)    `xor` (代替 `^`)    `and_eq` (代替 `&=`)    `or_eq` (代替 `|=`)
`xor_eq` (代替 `^=`)
- **重要说明**
1. C++11/14/17/20 新增关键字：如 `char8_t`, `concept`, `consteval`, `co_await` 等
2. 上下文相关关键字：如 `final`, `override` 等（在特定上下文中是关键字）
3. 预定义宏：如 `__cplusplus` 等（虽然不是关键字，但应避免使用）
4. 标准库名称：如 `cout`, `cin`, `string` 等（虽然不是关键字，但建议避免重定义）

# 用二维数组保存多个字符串
```cpp
char words[8][6]={"what","are","you","doing","?"};
cout<<words[1]<<endl; //显示are
cout<<words[3]+1<<endl; //显示oing
cout<<words[0][2]<<endl; //显示a
words[6]={'s','t','u','d','y','\0'} //error:不能用初始化表进行赋值
strcpy(words[6],"study");
```
```cpp
char words[8][5]={};
char sentence[100]="hello every one";
strcpy(words[5],sentence[6]);//编译报错，参数不匹配
strcpy(words[5],sentence+6);//将every one复制到第5行
cout<<words[6]<<endl;//显示one(前面有一个空格)
cout<<words[5]<<endl;//显示every one
```

# 自动类型转换

**转换方向**：`char → short → int → unsigned → long → float → double`
在混合类型表达式中，不同类型数据运算时自动转换的"优先级"顺序。箭头方向表示"小类型向大类型转换"。排在后面的类型可以"容纳"排在前面的类型（内存大小）。这样的转换方向确保了计算精度不丢失，避免了数据溢出并统一计算标准。特殊情况：相同级别的有符号和无符号运算时，有符号转为无符号。整型与浮点运算时，整型转为浮点。

**转换规则示例**：
```cpp
char c = 'A';
int i = 10;
double d = 3.14;

double result1 = c + i;     // char→int→double
double result2 = i + d;     // int→double
float result3 = c + 2.5f;   // char→float

int i = 3.14;    // i = 3，小数部分被截断
double d = 5;    // d = 5.0，整数转为浮点数
```

# size of ()
**基本语法:**
```cpp
sizeof(类型)
sizeof(表达式)
// 或者
sizeof 表达式   // 当操作数是表达式时，括号可以省略（但建议总是使用括号）
```

**常见用法和示例**：
a. 用于基本数据类型
```cpp
#include <stdio.h>

int main() {
    printf("sizeof(char) = %zu\n", sizeof(char));        
    // 总是 1
    printf("sizeof(short) = %zu\n", sizeof(short));      
    // 通常是 2
    printf("sizeof(int) = %zu\n", sizeof(int));          
    // 通常是 4
    printf("sizeof(long) = %zu\n", sizeof(long));        
    // 4 或 8，取决于系统
    printf("sizeof(float) = %zu\n", sizeof(float));      
    // 通常是 4
    printf("sizeof(double) = %zu\n", sizeof(double));    
    // 通常是 8

    return 0;
}
//注意：基本数据类型的大小依赖于编译器和平台。
```
b. 用于变量
```cpp
int num = 10;
double arr[20];
char ch;

printf("sizeof(num) = %zu\n", sizeof(num));     
// 等价于 sizeof(int)
printf("sizeof(arr) = %zu\n", sizeof(arr));     
// 20 * sizeof(double)
printf("sizeof(ch) = %zu\n", sizeof(ch));       
// 等价于 sizeof(char) = 1
```
 c. 用于数组（重要！）
```cpp
int arr[10];
printf("整个数组的大小: %zu\n", sizeof(arr));   
 // 10*sizeof(int)
printf("数组元素个数: %zu\n", sizeof(arr)/sizeof(arr[0]));  // 10 - 计算数组长度的常用方法

// 但在函数参数中，数组会退化为指针
void func(int arr_param[]) {
    printf("在函数中: %zu\n", sizeof(arr_param));  
// 输出指针的大小(4或8)，不是数组大小！
}
```
 d. 用于指针
```cpp
int *ptr;
char *cptr;
double *dptr;

printf("sizeof(ptr) = %zu\n", sizeof(ptr));     // 所有数据指针的大小通常相同(4或8)
printf("sizeof(cptr) = %zu\n", sizeof(cptr));   // 同上
printf("sizeof(dptr) = %zu\n", sizeof(dptr));   // 同上
```
e. 用于结构体
```cpp
struct Student {
    int id;
    char name[20];
    double score;
};

printf("sizeof(Student) = %zu\n", sizeof(struct Student));  // 可能大于各成员之和（由于内存对齐）
```

**重要特性：** 编译时求值：`sizeof` 在编译时计算，不会在运行时计算。且不会对表达式求值。
   ```cpp
   int x = 10;
   printf("%zu\n", sizeof(x++));  // x++ 不会执行！
   printf("%d\n", x);             // 仍然是 10
   
   sizeof(1+1)  // 编译时等价于 sizeof(2)，再等价于 sizeof(int)
   ```

**与 `strlen()` 的区别：**
> [!info]
> 
| `sizeof` | `strlen()` |
|----------|------------|
| 运算符 | 函数 |
| 计算内存大小 | 计算字符串长度 |
| 包含 '\0' | 不包含 '\0' |
| 编译时求值 | 运行时求值 |


# 输出数组元素
```cpp
for (int i=0;i<25;i++){
	cout<<a[i/5][i%5]
}


for (int i=0;i<25;i++){
	cout<<a[0][i]//二位数组其实是一位数组连续排序的
}
```

# ASCII
```
Dec Hex Char   Dec Hex Char    Dec Hex Char    Dec Hex Char
000 00 NUL      032 20 Space    064 40 @        096 60 `
001 01 SOH      033 21 !        065 41 A        097 61 a
002 02 STX      034 22 "        066 42 B        098 62 b
003 03 ETX      035 23 #        067 43 C        099 63 c
004 04 EOT      036 24 $        068 44 D        100 64 d
005 05 ENQ      037 25 %        069 45 E        101 65 e
006 06 ACK      038 26 &        070 46 F        102 66 f
007 07 BEL      039 27 '        071 47 G        103 67 g
008 08 BS       040 28 (        072 48 H        104 68 h
009 09 HT       041 29 )        073 49 I        105 69 i
010 0A LF       042 2A *        074 4A J        106 6A j
011 0B VT       043 2B +        075 4B K        107 6B k
012 0C FF       044 2C ,        076 4C L        108 6C l
013 0D CR       045 2D -        077 4D M        109 6D m
014 0E SO       046 2E .        078 4E N        110 6E n
015 0F SI       047 2F /        079 4F O        111 6F o
016 10 DLE      048 30 0        080 50 P        112 70 p
017 11 DC1      049 31 1        081 51 Q        113 71 q
018 12 DC2      050 32 2        082 52 R        114 72 r
019 13 DC3      051 33 3        083 53 S        115 73 s
020 14 DC4      052 34 4        084 54 T        116 74 t
021 15 NAK      053 35 5        085 55 U        117 75 u
022 16 SYN      054 36 6        086 56 V        118 76 v
023 17 ETB      055 37 7        087 57 W        119 77 w
024 18 CAN      056 38 8        088 58 X        120 78 x
025 19 EM       057 39 9        089 59 Y        121 79 y
026 1A SUB      058 3A :        090 5A Z        122 7A z
027 1B ESC      059 3B ;        091 5B [        123 7B {
028 1C FS       060 3C <        092 5C \        124 7C |
029 1D GS       061 3D =        093 5D ]        125 7D }
030 1E RS       062 3E >        094 5E ^        126 7E ~
031 1F US       063 3F ?        095 5F _        127 7F DEL
```


# C++ 运算符总结

## 一、算术运算符

| 运算符 | 描述 | 示例 |
|--------|------|------|
| `+` | 加法 | `a + b` |
| `-` | 减法 | `a - b` |
| `*` | 乘法 | `a * b` |
| `/` | 除法 | `a / b` |
| `%` | 取模（求余） | `a % b` |
| `++` | 自增 | `a++` 或 `++a` |
| `--` | 自减 | `a--` 或 `--a` |

```cpp
int a = 10, b = 3;
cout << a + b;  // 13
cout << a / b;  // 3 (整数除法)
cout << a % b;  // 1
```

## 二、关系运算符

| 运算符 | 描述 | 示例 |
|--------|------|------|
| `==` | 等于 | `a == b` |
| `!=` | 不等于 | `a != b` |
| `>` | 大于 | `a > b` |
| `<` | 小于 | `a < b` |
| `>=` | 大于等于 | `a >= b` |
| `<=` | 小于等于 | `a <= b` |

```cpp
int a = 5, b = 3;
bool result = (a > b);  // true
```

## 三、逻辑运算符

| 运算符 | 描述 | 示例 |
|--------|------|------|
| `&&` | 逻辑与 | `a && b` |
| `||` | 逻辑或 | `a || b` |
| `!` | 逻辑非 | `!a` |

```cpp
bool a = true, b = false;
cout << (a && b);  // false
cout << (a || b);  // true
cout << (!a);      // false
```

## 四、位运算符

| 运算符 | 描述 | 示例 |
|--------|------|------|
| `&` | 位与 | `a & b` |
| `|` | 位或 | `a | b` |
| `^` | 位异或 | `a ^ b` |
| `~` | 位取反 | `~a` |
| `<<` | 左移 | `a << n` |
| `>>` | 右移 | `a >> n` |

```cpp
int a = 5;  // 0101
int b = 3;  // 0011
cout << (a & b);   // 1 (0001)
cout << (a | b);   // 7 (0111)
cout << (a << 1);  // 10 (1010)
```

## 五、赋值运算符

| 运算符 | 描述 | 示例 |
|--------|------|------|
| `=` | 简单赋值 | `a = b` |
| `+=` | 加后赋值 | `a += b` |
| `-=` | 减后赋值 | `a -= b` |
| `*=` | 乘后赋值 | `a *= b` |
| `/=` | 除后赋值 | `a /= b` |
| `%=` | 取模后赋值 | `a %= b` |
| `&=` | 位与后赋值 | `a &= b` |
| `|=` | 位或后赋值 | `a |= b` |
| `^=` | 位异或后赋值 | `a ^= b` |
| `<<=` | 左移后赋值 | `a <<= b` |
| `>>=` | 右移后赋值 | `a >>= b` |

```cpp
int a = 10;
a += 5;   // a = 15
a *= 2;   // a = 30
```

## 六、其他运算符

### 1. 条件运算符（三元运算符）
```cpp
条件 ? 表达式1 : 表达式2;
int max = (a > b) ? a : b;
```

### 2. 逗号运算符
```cpp
int a = (b = 3, b + 2);  // a = 5, b = 3
```

### 3. sizeof 运算符
```cpp
int arr[5];
cout << sizeof(arr);      // 20 (假设int为4字节)
cout << sizeof(int);      // 4
```

### 4. 取地址和引用运算符
```cpp
int a = 10;
int* p = &a;    // & 取地址
int& r = a;     // & 声明引用
int b = *p;     // * 解引用
```

### 5. 成员访问运算符
```cpp
struct Point {
    int x, y;
};

Point p;
p.x = 10;        // . 直接成员访问

Point* ptr = &p;
ptr->x = 20;     // -> 指针成员访问
```

### 6. 作用域解析运算符
```cpp
namespace MySpace {
    int value = 10;
}

cout << MySpace::value;  // :: 访问命名空间成员
```

### 7. 类型转换运算符
```cpp
int a = 10;
double b = static_cast<double>(a);  // C++风格类型转换
double c = (double)a;               // C风格类型转换
```

## 七、运算符优先级表（从高到低）

| 优先级 | 运算符                                                    | 描述      | 结合性  |
| --- | ------------------------------------------------------ | ------- | ---- |
| 1   | `::`                                                   | 作用域解析   | 从左到右 |
| 2   | `()` `[]` `.` `->` `++` `--`                           | 后置自增/自减 | 从左到右 |
| 3   | `++` `--` `+` `-` `!` `~` `*` `&` `(type)` `sizeof`    | 前置运算符   | 从右到左 |
| 4   | `.*` `->*`                                             | 成员指针    | 从左到右 |
| 5   | `*` `/` `%`                                            | 乘除取模    | 从左到右 |
| 6   | `+` `-`                                                | 加减      | 从左到右 |
| 7   | `<<` `>>`                                              | 位移      | 从左到右 |
| 8   | `<` `<=` `>` `>=`                                      | 关系      | 从左到右 |
| 9   | `==` `!=`                                              | 相等      | 从左到右 |
| 10  | `&`                                                    | 位与      | 从左到右 |
| 11  | `^`                                                    | 位异或     | 从左到右 |
| 12  | \|                                                     | 位或      | 从左到右 |
| 13  | `&&`                                                   | 逻辑与     | 从左到右 |
| 14  | \|\|                                                   | 逻辑或     | 从左到右 |
| 15  | `?:`                                                   | 条件      | 从右到左 |
| 16  | `=` `+=` `-=` `*=` `/=` `%=` `&=` `=` `^=` `<<=` `>>=` | 赋值      | 从右到左 |
| 17  | `,`                                                    | 逗号      | 从左到右 |

## 八、特殊运算符

### 1. new 和 delete（动态内存）
```cpp
int* p = new int;       // 分配单个int
int* arr = new int[5];  // 分配数组
delete p;               // 释放单个
delete[] arr;           // 释放数组
```

### 2. typeid（运行时类型信息）
```cpp
#include <typeinfo>
cout << typeid(a).name();  // 获取类型名称
```

### 3. throw（异常抛出）
```cpp
throw runtime_error("错误信息");
```

## 九、运算符重载

```cpp
class Complex {
private:
    double real, imag;
public:
    // 重载 + 运算符
    Complex operator+(const Complex& other) {
        return Complex(real + other.real, imag + other.imag);
    }
    
    // 重载 << 运算符（友元函数）
    friend ostream& operator<<(ostream& os, const Complex& c);
};

ostream& operator<<(ostream& os, const Complex& c) {
    os << c.real << " + " << c.imag << "i";
    return os;
}
```

## 总结要点

1. **算术运算符**：基本数学运算
2. **关系运算符**：比较大小，返回布尔值
3. **逻辑运算符**：组合布尔表达式
4. **位运算符**：对二进制位进行操作
5. **赋值运算符**：赋值和复合赋值
6. **其他运算符**：包括条件、逗号、sizeof等
7. **优先级**：使用括号明确运算顺序
8. **重载**：可以为自定义类型定义运算符行为

记住：当不确定运算顺序时，使用括号是最安全的选择！




```mermaid

<mxCell id="2" value="C++程序设计与思想" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;fontSize=16;fontStyle=1;" vertex="1" parent="1">

  <mxGeometry x="300" y="20" width="200" height="60" as="geometry"/>

</mxCell>

<mxCell id="3" value="基础语法" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;" vertex="1" parent="1">

  <mxGeometry x="50" y="120" width="120" height="80" as="geometry"/>

</mxCell>

<mxCell id="4" value="面向对象编程" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;" vertex="1" parent="1">

  <mxGeometry x="200" y="120" width="120" height="80" as="geometry"/>

</mxCell>

<mxCell id="5" value="模板编程" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;" vertex="1" parent="1">

  <mxGeometry x="350" y="120" width="120" height="80" as="geometry"/>

</mxCell>

<mxCell id="6" value="标准库STL" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;" vertex="1" parent="1">

  <mxGeometry x="500" y="120" width="120" height="80" as="geometry"/>

</mxCell>

<mxCell id="7" value="内存管理" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;" vertex="1" parent="1">

  <mxGeometry x="650" y="120" width="120" height="80" as="geometry"/>

</mxCell>

<mxCell id="8" value="变量与数据类型" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="20" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="9" value="控制结构" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="140" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="10" value="函数与作用域" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="260" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="11" value="类与对象" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="170" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="12" value="继承与多态" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="290" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="13" value="函数模板" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="320" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="14" value="类模板" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="440" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="15" value="容器" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="470" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="16" value="算法" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="590" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="17" value="迭代器" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="710" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="18" value="智能指针" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="620" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="19" value="RAII机制" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="1">

  <mxGeometry x="740" y="240" width="100" height="60" as="geometry"/>

</mxCell>

<mxCell id="20" value="设计模式" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;" vertex="1" parent="1">

  <mxGeometry x="200" y="360" width="120" height="60" as="geometry"/>

</mxCell>

<mxCell id="21" value="最佳实践" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;" vertex="1" parent="1">

  <mxGeometry x="350" y="360" width="120" height="60" as="geometry"/>

</mxCell>

<mxCell id="22" value="性能优化" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;" vertex="1" parent="1">

  <mxGeometry x="500" y="360" width="120" height="60" as="geometry"/>

</mxCell>

<mxCell id="23" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="2" target="3">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="24" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="2" target="4">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="25" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="2" target="5">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="26" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="2" target="6">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="27" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="2" target="7">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="28" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="3" target="8">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="29" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="3" target="9">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="30" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="3" target="10">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="31" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="4" target="11">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="32" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="4" target="12">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="33" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="5" target="13">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="34" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="5" target="14">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="35" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="6" target="15">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="36" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="6" target="16">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="37" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="6" target="17">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="38" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="7" target="18">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="39" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="7" target="19">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="40" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="4" target="20">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="41" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="3" target="21">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>

<mxCell id="42" style="endArrow=classic;html=1;edgeStyle=orthogonalEdgeStyle;exitX=0.5;exitY=0;entryX=0.5;entryY=1;" edge="1" parent="1" source="6" target="22">

  <mxGeometry relative="1" as="geometry"/>

</mxCell>
```