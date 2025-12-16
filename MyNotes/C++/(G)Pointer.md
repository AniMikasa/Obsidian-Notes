## 1、指针的概念
**指针**：指针是以内存地址作为数据的值，允许多个指针共享同一地址上的值，支持内存动态分配（不是预先定义）
**指针变量**：存储地址的变量
**变量的指针**：变量A存储变量B的地址，那么A是B的指针
**定义指针**：类型标识符   \*指针变量
```cpp
int  *intp
```
**指针内容的调用**：解引用运算符"\*"    

> 提示：\*intp 代表intp指向的这个内存单元的内容,而intp指向任何随机的地方

**指针的初始化**：可用NULL来初始化一个指针，表示不指向任何地址（空指针）
```cpp
int *intp = NULL//符号常量，值为0
//或者nullptr指针常量，不能自动转化为整型
```
**指针的赋值**：取地址运算符"&"
```cpp
intp = &x //&后面不能跟常量或表达式，如&2没有意义，&(m*n+p)也是没有意义的
const char *s = "Hello"；//把字符串的起始地址存入指针
int x, *p = &x;//在定义的同时完成初始化
```

**统配指针类型**
```cpp
void *指针变量名;//只存放地址，不指定类型，任何指针都可以对其赋值
```

**指针常量与常量指针**：
```cpp
//指针常量：不许改变指针本身
int x=5;int *const p=&x;*p=8;//x的值变为8
//指针常量必须在定义时给出初始值
```

```cpp
//常量指针：不许改变指针所指的对象，本身是变量
const int *p;//或int const *p;
int x,y;p=&x;p=&y;
//注意这里x,y实际上不是常量，我们可以随意对它们赋值，但是不可以通过指针调用它们进行赋值！
```

## 2、指针运算与数组
**指针运算**
指针+(-)1表示数组中指针指向元素的下(上)一元素地址。合法的指针操作如：p+k,p-k,p1-p2。指针可以相减，但不能相加。如果是p一个指向整型的指针且值为1000，执行p++后，它的值为1004


**指向数组元素的指针**：每个数组元素都是一个独立的变量，因此可以有指针指向它(p=&a\[3\])
数组元素的地址是通过数组的首地址来计算。（如数组的首地址是1000，则第i个元素的地址是1000+i\*每个数组元素所占的空间长度）

> 获取int数组a中元素a\[1\]的地址的方式: &a\[1],a+1    (编译器会自动4\*1,所以不是a+4)

**指针与数组**：数组名可以看成是指针常量，对一维数组来说，数组名是数组的起始地址，也就是第0个元素的地址。`cout`一个数组的指针可以不加解引用。
```cpp
//输出数组a的10个元素
for (i = 0; i < 10; ++i)
    cout << a[i];
for (i = 0; i < 10; ++i)
    cout << *(a + i);
for (p = a; p < a + 10; ++p)
    cout << *p;
for (p = a, i = 0; i < 10; ++i)
    cout << *(p + i);
for (p = a, i = 0; i < 10; ++i)
    cout << p[i];
```

## 3、动态内存分配
**动态内存分配与回收** 定义一个指针，并在运行时为其分配适当的内存。C++中由new和delete两个运算符来实现内存的动态分配。注意只有动态数组可以用变量来规定大小，普通数组不可以!
 ```cpp
 //运算符new用于进行内存分配：
 char* p=new char;
 char* p=new char[n];//把元素初始化为空字符 p=new char [n]();p=new char[n]{'A',0};
 p=new char('A');//()代表初值
 
 //delete释放new分配的内存
 delete p;
 delete []p;//数组的情况
 ```

**检查new操作失败**：由于内存空间有限，有可能无法分配所需的内存空间。可用标准库`cassert`中的`assert()`宏来检查new操作失败。`assert()`有一个参数，通常是一个表达式，称为“断言”。如果断言为真，则程序继续运行。如果断言为假，则在发出一个“错误消息”后程序会终止。该“错误消息”包含源文件名、出错行数和表达式的字面内容。十分有助于debug。
```cpp
#include <iostream>
#include <cassert> // 包含assert宏的库
using namespace std;

int main() {
    int *p;
    p = new int[10000];
    assert(p != NULL); // 若p等于NULL，则发出错误消息，退出程序执行
    *p = 20; 
    cout << p[0]; // 输出20
    delete[] p;
    return 0;
}
```
当然也可以手动检查：
```cpp
if(!p) cout<<"<Error Information>";
```

**内存分配的进一步介绍**：
静态分配：对全局变量和静态变量，编译器为它们分配空间，这些空间在整个程序运行期间都存在。
自动分配：局部变量的空间分配在系统栈区（stack）。当函数被调用时，空间被分配；当函数执行结束后，空间被释放。
动态分配：在程序执行过程中需要新的存储空间时，可用动态分配的方法向系统中请新的空间，当不再使用时用显式的方法还给系统（非自动）。这部分空间是从被称为堆(heap)的内存区域分配的。
```
OS
Program
Heap（堆）         //动态分配
Stack（栈）        //自动分配
Globe variables   //静态分配
```

**内存泄露** ：如果失去了指向动态分配的区域的指针，这些区域将变成孤立的。堆管理器认为你在继续使用它们，但你不知道它们在哪里。可以用`delete`操作符释放由`new`申请的内存（只能释放`new`申请的内存）。程序结束运行后，该区域的内存会自动被全部释放。当你释放了内存空间，堆管理器重新收回这些区域，但你的指针仍然指向堆区域(意思是不会删除该指针)，但你不应该再通过该指针访问它指向的区域。你要确保在程序中同一块用`new`分配的内存只释放一次。
```cpp
int *p=new int[10];
if (n>0){
	p=new int [n];//内存泄漏
	delete []p;
}
delete []p;//重复释放
```

**示例**:输入数据，计算它们的和。数据个数在设计程序时未知，据程序运行时用户输入的值来确定。存储一批数据（同类）应该用数组，但C++的数组大小必须是固定的。该问题有两个解决方案：
- 开设一个足够大的数组，每次运行时只使用一部分,但缺点是浪费空间
- 用动态内存分配根据输入的数据量申请一个动态数组
```cpp
#include <iostream>
#include <cstdlib> // 需要包含这个头文件来使用exit函数
using namespace std;

int main() {
    int *p, i, n, sum = 0;
    cout << "An array will be created dynamically.\n\n";
    cout << "Input an array size n followed by n integers:";
    cin >> n;
    
    if (!(p = new int[n])) exit(1);  // 修正了语法错误，和之前的assert是一样的
    
    for (i = 0; i < n; ++i) cin >> p[i];
    for (i = 0; i < n; ++i) sum += p[i];
    
    delete[] p;
    cout << "Number of elements:" << n << endl;
    cout << "Sum of the elements:" << sum << endl;
    return 0;
}
```

## 4、字符串再讨论
**字符串再讨论**：字符串的另一种表示是定义一个指向字符的常量指针，然后直接将一个字符串常量赋给它。字符串常量存储在一个称为"数据段"的内存区域里。
```

┌─────────────┐
│    Stack    │ ← 局部变量（包括字符数组）        栈
├─────────────┤
│    Heap     │ ← 动态分配的内存（malloc/new）   堆
├─────────────┤
│             │
│  数据段 Data │ ← 全局/静态变量、字符串常量
│  (BSS/Data) │
├─────────────┤
│  代码段 Text │ ← 程序代码（有些编译器把字符串常量放这里）
└─────────────┘

```
如：将存储字符串"abcde"的内存的首地址赋给常量指针letters。
```cpp
const char *letters;
letters = "abcde";   //注意这是在用地址赋值，不是把字符串的内容赋值给指针！
```
**用指针表示字符串的其他用法**：无论哪种方法，本质上都是将字符串的首地址赋值给指针
1.将表示字符串的字符数组(栈区)名赋给指针(因为该数组名就是字符串的首地址)；
2.用动态内存分配的方法申请一块空间存储字符串，让指针指向该空间(堆区)(因为该空间的首地址就是字符串的首地址)。
**用指针表示的字符串的操作**：指针可以直接作为字符串操作函数的参数。但必须注意，如果该指针指向的是一个字符串常量时，则使用是受限的(如不能作为strcpy的第一个参数，因为数据段区不可修改)。当字符串用一个指针变量表示时，我们可以把此指针变量解释成数组的首地址，通过下标访问字符串中的字符。如 const char \*letters = "abcde"，则 letters\[3\]的值是'd'。
**字符串作为函数的参数**：字符串作为参数和数组名作为参数的传递一样，可以有两种方法：作为字符数组传递；作为指向字符的指针传递。两种传递方式的本质是一样的，都是传递了字符串的首地址。和一般数组不同，字符串作为字符数组传递时不需要指定长度。因为字符串操作的结果是依据空字符`\0`。

**例一**:完成一个函数，统计字符串中单词数
```cpp
#include "ctype.h" // 包含函数isspace(char)，判断参数是否为空白字符
using namespace std;

int word_cnt(const char *s) { // 不能通过s修改它指向的字符串，但可以修改s
    int cnt = 0;
    while (*s != '\0') {
        while (isspace(*s)) ++s; // 跳过空白字符
        if (*s != '\0') {
            ++cnt; // 找到一个单词
            while (!isspace(*s) && *s != '\0') ++s; // 跳过单词
        }
    }
    return cnt;
}
```
**例二**：实现高精度加法（计算两个128位的十进制整数的和）
```cpp
//生成数函数的设计
//根据用字符串表示的数（如“12345”）来生成所需的数组
int set(const char *s1, char s[]) { // s1:任意长度的字符串; s: 128个元素的数组
    int i = strlen(s1) - 1, j = 0;

    while (i >= 0) {
        if (s1[i] > '9' || s1[i] < '0') { 
            cout << "set error\n"; 
            return -1;
        }
        else { 
            s[i] = s1[i]; 
            --i; 
            ++j; 
        }
    }

    for ( ; j < 128; ++j ) 
        s[j] = '0'; // 剩下的元素都赋值为字符零

    return 0;
}


// 加法函数的设计
// p1和p2相加，结果存入s  
void add(const char *p1, const char *p2, char s[]) {  
    int i, tmp, carry = 0; // carry为进位  

    for (i = 0; i < 128; ++i) {  
        tmp = p1[i] - '0' + p2[i] - '0' + carry;  
        if (tmp >= 10) 
            carry = tmp / 10; 
        else 
            carry = 0;  
        s[i] = tmp % 10 + '0';  
    }  
}


//显示函数的设计
void show(const char *s) {
    int i = 127;
    while (s[i] == '0') 
        --i; // 跳过前面的0
    while (i >= 0) {
        cout << s[i];
        --i;
    }
    cout << endl;
}
```
注意，`cout`字符数组的名字会得到以`\0`结尾的字符串，为了得到字符数组的地址，我们需要使用类型转换输出，例如：
```cpp
cout << "字符数组地址: " << (void*)str << endl;
```
## 5、指针与函数
**指针作为函数参数**：
**例一**:编一函数，交换两个参数的值(址传递)
```cpp
swap(int *a,int *b){
	int c;
	c=*a;
	*a=*b;
	*b=c;
}
swap(&x,&y);
```
**例二**:实现解一元二次方程的函数
目前为止我们了解到的函数只能有一个返回值，由return语句返回。一个一元二次方程有两个解，为了让此函数返回两个解,采用指针作为函数的参数。
函数原型可设计为：
```cpp
void SolveQuadratic(double a, double b, double c, double *px1, double *px2)
```
函数的调用：
  ```cpp
  SolveQuadratic(1.3, 4.5, 2.1, &x1, &x2)
  SolveQuadratic(a, b, c, &x1, &x2)
  ```
我们可以进一步改进原型 :并不是每个一元二次方程都有两个不同根，有的可能有两个等根，有的可能没有根。为了让函数的调用者知道x1和x2中包含的是否为有效的解,我们让函数返回一个整型数，该整型数表示解的情况。
```cpp
//函数的定义
int SolveQuadratic(double a,double b,double c, double *px1,double *px2) {
    double delta, sqrtDelta;
    //不是一元二次方程
    if(a == 0) return 3; 
    
    delta = b * b - 4 * a * c;
    //无根
    if( delta < 0 ) return 2; 
    //等根
    if ( delta == 0 ) { *px1 = -b /2 * a}; return 1; } 
    //两个不等根
    sqrtDelta = sqrt(delta);
    *px1 = (-b + sqrtDelta) / (2 * a);
    *px2 = (-b - sqrtDelta) / (2 * a);
    return 0;
}

//函数的调用：

int main() {
    double a,b,c,x1,x2;
    int result;
    cout << "请输入a,b,c: ";
    cin >> a >> b >> c;
    result = SolveQuadratic(a, b, c, &x1, &x2);
    switch (result) {
    case 0: cout << "方程有两个不同的根：x1 = " << x1 << " x2 = " << x2; break;
    case 1: cout << "方程有两个等根：" << x1; break;
    case 2: cout << "方程无根"; break;
    case 3: cout << "不是一元二次方程";
    }
    return 0;
}
```

**数组传递的进一步讨论**：数组传递的本质是地址传递，因此形参和实参用数组名和指针是一样的。数组传递时，函数原型可写为：
 ```cpp
void fun(T a[], int n); //T表示int、char等类型,此处只是代替一下！
   
//也可写为 
void fun(T *n, int n);
 ```
在函数内部，a和p都能当作数组使用。调用时，对这两种形式都可用数组名或指针（都是地址）作为实参。建议如果传递的实参是数组的起始地址，用第一种形式；如果传递的实参是普通的指针，用第二种形式。
```cpp
#include <iostream>  
using namespace std;  

void f(int arr[], int k) {  
    cout << sizeof(arr) << " " << sizeof(k) << endl;  
}  

int main() {  
    int a[10]={1,2,3,4,5,6,7,8,9,0};  
    cout << sizeof(a) << endl;  
    f(a,10);  
    return 0;  
}  
//输出：
40
4  4
//C++把数组名作为参数传递的情况处理成指针的传递
```
数组传递具有灵活性：
```cpp
void sort(int p[],int n){//···}
int main(){
	int a[];
	//···
	sort(a,100);    //排序整个数组
	sort(a,50);     //整个数组的50个数组
	sort(a+50,50);  //整个数组的后50个元素
}
```
接下来我们用分治法在一个整数数组找出最大和最小值：
```cpp
void minmax(int a[],int n, int *min_ptr,int *max_ptr){
	innt min1,max1,min2,max2
	switch(n){
	case 1:*min_ptr=*max_ptr=a[0];return;//最大最小都是a[0]
	case 2: if (a[0] < a[1]) { *min_ptr = a[0]; *max_ptr = a[1]; }
		    else { *min_ptr = a[1]; *max_ptr = a[0]; }
		    return;//大的放入*max_ptr,小的放入*min_ptr
	default:minmax(a , n/2 , &min1,&min2);//确保被调函数的值返回主调函数“&”
			minmax(a+n/2 , n-n/2, &min2, &max2 );//分
		    if (min1 < min2) *min_ptr = min1;//注意，这里不能是min_ptr=&min1;
		    else *min_ptr = min2;
		    if (max1 < max2) *max_ptr = max2;
		    else *max_ptr = max1;//治
		    return;
	}
```

**返回指针的函数**：
函数原型：类型 \*函数名 （形式参数表）
当返回值是指针时，返回地址对应的变量不能是一个局部变量（容易出现错误，而且暴露与否具有随机性）。我们实现一个交换三个数中最大最小值的函数：
```cpp
//交换a、b、c中的最大值和最小值
int *max(int *a, int *b, int *c) { //返回最大的变量的地址
    if (*a>*b)
    if (*a>*c) return(a); else return(c);
    else if (*b>*c) return(b); else return(c);
}

int *min(int *a, int *b, int *c) { //返回最小的变量的地址
    if (*a<*b)
    if (*a<*c) return(a); else return(c);
    else if (*b<*c) return(b); else return(c);
}

void swap(int *a, int *b) {
    int c;
    c = *a; *a = *b; *b = c;
}

//交换a，b，c中的最大值和最小值  
int main() {  
    int x, y, z;  
    cin >> x >> y >> z;  
    cout << endl << x << ‘\t’ << y << ‘\t’ << z;  
    swap(max(&x, &y, &z), min(&x, &y, &z));  
    cout << endl << x << ‘\t’ << y << ‘\t’ << z;  
    return 0;  
}
```
类似的，可以返回数组的起始地址，但不能是局部变量。

## 6、引用类型与函数
**C++中的引用：**
引用,即给一个变量取一个别名。例：
```cpp
int i;
int &j = i;//j是i的别名，i与j是同一个内存单元
```
有了以上定义后，使用j就等于使用i。引用实际上是一种隐式指针，相当于省去了`*`。
注意到，定义引用时必须立即对它初始化，不能定义完成后再赋值。如：
```cpp
int i;
int &j; //错误
j = i;
```
为引用提供的初始值可以是一个变量或另一个引用。如：
```cpp
int i = 5;
int &j1 = i;
int &j2 = j1;
```
引用不可重新赋值，不可使其作为另一变量的别名。
```cpp
int i, k;
int &j = i;
j = &k;  //错误（注：指针可以指向另一个变量，有区别）
```

**引用使用的注意事项：** 
引用不是一种数据类型，下面的“类型说明”都是非法的：
```cpp
int &&r= ……;     //不能建立引用的引用（&&表示"且"）
int &a[3] = ……;  //不能建立引用的数组
int &*p = ……;    //不能建立引用的指针（但可以有指针的引用 int *&p=j）
```

**引用参数：** 
C++中引入引用的主要目的是将引用作为函数的参数
```cpp
#指针参数
void swap(int *m, int *n) {
    int temp;
    temp=*m;
    *m=*n; *n=temp;
}
//调用：
swap(&x, &y)

#引用参数
void swap(int &m, int &n) {
    int temp;
    temp=m;
    m=n; n=temp;
}
//调用：
swap(x, y)//实参必须是变量，不能是表达式！
```
**参数的引用传递：**
```cpp
void f (int &r) {
    cout << "r = " << r <<endl;
    cout << "&r = " << &r <<endl;
    r=5;
    cout << "r = " << r <<endl;
}

int main() {
    int x=47;
    cout << "x = " << x <<endl;
    cout << "&x = " << &x <<endl;
    f(x); cout << "x = " << x <<endl;
    return 0;
}


# 执行结果
x = 47  
&x = 0x0065FE00  
r = 47  
&r = 0x0065FE00  
r = 5  
x = 5  
```
引用传递是地址传递的另一种更简单明了的实现方法。在C++程序中，函数参数经常采用引用传递。利用引用传递的好处是减少函数调用时的开销：不需要分配新的内存空间；不需要复制实际参数的数值。如果在函数内不许改变参数的值，则形式参数用const限定。对非const的形式参数采用引用传递时，实际参数不能是常量或临时量。

**返回引用的函数**：函数的返回值可以是一个引用。它表示函数的返回值是函数内某一个变量（不能是局部变量）的引用。返回引用的函数原型：`类型 &函数名 (形式参数表)`
```cpp
//交换a,b,c中的最大值和最小值
int &max(int &a, int &b, int &c) { //返回最大的变量的地址
    if (a>b)
    if (a>c) return(a); else return(c);
    else if (b>c) return(b); else return(c);
}

int &min(int &a, int &b, int &c) { //返回最小的变量的地址
    if (a<b)
    if (a<c) return(a); else return(c);
    else if (b<c) return(b); else return(c);
}

void swap(int &a, int &b) {
    int c;
    c = a; a = b; b = c;
}

```

**特别用途**：将函数用于赋值运算符的左边，即作为左值。
```cpp
int a[] = {1, 3, 5, 7, 9};
int &index(int); //声明返回引用的函数
void main() {
    index(2) = 25; //将a[2]赋值为25
    cout << index(2);
}

int &index(int i) { return a[i]; } //函数是a[i]的一个引用
```
当有多个程序员协作时，引用可以使用另一个源文件中的封装数据。
```cpp
#file1.cpp

int &index(int); // 声明返回引用的函数

int main() {
    index(2) = 25; // 通过接口将a[2]赋值为25
    cout << index(2); // 通过接口输出a[2]的值
    return 0;
}


#file2.cpp

int a[] = {1, 3, 5, 7, 9}; // 被封装的全局变量数组a
//数组a的接口函数
// 函数返回a[i]的引用（别名）
int &index(int i) {
	assert(j>=0 && j<5);//进行边界检查
    return a[i]; 
}
```
下面是一个综合性较强的例子，可以帮助加深理解：
```cpp
char &Good() {
    char *p = new char[5];
    strcpy(p, "good");
    return *p;
}

int main() {
    char &a = Good();
    cout << a;
    cout << &a;
    return 0;
}

//左侧程序中两次输出的内容分别是：(C)
//A: good、a的地址  
//B: g、a的地址  
//C: g、good  
//D: 以上皆错
```
## 7、指针数组与多级指针
**指针数组**:
指针本身也是变量，它们也可以像其他变量一样存储在数组中。一个数组，如果它的元素均为指针，则称为指针数组。一维指针数组的定义形式：`类型名 *数组名[数组长度]；`
例如，`char *words[10]；` 定义了一个名为 words 的指针数组，该数组有 10 个元素，每个元素是一个指向字符的指针。

**指针数组的应用**:比如用来指向一组字符串。
例：写一个函数用二分法查找某一个城市在城市表中是否出现，用递归实现。
```cpp
#二分查找函数

//该函数用二分查找在cityTable中查找cityName是否出现
//lh和rh表示查找范围，返回出现的位置
int binarySearch(char *cityTable[], int lh, int rh, char *cityName) {
    int mid, result;

    while (lh <= rh) {
        mid = (lh + rh) / 2;
        result = strcmp(cityTable[mid], cityName);
        if (result == 0) return mid; //找到
        else if (result > 0) return binarySearch(cityTable, lh, mid-1, cityName);
        else return binarySearch(cityTable, mid+1, rh, cityName);
    }
    return -1; //没有找到
}

#主程序

#include <iostream>
#include <string>
using namespace std;

int binarySearch(char *cityTable[], int lh, int rh, char *cityName);

int main() {
    char *cities[10] = {"aaa", "bbb", "ccc", "ddd", "eee", "fff", "ggg", "hhh", "iii", "jjj"};
    char tmp[10];

    while (cin >> tmp) 
        cout << binarySearch(cities, 0, 9, tmp) << endl;

    return 0;
}
```

**多级指针：**
指针指向的内容还是一个指针，称为多级指针。如有定义：`char *words[10]；`words 是一个数组，数组元素可以通过指针来访问。如果指针指向数组 words 的某一个元素，那么指向的内容就是一个指向字符的指针，因此 p 就是一个二级指针。
定义指针的方法：二级指针 `类型名 **变量名；`  三级指针 `类型名 ***变量名；` 
如`int **q；` 表示 q 指向的内容是一个指向整型的指针。可以这样使用：
```cpp
int x = 15, *p = &x; q = &p;
//Q  →  p  →  15
```
同样，
```cpp
char **s；//表示 s 指向的内容是一个指向字符的指针。
//s →  → "abcde"
```

**多级指针的应用**：
可以用指向指针的指针访问指针数组的元素。如：
```cpp
#include <iostream>
using namespace std;

int main() {
    char *city[] = {"aaa", "bbb", "ccc", "ddd", "eee"};
    char **p;

    for (p = city; p < city + 5; ++p)
        cout << *p << endl;
    
    return 0;
}

//输出结果：
//aaa
//bbb
//ccc
//ddd
//eee
```

**动态二维数组**:
二位数组与指针：
```cpp
int a[3][4]; // 等价于定义了 3 个指针常量

//内存布局：
//a → a[0] → 1 2 3 4
//    a[1] → 5 6 7 8  
//    a[2] → 9 10 11 12
```
动态二维数组：
```cpp
int **a; a = new int*[3]; // 动态定义了3个指针变量
    for(int i=0; i<3; ++i) a[i] = new int[4];
```

**指向一维数组的指针** ：
对于`int a[3][4]`，`a[i]`是一个指针数组，它的每个元素是一个整型指针，指向第i行的第零个元素。数组名a是一个指向一维指针数组的指针常量，指向二维数组a的第零个元素(即第零行`a[0]`)。
对a加1，事实上是跳到下一行，即(a+1)指向`a[1]`。指向一维数组的指针可以这样定义：
```cpp
类型名（*指针变量名）一维数组的元素个数;
//定义了一个数组指针
//注意：数组指针不是指针数组，圆括号不能省略，如果省略了圆括号就变成了指针数组
```
等价于`a[i][j]`的表达式
```cpp
int a[i][j];
*(a[i]+j)
(*(a+i))[j]
*((*(a+i))+j)
*(&a[0][0]+4*i+j)
```
输出数组 int `a[3][4]` 的所有元素：
```cpp
int a[3][4]
int (*p)[4], *q;

for (p = a; p < a + 3; ++p) {
    for (q = *p; q < *p+4; ++q)
    cout << *q << '\t';
    cout << endl;
}
```
注意：如果输出`a`和`a[0]`，这两个值是相同的。但是，这两个值的含义是不同的，前者是第0行的首地址，它的类型是由四个元素组成的一维整型数组指针，后者是第0行第一个元素的地址，它的类型是整型指针。