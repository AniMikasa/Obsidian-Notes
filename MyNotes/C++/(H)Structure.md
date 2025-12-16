## 1、结构体概述

**结构体类型作用：**
结构体是所有结构体类型的统称，而非单一类型。结构体类型允许程序员将多个分量聚合成一个整体，通过一个变量来表示。结构体的每个分量都有名字，称为字段或成员。结构体成员可以是各种类型，允许程序员针对特定问题创建特定的数据聚合。每个结构体类型在使用前需定义其包含的分量，以便系统处理该新类型。

**结构体的用法**：定义新的结构体类型；定义新类型的变量；访问结构体变量

**结构体类型的定义：** 比如：
```cpp
 struct studentT {
    char no[10];
    char name[10];
    int chinese;
    int math;
    int english;
};
```
注意到，字段名可与程序中的变量名相同;不同结构体中可以有相同的字段名;结构体成员的类型可以是任意类型，包括其他结构体类型。
```cpp
 struct dateT { 
    int month;
    int day;
    int year;
};

struct studentT {
    // ...其他字段...
    dateT birthday;
};
```


## 2、结构体变量

**定义与初始化**：
 ```cpp
 studentT student1;//此时系统分配了一块连续的空间存放每个分量，总的名字就是结构体变量的名字
 
 studentT student1 = {"00001", "工藤新一", 90, 99, 95};
```

**定义结构体类型的同时定义变量：** 第二种方法可以继续用结构体类型名定义更多变量，第一种不能
 ```cpp
 struct {
    字段声明;
} 结构体变量 = {初始化值};
```

 ```cpp
 struct 结构体类型名 {
    字段声明;
} 结构体变量1,  结构体变量2;
 ```

**结构体变量的访问**：访问结构体变量的成员使用点运算符`.`。如访问student1的名字：
```cpp
student1.name
```
若结构体成员中还有结构体变量，则逐级访问，如：
```cpp
student1.birthday.year
```

**结构体变量的赋值**:通常通过对其每个成员的赋值来实现,示例：
 ```cpp
 cin >> student1.no >> student1.name >> student1.chinese >> student1.math >> student1.english >> student1.birthday.year >> student1.birthday.month >> student1.birthday.day;
//注意， cin>>student1 这种方式是错误的！
```
同类型结构体变量间可以相互赋值，赋值本质上是内存字节的顺序复制，即使包含数组也没问，如：
```cpp
student1 = student2;//将student2的成员复制给student1的成员。
```
不能用`==`或`!=`直接比较两个结构体变量，需逐个比较成员。

**结构体变量的输出**:通过输出每个成员实现,示例：
 ```cpp
 cout << student1.no << student1.name << student1.chinese << student1.math << student1.english << student1.birthday.year << student1.birthday.month << student1.birthday. day;
//不能直接使用  cout << student1;  因为iostream库不能识别自定义的结构体类型。
 ```

## 3、指向结构体类型的指针
**定义：**
 ```cpp
 //直接定义指针变量
 studentT *sp;
 
 //在定义结构体类型的同时定义该类型结构体的指针
 sturct studentT{
	 字段声明;
 }*sp;
 ```
 
**指针操作结构体变量**：
给结构体指针赋值
```cpp
sp = &student1;
```
结构体指针的引用
```cpp
 (*sp).name //不常用

 sp->name   //常用，-> 是最高优先级运算符
```

**动态内存分配**: 用法和申请普通的动态变量一样
```cpp
studentT *sp;
sp = new studentT;

dateT *dp;
dp = new dateT {11, 19, 2024};
```

## 4、结构体数组

**定义与初始化：**
```cpp
#定义格式
studentT studentArray[SIZE]; // SIZE为符号常量

#初始化
studentT studentArray[5] = {
    {"00001 ", "工藤新一", 90, 99, 95},
    {...},{...}// 其他初始化值
};

#引用数组元素成员
studentArray[3].name

#数组元素间赋值
studentArray[4] = studentArray[2];
```

**例：**
【统计候选人得票】设有三个候选人，每次输入一个得票的候选人名字，最后输出各人的票结果
```cpp
 struct person {
    int id;
    int count;
} leader[3] = {0, 0, 1, 0, 2, 0};

int main() {
    int i, j, inputID;
    for (i = 1; i <= 100; ++i) {
        cin >> inputID;
        if (inputID < 0 || inputID > 2) {
            cout << "废票";
            continue;
        }
        leader[inputID].count += 1;
    }
    cout << endl;
    for (i  = 0; i < 3; ++i)
        cout << leader[i].id << "\t" << leader[i].count << endl;
    return 0;
}
```

【输出通讯录】
```cpp
#include <iostream>
#include <iomanip>
using namespace std;

struct personT {
    char name[10];
    char sex; // M or F
    char addr[30];
    int phonenum;
};

const int MAX = 100;

int main() {
    personT p[MAX];
    int i, num =  0;
    cout << "姓名 性别 地址 电话(@表示结束)：\n";
    while (num < 100) {
        cin >> p[num].name;
        if (p[num].name[0] == '@') break;
        cin >> p[num].sex >> p[num].addr >> p[num].phonenum;
        ++num;
    }
    
    cout << setiosflags(ios::left);//后续输出数据全部左对齐
    cout << setw(10) << "Name" << "Sex\t" << setw(30) << "Addr" << 
         <<"\tPhoneNum" << endl;
    for (i = 0; i < num; ++i)
        cout << setw(10) << p[i].name << p[i].sex << "\t" << setw(30)<<    
             <<p[i].addr << "\t" << p[i].phonenum << endl;
    return 0;
}
```

**指针与结构体数组：** 与普通的数组一样，结构体指针也能用来指向一个结构体数组。此时，对指针加1就是将其指向地址加了该结构体的大小。

## 4、结构体作为函数参数

**结构体的传递：** 结构体的传递与普通内置类型相同，是值传递。

**指针传递与引用传递：**
和普通的指针传递一样，函数中可以通过指针访问主调函数的记录（结构体），减少函数调用时的数据传递量。当希望把函数内部对结构体的修改返回给主调函数时，可以用指针传递或引用传递。即使不需要将修改返回给主调函数，如果结构体占用的内存量较大，那么值传递会浪费空间和时间，因此往往使用指针传递或引用传递。引用传递的形式比指针传递更简洁，所以用C++编程通常采用引用传递。如果既想提高参数传递效率，又要防止在被调函数中修改实际参数，可以在引用传递的形式参数中加const限定。
示例：
【打印学生信息】
```cpp
 // 指针与引用传递（不安全）
void PrintStudent(studentT *s) {
    cout << s->no << '\t' << s->name << '\t' << s->chinese << '\t' << s->math << '\t' << s->english << endl;
}

void PrintStudent(studentT &s) {
    cout << s.no << '\t' << s.name << '\t' << s.chinese << '\t' << s.math << '\t' << s.english << endl;
}

// C++常规做法（引用传递+const限定）
void PrintStudent(const studentT &s) {
    cout << s.no << '\t' << s.name << '\t' << s.chinese << '\t' << s.math << '\t' << s.english << endl;
}
```

【拼字计分游戏】输入一个单词，对单词中的每一个字母，找到表中对应的项，加上相应的分数
```cpp
#设计内部存储结构
//输入单词的存储：用一个字符数组word[20]
//存储分值表：
 struct PointTable {
    char letters[10];
    int  point;
};

PointTable pt[6] = {
    {"aeilnrstu", 1},
    {"dg", 4},
    {"bcmp", 6},
    {"fhvwy", 2},
    {"k", 5}
};

#函数evalpoint定义
 int evalpoint(char *word, PointTable *pt) {
    int i, j, total = 0;
    for (i = 0; i < strlen(word); ++i)
        for (j = 0; j < 6; ++j)
            if (strchr(pt[j].letters, word[i])) {
                total += pt[j].point;
                break;
            }
    return  total;
}

#主程序流程
 int main() {
    char word[20];
    while (true) {
        cout << "Input word: ";
        cin >> word;
        if (strcmp(word, "quit") == 0) break;
        cout << evalpoint(word, pt) << endl;
    }
    return 0;
}
```

 
 **返回结构体类型的函数**：
 ```cpp
 personT GetPersonData() {
    personT person;
    ...
    return person;
}
//由于返回的是一个结构体的复制，在主调函数中应该有这样的程序段：
int  main() {
    personT p1;
    p1 = GetPersonData(); // 保存返回值
    ...
    return 0;
}
```
  返回的是结构体的复制，类似于值传递，因此函数内变量消失不影响返回值。

**返回结构体的引用**：
 函数返回一个结构体的引用，如：
```cpp
personT &GetPersonData(void) {
    personT *person = new personT;
    // ...
    return *person;
}
```
本质上返回的是结构体的地址。在主调函数中可以有这样的程序段：
```cpp
personT &p1 = GetPersonData();
```
返回结构体的引用时，该结构体不能是该函数的局部变量。

## 5、链表
**链表的概念：**
单链表，双链表（同时存储前趋和后趋），循环链表
**链表的存储：**
```cpp
//每个节点都是一个节点变量
//包含一个同类结构体的指针
struct linkRec
{
    datatype data;
    linkRec *next;
};
```
**链表的操作：**
建立:
```cpp
//定义头指针：linkRec *head;
//建立头结点 ：申请空间；设为头结点（头结点一般不存储数据，仅用于为第一个存数据的结点提供前趋结点）；
//逐个从键盘输入数据，存入链表：接受输入；申请空间；输入数据放入申请到的空间；链入链表尾（把新结点的地址赋值给前趋结点的next指针）；
//置链表结束标志（最后一个结点的next指针）


head = new linkRec; // 头指针
rear = head; // 尾指针，指向最新加入的结点
cin >> in_data;
while(输入未结束) {
    p = new linkRec; // 建立一个新结点
    p->data = in_data; // 在新结点中存入数据
    rear->next = p; // 把新结点连接到链表末尾
    rear = p; // 把链表末尾更新为新结点
    cin >> in_data;
}
rear->next = NULL; // 设置链表结束标志
```

输出：
```cpp
p=head->next;
while(p!=NULL){
	cout<<p->data;
	p=p->next;
}
```

插入：在地址为p的结点后插入一个节点
```cpp
tmp=new linkRec;//为新节点申请空间（动态）
tmp->data=x;
tmp->next=p->next;
p->next=tmp
```

删除：把地址为p的结点的结点删除
```cpp
delPtr=p->next;
p->next=delPtr->next;
```

单链表整体删除：
循环实现
```cpp
void deleteEntireList() {
    Node* current = head;    // 从第一个节点开始
    Node* next = nullptr;    // 用于保存下一个节点
    
    while (current != nullptr) {
        next = current->next;  // 保存下一个节点的地址
        delete current;        // 删除当前节点
        current = next;        // 移动到下一个节点
    }
    
    head = nullptr;           // 重要：将头指针设为空
    cout << "整个链表已删除" << endl;
}
```
递归实现( 当链表很长的时候，可能会导致递归深度过大而造成栈溢出)
```cpp
void deleteEntireListRecursive(Node* node) {
    if (node == nullptr) {
        return;
    }
    deleteEntireListRecursive(node->next);
    delete node;
}

// 调用方式
void deleteEntireList() {
    deleteEntireListRecursive(head);
    head = nullptr;
    cout << "整个链表已删除" << endl;
}
```



**循环链表：**
【约瑟夫环】
```cpp
struct node {
    int data;
    node *next; };

int main() {
    node *head, *p, *q; // head为链表头
    int n, i;

    //输入n
    cout << "\ninput n:"; cin >> n;

    //建立链表
    head = p = new node;
    p->data = 0; //p指向表尾
    for (i=1; i<n; ++i) {
	    q = new node; //q为当前正在创建的节点
	    q->data = i;
	    p->next = q; p = q; //将q链入表尾
    }
    p->next = head; // 头尾相连
    
    
	// 删除过程
	q = head;
	while (q->next != q) { // 只要表非空
	    for (i = 0; i < 2; ++i) // 报数
	    { 
	        p = q; 
	        q = p->next; 
	    }

	    p->next = q->next; // 绕过节点q
	    cout << q->data << '\t'; // 显示被删者的编号
	    delete q; // 回收被删者的空间
	    q = p->next; // 让q指向报1的节点
	}

	// 打印结果
	cout << "\n最后剩下：" << q->data << endl;

	return 0;
}
```

**链表总结**：
和数组相比，其具有以下特点：实现较复杂；插入、删除效率高，但查找第i个元素效率低；无表满的问题；适合于动态表。
