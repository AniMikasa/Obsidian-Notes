[[(N)Summary#C++ 运算符总结|运算符总结]]
## 一、运算符重载的概念与限制  
- 允许为 `class` 类型重载内置运算符，不能创建新运算符。  
- 不能改变运算符的优先级、结合性或操作数个数。  
- 函数名必须为 `operator@`（@ 为运算符符号）。  
- 可重载为全局函数或成员函数；某些运算符（如 `=`、`[]`、`()`、`->`）只能重载为成员函数。  
- 全局函数需访问私有成员时，应声明为类的友元。

---

## 二、运算符重载函数的参数与返回类型  
- **全局函数**：参数个数等于运算符操作数个数。  
- **成员函数**：隐含 `this` 指针作为左操作数，参数个数比操作数少一个。  
- 通常需要返回值：若结果为当前对象本身，应返回对象的引用。

`+ * < ==`的运算符重载：可以重载为全局函数和成员函数，建议重载为全局函数
```cpp
//这里以成员函数为例，这三个运算符也可以重载为全局函数，此时函数有两个常值引用参数，建议重载为全局函数，这样程序灵活性更高，比如可以计算2+r，程序会调用构造函数，用2作为参数进行构造，但如果是成员函数，这个表达是不合法的
class Rational{
	private:
	    int num;
	    int den;
	    void ReductFraction();
	public:
	    Rational (int n = 0, int d = 1) { num = n; den = d; ReductFraction();}
	    Rational operator + (const Rational &rl) const; //运算符重载
	    Rational operator * (const Rational &rl) const; //运算符重载
	    bool operator < (const Rational &rl) const; //运算符重载
	    bool operator == (const Rational &rl) const; //运算符重载
	    void display() const { cout << num << '/' << den;}
};
Rational Rational::operator+(const Rational &rl) const
{
    Rational tmp;

    tmp.num = num * r1.den + r1.num * den;
    tmp.den = den * r1.den;
    tmp.ReductFraction();
	return tmp;
}
Rational Rational::operator*(const Rational &rl) const
{
    return Rational(num * r1.num,den * r1.den);//注意这里直接返回临时对象，效率更高
}

bool Rational::operator < (const Rational &rl) const
{
    return num * r1.den < den * r1.num;
}

bool Rational::operator == (const Rational &rl) const
{
    return num == r1.num && den == r1.den;
}
```


下标运算符的重载：必须重载为成员函数
```cpp
A &operator[] (int index);


double & DoubleArray::operator[](int index){
	assert (index>=low || index <= high);//#include <cassert>
	return storage[index - low];
}
```

函数调用运算符初始化：必须重载为成员函数
```cpp
//函数调用运算符重载后可以把当前对象当成函数使用
返回类型 operator()(形式参数表);

DoubleArray operator() (int start, int end, int lh)
{
    assert(start <= end && start >= low && end <= high); //判断范围是否正确
    DoubleArray tmp(lh, lh + end - start);
    for (int i = 0; i < end - start + 1; ++i)    //为返回的数组准备空间
    tmp.storage[i] = storage[start + i - low];
	return tmp;
}
```


---

## 三、具有赋值性质的运算符重载  

- 若未定义赋值运算符，编译器生成默认版本（按成员赋值）。  
- 类包含指针且不应共享目标时，需自定义赋值运算符与拷贝构造函数。  
- 赋值运算符中应先检查自赋值情况。  
- `++` 和 `--` 前缀为一元运算符，后缀为二元运算符（后缀第二个操作数为整型，编译器自动置 0）。  
- 赋值、复合赋值、自增自减（前缀）建议重载为成员函数，并返回当前对象的引用。

赋值运算符的重载：必须重载为成员函数
```cpp
A &operator= (const A &a);

DoubleArray &DoubleArray::operator=(const DoubleArray &right)
{
    if (this == &right) return *this;   // 防止自己复制自己
    delete [] storage;                  // 归还空间
    low = right.low;
    high = right.high;
    storage = new double[high - low + 1];
    for (int i=0; i <= high - low; ++i)
    storage[i] = right.storage[i];
    return *this;
}

//a=b+c时，可以调用移动构造函数
DoubleArray &DoubleArray::operator=(const DoubleArray &&right)
```




`++`和`--`运算符的重载:
可被重载成成员函数或友元函数。但因为这两项运算符改变了运算对象的状态，所以更倾向于将它们作为成员函数。作为前缀使用时，运算结果是修改以后的对象引用；作为后缀使用时，运算结果是修改以前的对象值。但问题是，处理++的两个重载函数的原型除了返回类型不同之外，其他完全相同的，处理--的两个重载函数也是如此。而仅返回类型不同的两个函数无法形成重载函数。为了解决这个问题，C++规定后缀运算符重载函数接收一个额外的（即无用的）int型的形式参数。使用后缀运算符时，编译器用0作为这个参数的值。当编译器看到一个前缀表示的++或--时，调用正常重载的这个函数。如果看到的是一个后缀表示的++或--，则调用有一个额外参数的重载函数。这样就把前缀和后缀的重载函数区分开了。
```cpp
Rational &Rational::operator++()     //前缀++
{
    num += den;
    return *this;
}

Rational Rational::operator++(int x) //后缀++
{
    Rational tmp = *this;
    num += den;
    return tmp;
}

Rational &Rational::operator--()     //前缀--
{
    num -= den;
    return *this;
}

Rational Rational::operator--(int x) //后缀--
{
    Rational temp = *this;
    
    num -= den;
    return tmp;
}
```

---

## 四、输入输出运算符重载  
- 必须重载为全局函数（因左操作数为 `istream/ostream` 对象）。  
- 第一个参数为 `istream/ostream` 的非常量引用。  
- 通常声明为类的友元以访问私有成员。  
- 输入运算符的第二个参数不能为 `const`。

`<<`
流插入运算符 `<<` 是一个二元运算符。例如，表达式 `cout << x` 的运算符两侧分别是 `cout` 和 `x`，`x` 是一个整型变量，`cout` 是输出流类型 `ostream` 的对象。`<<` 运算符将右边对象的值转换成文本形式插入左边的输出流对象，执行结果是左边的输出流对象的引用。对于 `cout << x`，运算结果为对象 `cout`。正因为 `<<` 运算的结果是左边对象的引用，所以允许执行 `cout << x << y` 等的操作。因为 `<<` 是左结合的，所以上述表达式先执行 `cout << x`，执行的结果是对象 `cout`，然后执行 `cout << y`。
```cpp
ostream & operator<< (ostream &os, const ClassType &obj){
	os << 要输出的内容;
	return os;
}

ostream& operator << (ostream &os, const Rational& obj){
	os<<obj.num<<'/'<<obj.den;
	return os;
}
```


 `>>`
与流插入运算符类似，流提取运算符也是一个二元运算符。对于 `cin >> x`，两个运算符分别是 `cin` 和 `x`，`cin` 是输入流类 `istream` 的对象，`>>` 操作从对象 `cin` 提取数据存入变量 `x`，运算结果是左边对象的引用。因此，`>>` 运算符也可以连用，例如，`cin >> x >> y >> z`。当重载 `>>` 时，重载函数的第一个参数是一个输入流对象的引用，返回的也是对同一个流的引用；第二个形式参数是右边对象的非常量引用，该形式参数必须是非常量的，因为流提取运算符重载函数的目的是将数据存入此对象。
```cpp
istream & operator>>(istream & is, ClassType &obj)
{
    is >> 对象的数据成员;
    return is;
}

istream& operator>>(istream &in, Rational& obj)
{
    in >> obj.num >> obj.den;
    obj.ReductFraction();
    return in;
}
```

---

## 五、自定义类型转换  

**从内置类型转为类类型**：
内置类型到类类型的转换是通过类的构造函数实现的。例如，对于 `Rational` 类的对象 `r`，我们可以执行 `r = 2`。此时，编译器隐式地调用 `Rational` 类的构造函数，将 `2` 作为实际参数，构造出一个 `num=2`、`den=1` 的 `Rational` 类的对象，并将它赋予 `r`。同理，如果有理数类将加法重载成友元函数，还允许执行 `r1 = 2 + r2` 等的运算。在调用函数 `operator+` 时，会调用构造函数将 `2` 作为参数构造一个有理数类的对象传给 `operator+` 的第一个参数。只要某个类 `A` 有一个单个参数的构造函数，C++就会执行参数类型到 `A` 类型对象的隐式转换。
但有时程序员并不希望编译器执行这种隐式转换，例如 `DoubleArray` 类有一个构造函数 `DoubleArray(int size)`，对 `DoubleArray` 类的对象 `array`，表达式：`array=8;`
是合法的，该表达式调用 `DoubleArray(8)` 构造一个下标范围是 `0~7` 的临时对象赋予 `array`。显然，这个赋值含义不明。为了避免这种问题，程序中可以禁止这种隐式转换。禁止隐式转换是在构造函数前加一个关键字 `explicit`。如果将 `DoubleArray` 的构造函数改为：
```cpp
explicit DoubleArray(int size){}
```
再遇到 `array = 8` 这样的语句时，编译器会报错。通常，除非有明显的需要隐式转换的理由，否则单个参数的构造函数应该被设为 `explicit`。将构造函数设置为 `explicit` 可以避免错误。当需要转换时，用户可以显式地构造对象，例如，执行 `array = DoubleArray(8)`。


**从类类型转为其他类型**：需定义类型转换运算符函数，原型为：  
  ```cpp
  operator 目标类型名 ( ) const
{
    return (结果为目标类型的表达式);
}
  ```
  无参数和返回类型声明，但函数体内需返回目标类型的对象。
```cpp
operator double ( ) const { return (double(num)/den); }
```
有了这个函数，就可以将一个 Rational 类的对象 r 赋予一个 double 型的变量 x。例如，r 的值为 (1,3)，经过赋值 x = r 后，x 的值为 0.333333。
利用类型转换函数实现隐式转换可以避免编写运算符的重载函数，从而减少代码。例如，尽管有理数类没有重载减法运算符，但可以执行 r - 1.5 等的操作，也可以执行 r1 = r2 - r3。在执行这类操作时，C++ 会将有理数 r 转换成 double 型的数据，执行 double 型的减法，计算结果也是 double 型的。
自动类型转换有时会给程序带来一些意想不到的结果，给程序的调试带来麻烦。禁止由带一个参数的构造函数引起的自动类型转换可以在构造函数前加一个关键字 explicit。C++11 将此类键字扩展到所有的类型转换函数。如果在类型转换函数前加了关键字 explicit，则表明不支持自动类型转换，但支持显式的类型转换。例如，在有理数类中，将有理数到 double 型的转换函数定义为：
```cpp
explicit operator double ( ) const { return (double(num)/den); }
```
如果 x 为 double 型的变量，r 为 Rational 类的对象，以下语句：
```cpp
x=r;
```
将导致一个编译错误。而合法的语句是:
```cpp
x=double(r);
```


---




