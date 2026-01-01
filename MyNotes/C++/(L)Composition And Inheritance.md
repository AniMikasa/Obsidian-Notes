## 一、组合
数据成员也可以是某个类的对象。如果一个类的某个数据成员是另一个类的对象，则该成员被称为对象成员。用组合的方式构建新的类时，程序员不必关心对象成员所属的类是如何实现的。

大多数对象成员不能像内置类型一样直接赋值，所以它的初值不一定能在构造函数体中通过赋值得到，而必须调用它的构造函数。因此用组合方式实现的类的构造函数往往带有初始化列表，在初始化列表中指定对象成员的初值。

**例 12.1** 定义一个复数类，复数的虚部和实部都用有理数表示。其需要实现的功能有复数的输入输出、复数的加法。

由于第11章已经定义了一个有理数类，则复数类的数据成员可以直接用有理数类的对象，因此复数类是一个用组合方法构建的类。它的定义如代码清单 12-1 所示。

**代码清单 12-1 Complex 类的定义**

```cpp
class Complex
{
    friend Complex operator + (const Complex &x, const Complex &y);
    friend istream& operator>>(istream &is, Complex &obj);
    friend ostream& operator<<(ostream &os, const Complex &obj);

private:
    Rational real; //实部
    Rational imag; //虚部
public:
    Complex(int r1 = 0, int r2 = 1, int i1 = 0, int i2 = 1)
        : real(r1, r2), imag(i1, i2) {}
    Complex(const Rational &rr, const Rational &ii)
        : real(rr), imag(ii) {}
};
```

注意 Complex 类的第一个构造函数。由于 real 和 imag 都是 Rational 类型，无法直接赋值，因此这两个数据成员都必须在初始化列表中用它们的构造函数设置初值，构造函数所需的参数来自 Complex 类的构造函数的参数表。除了这两个数据成员外，Complex 类没有其他的数据成员要设置初值，所以构造函数的函数体为空。

如果对象成员的初始化是由默认构造函数完成的，则该对象成员可以不出现在初始化列表中。

由此可见，组合可以简化创建新的工作，让创建新的程序员不需要过多地关注底层的细节，以便在更高的抽象层次上考虑问题，有利于创建功能更强的类。


## 二、继承

继承是软件重用的另一种方式，在程序设计中，程序员可以通过继承在某个已有类的基础上增加新的属性和行为来创建功能更强的类。

用继承方式创建新类时，程序员需要指出这个新类是在哪个已有的基础上扩展的。已有的类称为基类或父类，新类称为派生类或子类。派生类本身也可能会成为未来派生类的基类，又可以派生出功能更强的类。

派生类通常添加了其自身的数据成员和成员函数，因而比基类包含的内容更多，功能更强。派生类比基类更具体，它代表了一组外延较小的对象。继承的魅力在于能够添加基类所没有的特点以及取代和改进从基类继承的特点，使基类代码在派生类中得到重用。

继承机制除了可以支持软件重用之外，它还具有以下作用。
	（1）对事物进行分类，使对象之间的关系更加清晰。通过类之间的继承关系，可以把事物（概念）以层次结构表示出来。在这个层次结构中，存在着一种一般与特殊的关系，即 is-a 的关系。上层（基类）表示一般的概念，下层（派生类）表示特殊的概念，它是上层的具体化。
	（2）支持软件的增量开发。软件的开发往往不是一次完成的，而是随着对软件功能的逐步理解和改进不断完善的。继承关系可以用来实现这个完善的过程。通过扩充类的功能扩充整个软件的功能，整个程序的修改量可达到最少。
	（3）实现运行时的多态性，简化软件。

**派生类的定义**：
定义派生类需要指出它从哪个基类派生、它在基类的基础上增加了什么内容。
```cpp
class 派生类名: 继承方式 基类名
{
   新增加的成员声明;
}
```
派生类名是正在定义类的名称；基类名表示派生类是在这个类的基础上扩展的；继承方式可以是 public、private 和 protected，说明了基类的成员在派生类中的访问特性。继承方式可以省略，默认为 private。新增加的成员声明是派生类在基类基础上的扩充。例如：
```cpp
class Base
{
    int x;
public:
    void setx(int k);
};

class Derived1: public Base
{
    int y;
public:
    void sety(int k);
};
//类 Derived1 在类 Base 的基础上增加了一个数据成员 y 和一个成员函数 sety。因此 Derived1 有两个数据成员 x 和 y，有两个成员函数 setx 和 sety。
```



**访问特性**：protected 成员是一类特殊的私有成员，它不可以被全局函数或其他类的成员函数访问，但能被派生类的成员函数和友元函数访问。除了私有成员之外，基类其他成员在派生类中的访问特性取决于它在基类中的访问特性和继承方式。通常继承时采用的继承方式都是 public。它可以在派生类中保持基类的访问特性。

| 基类成员的访问特性 | public 继承 | protected 继承 | private 继承 |
|--------------------|--------------|----------------|--------------|
| public             | public       | protected      | private      |
| protected          | protected    | protected      | private      |
| private            | 不可访问     | 不可访问       | 不可访问     |

**派生类对象的构造和析构**：

派生类对象中包含了一个基类对象以及新增加的部分，初始化派生类对象需要同时初始化这两个部分。基类的数据成员往往都是私有的，派生类的成员函数无权访问这些私有成员，构造函数也不例外。因此，派生类对象的初始化通常由基类和派生类的构造函数共同完成。派生类的构造函数负责初始化新增加的数据成员，并在它的初始化列表中调用基类的构造函数初始化基类的数据成员。派生类构造函数的一般形式如下：

```cpp
派生类构造函数名(参数表): 基类构造函数名(参数表)
{
    ...
}
```

其中，基类构造函数中的参数值通常源于派生类构造函数的参数表，也可以用常量值。

构造派生类对象时，先执行基类的构造函数，再执行派生类的构造函数。如果派生类新增的数据成员是对象成员，则在创建对象时，先执行基类的构造函数，再执行对象成员的构造函数，最后执行自己的构造函数。

如果派生类对象构造时对包含的基类对象是用基类默认的构造函数初始化的，那么在派生类构造函数的初始化列表中可以不出现基类构造函数调用。注意：虽然没有显式地调用基类的构造函数，但基类的构造函数还是被调用了，而且调用的是默认的构造函数。

析构的情况也是如此。派生类的析构函数只负责新增加的数据成员的析构，派生类中的基类部分由基类的析构函数析构。派生类的析构函数在执行完函数体的语句后会自动调用基类的析构函数。

**例 12.4** 定义一个表示人的类 People，每个人包含两个信息：姓名和年龄。在 People 类的基础上，派生出一个表示学生的类 Student，每名学生有一个学号和班级号。观察学生类对象的构造和析构过程。

为了说明构造和析构过程，每个类只定义了构造函数和析构函数，构造函数和析构函数都会输出一条信息。按照这个思想，People 类的定义与实现如代码清单 12-8 所示，Student 类的定义与实现如代码清单 12-9 所示。

**代码清单 12-8 People 类的定义与实现**

```cpp
class People {
public:
    People(const char *s, int a) { //构造函数
        name = new char[strlen(s) + 1];
        strcpy(name, s);
        age = a;
        cout << "People constructor: [" << name << "] age : " << age << endl;
    }
    ~People() { //析构函数
        cout << "People destructor: [" << name << "] age : " << age << endl;
        delete [] name;
    }
protected:
    char *name;
    int age;
};
```

**代码清单 12-9 Student 类的定义与实现**

```cpp
class Student: public People {
public:
    Student(const char *s, int a, int n, const char *cls): People(s, a)
    {
        s_no = n;
        class_no = new char[strlen(cls) + 1];
        strcpy(class_no, cls);
        cout << "Student constructor: student number is " << s_no
             << ", class number is " << class_no << endl;
    }
    ~Student()
    {
        cout << "Student destructor: student number is " << s_no
             << ", class number is " << class_no << endl;
        delete [] class_no;
    }
private:
    int s_no;
    char *class_no;
};
```

由代码清单 12-9 可以看出，Student 类在它的构造函数的初始化列表中调用了 People 类的构造函数，并将形式参数 s 和 a 传递给它，构造一个 People 类的对象，然后在构造函数体中为 s_no 和 class_no 赋初值。People 类和 Student 类的使用如代码清单 12-10 所示。

**代码清单 12-10 People 类和 Student 类的使用**

```cpp
int main()
{
    Student s1("li", 20, 29, "F1003001");
    cout << endl;
    Student s2("wang", 21, 30, "F1102008");
    cout << endl;

    return 0;
}
```

代码清单 12-10 所示的程序运行结果如下：

```
People constructor: [li] age : 20
Student constructor: student number is 29, class number is F1003001

People constructor: [wang] age : 21
Student constructor: student number is 30, class number is F1102008

Student destructor: student number is 30, class number is F1102008
People destructor: [wang] age : 21
Student destructor: student number is 29, class number is F1003001
People destructor: [li] age : 20
```

main 函数首先定义一个 Student 类的对象 s1。该定义先调用基类的构造函数，输出了第 1 行，再调用派生类的构造函数，输出了第 2 行。接着 main 函数输出一个空行，再定义一个 Student 类的对象 s2，该对象定义输出了第 4 行和第 5 行。main 函数又输出一个空行，程序结束。此时会消亡所有变量，这将调用对象的析构函数。析构的次序和构造的次序正好相反。程序先构造 s1，再构造 s2。析构时，先析构 s2，再析构 s1。析构 s2 会先执行 Student 类的析构函数，再执行 People 类的析构函数，于是输出了第 7 行和第 8 行。最后析构 s1，输出最后两行。

由此可见，派生类对象构造时，基类的构造函数和派生类的构造函数各司其职，派生类构造函数不必关心基类部分的构造。

### 12.2.3 重定义基类的函数

派生类是基类的扩展，它可以是对保存数据内容的扩展，也可以增加新的功能，或者扩展某个功能。当派生类对基类的某个功能进行扩展时，它定义的成员函数名可能会与基类的成员函数名重复。这就如同非智能手机升级为智能手机后，仍然有一个电源按钮。但按了电源按钮后，智能手机和非智能手机做的事情是不一样的。

派生类定义了一个与基类中某个成员函数原型完全相同的成员函数，称为重定义基类的函数。此派生类中拥有两个原型完全相同的成员函数。通过“派生类对象.成员函数”或“派生类指针->成员函数”的形式调用此函数时会有二义性。C++规定派生类对象调用的是派生类重定义的函数，基类的同名函数被隐藏了。

**例 12.5** 某银行有两类账户：一类是普通账户；另一类是允许透支的 VIP 账户。每个账户包括姓名、账号和余额，VIP 账户还必须包括透支额度、欠款总额和透支利率。每个账户需有存款、取款、查看账户各个属性和显示账户信息的操作，VIP 账户还必须有维护透支信息方面的操作。试设计两个类。

观察这两个类可以发现，VIP 账户是一类特殊的普通账户，它具备普通账户所有的属性和行为，因此这里可以采用继承的方式，在普通账户的基础上增加透支功能来派生一个 VIP 账户。

根据题意可以得到代码清单 12-11 所示的普通账户（Account）类定义成员函数的实现。

**代码清单 12-11 Account 类定义成员函数的实现**

```cpp
class Account
{
private:
    char name[20];
    long acctNum; //账户名
    double balance; //余额
public:
    Account(const char *s = "", long an = -1, double bal = 0.0)
        : acctNum(an), balance(bal) { strcpy(name, s); }
    void Deposit(double amt) { balance += amt; }
    void Withdraw(double amt);
    double getBalance() const { return balance; }
    const char *getName() const { return name; }
    int getAcctNum() const { return acctNum; }
    void ViewAcct() const;
};

void Account::Withdraw(double amt)
{
    if (amt <= balance)
        balance -= amt;
    else
        cout << "余额不够\n";
}

void Account::ViewAcct() const
{
    cout << "账户姓名：" << name << '\t';
    cout << "账号：" << acctNum << '\t';
    cout << "余额：" << balance << endl;
}
```

在此基础上可以扩展出 VIP 账户类。VIP 账户需要额外维护透支信息，因此增加了 3 个与透支有关的属性以及维护这些属性的函数。但注意虽然普通账户类有取款函数和显示账户情况的函数，但 VIP 账户的这两个功能的处理过程与普通账户的处理过程是不同的。VIP 用户取款时余额不够可以透支，显示账户信息时内容也更多。所以 VIP 类必须由程序员自行定义这两个函数，并让它们与基类的函数有同样的名称，即重定义了基类的函数。VIP 账户（AccountVIP）类的定义及部分成员函数的实现如代码清单 12-12 所示。

**代码清单 12-12 AccountVIP 类的定义及部分成员函数的实现**

```cpp
class AccountVIP : public Account
{
private:
    double maxLoan; //透支额度
    double rate;    //透支利率
    double owsBank; //欠款总额
public:
    AccountVIP(const char *s = "", long an = -1, double bal = 0.0,
               double ml = 500, double r = 0.11125) : Account(s, an, bal)
    {
        maxLoan = ml;
        rate = r;
        owsBank = 0;
    }
    AccountVIP(const Account & ba, double ml = 500,
               double r = 0.11125) : Account(ba)
    {
        maxLoan = ml;
        rate = r;
        owsBank = 0;
    }
    void ViewAcct() const;
    void Withdraw(double amt);
    void ResetMax(double m) { maxLoan = m; } //修改额度
    void ResetRate(double r) { rate = r; }   //修改利率
    void ResetOws() { owsBank = 0; }         //还款
};
```

VIP 账户信息包括账户的基本信息和透支信息。基本信息是从基类继承的，是基类的私有成员，派生类函数无法访问。所以在显示 VIP 账户信息时，必须调用基本账户类的几个 get 函数获取基本账户信息。AccountVIP 类 ViewAcct 函数的实现如代码清单 12-13 所示。

**代码清单 12-13 AccountVIP 类 ViewAcct 函数的实现**

```cpp
void AccountVIP::ViewAcct() const
{
    cout << "账户姓名：" << getName() << '\t';
    cout << "账号：" << getAcctNum() << '\t';
    cout << "余额：" << getBalance() << '\t';
    cout << "透支额度：" << maxLoan << '\t';
    cout << "欠款总额：" << owsBank << '\t';
    cout << "透支利率：" << 100 * rate << "%\n";
}
```

细心的读者可能发现，上述函数的一部分工作与基本账户类的 ViewAcct 函数完全相同。事实上，派生类重定义函数中往往都会包含基类同名函数的功能。能否直接调用基类的同名函数呢？如果直接调用 ViewAcct 函数，编译器会认为是递归调用，因为基类的同名函数是被隐藏的。调用基类的函数可以在函数名前加上基类的限定，即用“基类名::函数名”明确指出调用的是基类的函数。取款函数也是如此。当余额足够时，取款过程与普通账户的取款过程相同，可以调用基类的取款函数；余额不够时，需要进行额外的处理。AccountVIP 类利用基类函数实现的 ViewAcct 和 Withdraw 函数如代码清单 12-14 所示。

**代码清单 12-14 AccountVIP 类 ViewAcct 和 Withdraw 函数的实现**

```cpp
void AccountVIP::ViewAcct() const
{
    Account::ViewAcct();
    cout << "透支额度：" << maxLoan << '\t';
    cout << "欠款总额：" << owsBank << '\t';
    cout << "透支利率：" << 100 * rate << "%\n";
}

void AccountVIP::Withdraw(double amt)
{
    double bal = getBalance();
    if (amt <= bal) Account::Withdraw(amt);
    else if (amt <= bal + maxLoan - owsBank) { //透支额度内
        double advance = amt - bal; //计算本次透支额
        owsBank += advance * (1.0 + rate); //加入利息
        cout << "需要透支：" << advance << '\t';
        cout << "利息：" << advance * rate << endl;
        Deposit(advance);
        Account::Withdraw(amt);
    }
    else
        cout << "超出透支额度" << endl;
}
```

有没有更好的解决方案？答案是可以将基类的数据成员的访问特性设计成 protected，这样派生类的函数就可以直接调用基类的数据成员了。修改后的 Account 类的定义如代码清单 12-15。

**代码清单 12-15 采用保护成员的 Account 类定义**

```cpp
class Account
{
protected:
    char name[20];
    long acctNum;
    double balance;
public:
    Account(const char *s = "", long an = -1, double bal = 0.0)
        : acctNum(an), balance(bal) { strcpy(name, s); }
    void Deposit(double amt) { balance += amt; }
    void Withdraw(double amt);
    void ViewAcct() const;
};
```

**代码清单 12-16 从采用保护成员的 Account 类继承的 AccountVIP 类 ViewAcct 和 Withdraw 函数的实现**

```cpp
void AccountVIP::ViewAcct() const
{
    cout << "账户姓名：" << name << '\t';
    cout << "账号：" << acctNum << '\t';
    cout << "余额：" << balance << '\t';
    cout << "透支额度：" << maxLoan << '\t';
    cout << "欠款总额：" << owsBank << '\t';
    cout << "透支利率：" << 100 * rate << "%\n";
}

void AccountVIP::Withdraw(double amt)
{
    if (amt <= balance) balance -= amt;
    else if (amt <= balance + maxLoan - owsBank) { //透支额度内
        double advance = amt - balance;
        owsBank += advance * (1.0 + rate);
        cout << "需要透支：" << advance << '\t';
        cout << "利息：" << advance * rate << endl;
        balance = 0;
    }
    else
        cout << "超出透支额度" << endl;
}
```

虽然 protected 访问特性简化了派生类成员函数的实现，但也破坏了基类的安全性。例如，基类的数据成员 balance 应该只能由基类的存款和取款函数修改，但变成了保护成员后，派生类的函数也能修改，所以要慎用保护成员。

### 12.2.4 派生类对象的赋值

派生类不继承基类的构造函数，而是在派生类的构造函数中调用基类的构造函数。同样派生类也不继承基类的赋值运算符重载函数，但可以调用基类的赋值运算符重载函数。如果派生类没有定义赋值运算符重载函数，编译器会为它提供一个默认的赋值运算符重载函数。该函数对派生类新增加的数据成员对应赋值，对派生类中的基类对象调用基类的赋值运算符重载函数进行赋值。

如果默认的赋值运算符重载函数不能满足派生类的要求，则可以在派生类中重载赋值运算符。在派生类的赋值运算符重载函数中，显式调用基类的赋值运算符重载函数来实现基类成员的赋值。

假如要使代码清单 12-8 中的 People 类的对象之间能够赋值，程序必须重载赋值运算符。因为 People 类包含一个字符串的数据成员，所以不能用默认的赋值运算符重载函数。同理，由于 Student 类也扩展了一个字符串的数据成员，因此也必须重写赋值运算符重载函数。这两个函数的定义如代码清单 12-17 所示。

**代码清单 12-17 People 类和 Student 类的赋值运算符重载函数**

```cpp
People &operator=(const People &other)
{
    if (this == &other) return *this;
    delete [] name;
    name = new char[strlen(other.name) + 1];
    strcpy(name, other.name);
    age = other.age;
    return *this;
}

Student &operator=(const Student &other)
{
    if (this == &other) return *this;
    s_no = other.s_no;
    delete [] class_no;
    class_no = new char[strlen(other.class_no) + 1];
    strcpy(class_no, other.class_no);
    People::operator=(other);
    return *this;
}
```

注意 Student 类的赋值运算符重载函数。该函数在检查了是否自我赋值以后，先对 Student 类新增的数据成员 s_no 和 class_no 进行了赋值，然后调用基类的赋值运算符重载函数完成基类部分的赋值。

### 12.2.5 派生类作为基类

基类本身可以是一个派生类，例如：

```cpp
class Base {…};
class D1: public Base {…};
class D2: public D1 {…};
```

每个派生类继承它的直接基类的所有成员，而不用去管它的基类是完全自行定义的还是从某一个类继承的。也就是说，类 D1 包含了 Base 的所有成员以及 D1 增加的所有成员。类 D2 包括了 D1 的所有成员及自己新增的成员。对于代码清单 12-18 中的类，Base 包含一个数据成员 x；Derive1 从 Base 继承，又增加了一个数据成员 y，因此它包含两个数据成员 x 和 y。Derive2 从 Derive1 继承，又增加了一个数据成员 z，因此它包含 3 个数据成员 x、y 和 z。在定义 Derive2 的时候，不用去管 Derive1 是如何实现的，只需要知道 Derive1 有两个数据成员以及这两个数据成员的访问特性。

**代码清单 12-18 派生类作为基类实例**

```cpp
//派生类作为基类实例
class Base {
    int x;
public:
    Base(int xx) { x = xx; cout << "constructing Base\n"; }
    ~Base() { cout << "destructing Base\n"; }
};

class Derive1: public Base {
    int y;
public:
    Derive1(int xx, int yy): Base(xx) { y = yy; cout << "constructing Derive1\n"; }
    ~Derive1() { cout << "destructing Derive1\n"; }
};

class Derive2: public Derive1 {
    int z;
public:
    Derive2(int xx, int yy, int zz): Derive1(xx, yy) 
    { z = zz; cout << "constructing Derive2\n"; }
    ~Derive2() { cout << "destructing Derive2\n"; }
};
```

当构造派生类对象时，程序员不需要知道基类的对象是如何构造的，只需知道调用基类的构造函数就能构造基类的对象。如果基类是从某一个其他类继承的派生类，构造基类对象又会调用它的基类的构造函数，依次上溯。例如，Derive2 的构造函数的初始化列表只需要指出调用 Derive1 的构造函数，Derive1 的构造函数的初始化列表只需要指出调用 Base 的构造函数。当构造 Derive2 类的对象时，先调用 Derive1 的构造函数，而 Derive1 的构造函数执行时又会先调用 Base 的构造函数。因此，构造 Derive2 类的对象时，最先初始化的是 Base 的数据成员，再初始化 Derive1 新增的成员，最后初始化 Derive2 新增的成员。析构的过程正好相反。如果定义了一个代码清单 12-18 中的类 Derive2 的对象 obj，将会输出：

```
constructing Base
constructing Derive1
constructing Derive2
```

从中可以看出 obj 的构造过程；而该对象析构时，将会输出：

```
destructing Derive2
destructing Derive1
destructing Base
```

说明析构的过程是先执行派生类 Derive2 的析构函数，然后析构它包含的基类 Derive1 的对象。在执行 Derive1 的析构函数时，又会先执行 Derive1 的析构函数体，然后调用 Derive1 的基类 Base 的析构函数。

### 12.2.6 将派生类对象隐式转换为基类对象

要使 A 类对象能自动转换为 B 类对象，在 A 类中必须定义一个由 B 类转换的类型转换函数。由于派生类对象中包含了一个基类的对象，因此 C++规定派生类对象转换为基类对象时不必定义类型转换函数，默认转换结果是派生类对象中的基类部分。派生类对象到基类对象的隐式转换可能发生在以下 3 种场景。

1. **将派生类对象赋予基类对象**  
   将派生类中的基类部分赋予基类对象。此时，派生类新增加的成员就舍弃了。例如，对以下两个类：

```cpp
class Base {
    int x;
public:
    Base(int x1 = 0) { x = x1; }
    void display() const { cout << x << endl; }
};

class Derived: public Base {
    int y;
public:
    Derived(int x1 = 0, int y1 = 0): Base(x1) { y = y1; }
    void display() const { Base::display(); cout << y << endl; }
};
```

在派生类中，有两个数据成员 x 和 y。除了构造函数以外，有两个原型相同的公有的成员函数 display。

执行如下语句：

```cpp
Derived d(1, 2);
Base b;
```

构造对象 b 后，x 的初值是 0。当执行 `b = d` 后，将 d 对象中的基类部分赋予了 b，则 b 的 x 值为 1。  
该过程如图 12-1 所示。

**图 12-1 派生类对象赋予基类对象**

2. **基类指针指向派生类对象**  
   基类指针可以指向派生类对象。尽管该指针指向的对象是一个派生类对象，但由于它本身是一个基类的指针，只能解释基类的成员，而不能解释派生类新增的成员。因此，从指向派生类的基类指针出发，只能访问派生类中的基类部分。例如执行：

```cpp
Base *bp = &d;
bp->display();
```

尽管 Derived 类有两个 display 函数，但基类指针只有看见派生类中的基类部分。因此调用的是基类的 display 函数，如图 12-2 所示。

如果试图通过基类指针访问那些派生类增加的成员，编译器会报告语法错误。

3. **基类的对象引用派生类的对象**  
   引用事实是一种隐式的指针。当用一个基类对象引用派生类对象时，相当于给派生类中的基类部分取了一个名称，从这个基类对象看到的是派生类中的基类部分。例如，定义：

```cpp
Derived d(1,2);
Base &b = d;
```

则 b 是 d 对象中的基类部分的一个别名。对 b 的访问就是对 d 对象中的基类部分的访问，对 b 的修改就是对 d 对象中的基类部分的修改，如图 12-3 所示。

派生类的对象可以隐式地转换为基类的对象，这是因为派生类对象中包含了一个基类的对象。但基类对象无法隐式转换为派生类对象，因为它无法解释派生类新增的成员。除非在基类中定义了一个向派生类转换的类型转换函数，才能将基类对象转换为派生类对象。同样地，也不能将基类对象的地址赋予派生类的指针或将一个基类指针赋予一个派生类的指针，即使这些类指针指向的就是一个派生类的对象。例如，有定义：

```cpp
Derived d, *dp;
Base *bp = &d;
```

如果程序员能够确信该基类指针指向的是一个派生类的对象，确实想把这个基类指针赋予派生类的指针，这时可以用强制类型转换：

```cpp
dp = reinterpret_cast<Derived *>(bp);
```

这样相当于告诉编译器：我知道这个危险，但我保证不会出问题。reinterpret_cast 是一种相当危险的转换，它让编译器按程序的意图解释内存中的信息，将基类指针指向的内存中的内容强制看成派生类对象。

**图 12-2 基类指针指向派生类对象**  
**图 12-3 基类对象引用派生类对象**

## 12.3 运行时的多态性

### 12.3.1 多态性

在现实生活中，当向不同的人发出同样的一个命令时，不同的人可能有不同的反应。例如，学校给每名学生寝室的寝室长一个任务，让其每天早上督促同一寝室的同学去上课。由于每名同学都知道自己该去哪里上课，因此寝室长无须一一关照，而只需要对所有同学说一句“该上课去了”，每名同学都会去自己该去的教室。这就是多态性。

在程序设计中也是如此。如果对两个整型数和两个实型数发出同样的一个加法指令，由于整型数和实型数在计算机内的表示方法是不一样的，因此将两个整型数相加和将两个实型数相加的过程是不一样的，计算机会按不同的方法实现相应的功能。

有了多态性，相当于对象有了主观能动性，使用起来就方便了。特别是当被管理对象的数量和类型增加时，管理者的工作量并不增加。例如，对上例中的寝室长而言，一间寝室有 4 名同学和一间寝室有 10 名同学的工作量是一样的，其都只说一句话——“该上课去了”。寝室长的工作相当于 main 函数的工作。也就是说，只要增加对象类型，main 函数不变，系统的功能就可以得到扩展。同理，如果程序员定义了复数类，并重载了“+”运算，那么对两个复数类对象 c1 和 c2 可以执行 c1 + c2。在 C++编译器没有被修改的情况下，C++功能得到了扩展。

在面向对象的程序设计中，多态性有两种实现方式：编译时的多态性（或称静态绑定）和运行时的多态性（或称动态绑定）。第 11 章介绍的运算符重载和第 5 章介绍的重载函数都属于编译时的多态性。在运算符重载和函数重载时，尽管程序中使用的是同样的指令，如对整型变量 x、y 执行 x + y 和对有理数的对象 r1、r2 执行 r1 + r2，但在编译时编译器会把这两个表达式替换成不同的函数调用。前者调用两个参数是整型的 operator+ 函数，后者调用两个参数都是有理数类对象的 operator+ 函数。运行时的多态性是指同样的一条指令到底要调用哪个函数需执行到这一条指令时才能决定。运行时的多态性是通过虚函数和基类指针指向不同的派生类的对象实现的。

### 12.3.2 虚函数

虚函数是在类定义中的函数原型声明前加一个关键字 virtual。当包含虚函数的类作为基类，通过该类指针或基类的引用调用虚函数时，会先检查指针指向的是基类对象还是派生类对象。如果指向基类对象，则执行基类的虚函数。如果指向派生类对象，则检查派生类是否重定义了该函数。如果没有重定义，执行基类的虚函数，否则执行派生类重定义的函数。例如如下定义：

```cpp
class A {
public:
    virtual void f1(int);
    virtual void f2();
};

class B: public A {
public:
    void f1(int);
    void f2(int);
};

A *p1 = new A, *p2 = new B;
```

执行 `p1->f1(2)` 时，执行的是基类的 f1 函数，因为 p1 是基类指针指向基类对象。p2 是基类指针指向派生类对象。由于 p2 是基类指针，只有看到基类的部分，因此执行 `p2->f1(2)` 找到的是基类的 f1 函数。但由于 f1 是虚函数，因此检查 p2 到底指向谁。p2 指向的是 B 类对象，于是找到了 B 类中重定义的 f1 函数并执行。

使用虚函数时必须注意以下两个问题。

- 在派生类中重新定义虚函数时，它的函数原型必须与基类中的虚函数完全相同，否则编译器会认为派生类有两个重载函数。例如，B 类中的 f2 函数和从 A 类继承的 f2 函数形成了两个重载函数。
- 虚函数具有继承性。派生类中对虚函数的重定义函数默认也是虚函数——不管在重定义时有没有加 virtual。

为了保证派生类覆盖基类的虚函数时函数原型必须完全相同，C++11 采用了一个关键字 override 显示指定覆盖。如果覆盖的函数原型与基类的虚函数不一致，编译器会报错。关键字 override 放在派生类函数原型的最后。例如：

```cpp
class A {
public:
    virtual void f1(int);
    virtual void f2();
};

class B: public A {
public:
    void f1(int) override; //正确，f1 覆盖了基类的 f1
    void f2(int) override; //错误，f2 与基类的 f2 原型不符，编译器会报错
    int f3(int) override;  //错误，基类没有名为 f3 的虚函数，编译器会报错
};
```

C++11 还允许将某个虚函数指定为 final，表示它的派生类中不允许覆盖此函数。例如：

```cpp
class A {
protected:
    int a;
public:
    A(int x) { a = x; }
    virtual int f() { return a; }
};

class B: public A {
public:
    B(int x): A(x) {}
    int f() final { return 2 * a; }
};

class C: public B {
public:
    C(int x): B(x) {}
    int f() { return 3 * a; } // 错误，B::f() 是 final，不允许覆盖
};
```

按照虚函数的特性，B 类中的 f 函数也是虚函数。但由于此函数声明为 final，表示从 B 派生的类不允许覆盖此函数，因此编译器会对 C 类中的 f 函数做出错信息。

### 12.3.3 运行时的多态性

运行时的多态性是用虚函数实现的。如果从某个包含虚函数的基类派生出多个派生类，每个派生类都可能重新定义这个虚函数。当用基类的指针指向不同派生类的对象时，就会调用不同的函数，这样就实现了多态性。而这个绑定要到运行时根据当时基类指针指向的是哪一个派生类的对象，才能决定调用哪一个函数。因而被称为运行时的多态性。

例 12-5 中定义了一个普通账户类和一个 VIP 账户类，两个类都有显示账户信息的函数和取款函数。如果把基类的两个函数都定义成虚函数，那么主程序在显示账户信息和处理取款时不必考虑账户类型了。程序运行时会根据指针指向的对象调用相应的函数。修改后的 Account 定义如代码清单 12-19 所示。某次使用如代码清单 12-20 所示。

**代码清单 12-19 虚函数定义示例**

```cpp
class Account
{
protected:
    char name[20];
    long acctNum;
    double balance;
public:
    Account(const char *s = "", long an = -1, double bal = 0.0)
        : acctNum(an), balance(bal) { strcpy(name, s); }
    void Deposit(double amt) { balance += amt; }
    virtual void Withdraw(double amt);
    virtual void ViewAcct() const;
};
```

**代码清单 12-20 多态性示例**

```cpp
int main()
{
    Account *data[5];
    createAccount(data);
    for (int i = 0; i < 5; ++i)
        data[i]->ViewAcct();
    cout << endl;
    for (int i = 0; i < 5; ++i) {
        data[i]->Withdraw(150);
        data[i]->ViewAcct();
        delete data[i];
    }
    return 0;
}

void createAccount(Account *data[])
{
    const char *name[5] = {"aaa", "bbb", "ccc", "ddd", "eee"};
    for (int i = 0; i < 5; ++i)
        if (i % 2) data[i] = new Account(name[i], i + 10, i * 100);
        else data[i] = new AccountVIP(name[i], i + 10, i * 100, i * 100 + 500, i * 0.01);
}
```

程序定义了一个 Account 类的指针数组，它既可以指向普通账户对象，也可以指向 VIP 账户对象。首先调用 createAccount 函数生成了 5 个账户存放在数组中，奇数下标指向 Account 类对象，偶数下标指向 AccountVIP 类对象。在随后对账户进行操作时不需要再关心指针指向的是哪些账户，运行时的多态性会决定到底调用哪个函数。程序输出如下：

```
账户姓名：aaa  账号：10  余额：0   透支额度：500  欠款总额：0   透支利率：0%
账户姓名：bbb  账号：11  余额：100
账户姓名：ccc  账号：12  余额：200  透支额度：700  欠款总额：0   透支利率：2%
账户姓名：ddd  账号：13  余额：300
账户姓名：eee  账号：14  余额：400  透支额度：900  欠款总额：0   透支利率：4%

需要透支：150  利息：0
账户姓名：aaa  账号：10  余额：0   透支额度：500  欠款总额：150  透支利率：0%
余额不够
账户姓名：bbb  账号：11  余额：100
需要透支：50   利息：1
账户姓名：ccc  账号：12  余额：0   透支额度：700  欠款总额：51   透支利率：2%
余额不够
账户姓名：ddd  账号：13  余额：300
需要透支：0    利息：0
账户姓名：eee  账号：14  余额：250  透支额度：900  欠款总额：0   透支利率：4%
```

如果银行需要增加一类儿童账户，这类账户不能透支，而且开户时需要指定取款上限，因此它也可以从 Account 类派生。儿童账户除了增加一个取款上限的属性外，它的取款过程和显示账户信息的过程也与基本账户不同，所以也需要重定义类的这两个函数。儿童账户的定义如代码清单 12-21 所示。

**代码清单 12-21 儿童账户类型及成员函数的实现**

```cpp
class AccountBaby : public Account
{
private:
    double maxWithdraw;
public:
    AccountBaby(const char *s = "", long an = -1, double bal = 0.0,
                double md = 100): Account(s, an, bal), maxWithdraw(md) {}
    AccountBaby(const Account & ba, double md = 100):
        Account(ba), maxWithdraw(md) {}
    void ViewAcct() const;
    void Withdraw(double amt);
};

void AccountBaby::ViewAcct() const
{
    cout << "账户姓名：" << name << '\t';
    cout << "账号：" << acctNum << '\t';
    cout << "余额：" << balance << '\t';
    cout << "取现额度：" << maxWithdraw << '\n';
}

void AccountBaby::Withdraw(double amt)
{
    if (amt > balance)
        cout << "余额不够\n";
    else if (amt > maxWithdraw)
        cout << "超出取现额度\n";
    else balance -= amt;
}
```

增加了儿童账户类后，主程序保持不变。如果生成账户的函数是：

```cpp
void createAccount(Account *data[])
{
    const char *name[5] = { "aaa", "bbb", "ccc", "ddd", "eee" };
    for (int i = 0; i < 5; ++i)
        switch (i % 3) {
            case 0: data[i] = new AccountBaby(name[i], i+10, 200, (i+1)*50); break;
            case 1: data[i] = new Account(name[i], i+10, 200); break;
            default: data[i] = new AccountVIP(name[i], i + 10, 100, (i+1)*100, i*0.01);
        }
}
```

程序的输出结果如下：

```
账户姓名：aaa  账号：10  余额：200  取现额度：50
账户姓名：bbb  账号：11  余额：200
账户姓名：ccc  账号：12  余额：100  透支额度：300  欠款总额：0   透支利率：2%
账户姓名：ddd  账号：13  余额：200  取现额度：200
账户姓名：eee  账号：14  余额：200

超出取现额度
账户姓名：aaa  账号：10  余额：200  取现额度：50
余额不够
账户姓名：bbb  账号：11  余额：50
需要透支：50   利息：1
账户姓名：ccc  账号：12  余额：0   透支额度：300  欠款总额：51   透支利率：2%
余额不够
账户姓名：ddd  账号：13  余额：50   取现额度：200
余额不够
账户姓名：eee  账号：14  余额：50
```

### 12.3.4 虚析构函数

构造函数不能是虚函数，但析构函数可以是虚函数，而且最好是虚函数。如果派生类新增的数据成员中含有指向动态申请的内存的指针，那么派生类必须定义析构函数释放这部分空间。如果派生类的对象是通过基类的指针操作的，则删除基类指针指向的派生类对象时会造成内存泄漏。当基类指针指向的对象析构时，通过基类指针找到基类的析构函数，执行基类的析构函数；但此时派生类动态申请的空间没有释放，释放这些空间必须执行派生类的析构函数。

要解决这个问题，程序员可以将基类的析构函数定义为虚函数。这样，当析构基类指针指向的派生类对象时，会先找到基类的析构函数。由于基类的析构函数是虚函数，又会找到派生类的析构函数，执行派生类的析构函数。派生类的析构函数在执行时会自动调用基类的析构函数，因此基类和派生类的析构函数都被执行，这样就把派生类的对象完全析构，而不是只析构派生类的基类部分了。

与其他虚函数一样，析构函数的虚函数性质都将被继承。因此，如果继承层次树中的根类的析构函数是虚函数，所有派生类的析构函数都是虚函数。

## 12.4 纯虚函数和抽象类

纯虚函数是一个在基类中声明的虚函数。它在基类中没有定义，但要求在派生类中定义自己的版本。纯虚函数的声明形式如下：

```cpp
virtual 返回类型 函数名(参数表) = 0;
```

至少含有一个纯虚函数的类被称为抽象类。由于抽象类包含了一些无法运行的纯虚函数，因此程序中不能定义抽象类的对象，只能定义抽象类的指针。指针的作用是指向派生类对象。

抽象类的作用是作为基类，规范由它派生的派生类的行为，保证进入继承层次的每个类具有纯虚函数所要求的行为，避免了这个继承层次中的用户由于偶尔的失误（例如，忘了为它所建立的派生类提供继承层次所要求的行为）而影响系统正常运行。

例如，某个系统需要处理二维平面上的各种形状，如三角形、矩形、圆形等，为此需要定义各种形状的类。每种形状都需要有一个输出该形状面积的功能。为了保证每个类都具有这个功能，这里可以定义一个抽象类 Shape，其中包含一个纯虚函数 area。每一个具体形状类都要求从 Shape 类派生。如果某个具体形状类没有重定义 area 函数，则它包含了一个从基类继承而来的纯虚函数，因而无法定义它的对象。Shape 类的定义如代码清单 12-22 所示。

**代码清单 12-22 Shape 类的定义**

```cpp
class Shape {
protected:
    double x, y;
public:
    Shape(double xx, double yy) { x = xx; y = yy; }
    virtual double area() const = 0;
};
```


