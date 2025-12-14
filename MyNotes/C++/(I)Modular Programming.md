## 1、模块划分
**模块划分：**
当程序变得更长的时候，要在一个单独的源文件中处理如此众多的函数会变得困难。把程序再分成几个小的源文件。每个源文件都包含一组相关的函数。一个源文件被称为一个模块。模块划分标准：块内联系尽可能大，块与块之间联系尽可能小。
**头文件的设计**：
为了方便起见，我们把所有的符号常量定义、类型定义和函数原型声明写在一个头文件里，让每个模块都include这个头文件。
```cpp
// File: main.cpp
#include "header.h"

int main() {
    float a;
    cin >> a;
    cout << yourfun(a);
    cout << endl;
    cout << myfun(a);
    return 0;
}

// File: source.cpp
#include "header.h"

char myfun(float x) {
    int result = yourfun(x);
    return result%10 + '0';
}

int yourfun(float x) {
    return x+0.5;
}

// File: header.h
#include <iostream>
using namespace std;

char myfun(float);
int yourfun(float);
```
 当头文件A包含头文件B，且另一个模块同时包含这两个头文件时，会导致头文件B中的内容在程序中重复出现，引起重复定义编译错误。因此我们在每个头文件中加上头文件保护。
 ```cpp
#ifndef _NAME_H
#define _NAME_H

// 头文件真正需要写的内容：
// 符号常量定义
// 新类型定义（结构体、枚举等）
// 函数原型声明

#endif
 ```

示例：
【石头剪刀布游戏】
```cpp
// 文件：p_r_s.h（头文件的后缀名为.h）
// 本文件定义了两个枚举类型，声明了本程序包括的所有函数原型
#ifndef P_R_S
#define P_R_S
#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;

enum p_r_s {paper, rock, scissor, game, help, quit} ;
enum outcome {win, lose, tie, error} ;

outcome compare(p_r_s player_choice, p_r_s machine_choice);
void prn_game_status();
void prn_help();
void report(outcome result);

p_r_s selection_by_machine();
p_r_s selection_by_player();
#endif





//文件: main.cpp
//石头、剪子、布游戏的主模块

#include “p_r_s.h”//石头剪刀布游戏的头文件

int main) {
	outcome result;
	p_r_s player_choice, machine_choice;
	
	// seed the random number generator
	srand(time(NULL));
	
	while((player_choice = selection_by_player()) != quit)
    switch(player_choice) {
    case paper: case rock: case scissor:
        machine_choice = selection_by_machine();
        result = compare(player_choice, machine_choice);
        report(result); 
        break;
    case game: 
        prn_game_status(); 
        break;
    case help: 
        prn_help(); 
        break;
    default:
        cout<<"PROGRAMMER ERROR!\n\n";
        exit(1);//和return的区别是其直接结束程序而不是一个函数
    } 

prn_game_status();
return 0;


//文件：select.cpp
//包括机器选择selection_by_machine和玩家选择selection_by_player函数的实现

#include "p_r_s.h"

p_r_s selection_by_machine() {
    int select = (rand() * 3 / (RAND_MAX + 1));
    
    cout << " I am ";
    switch(select){
        case 0: cout << "paper. "; break;
        case 1: cout << "rock. "; break;
        case 2: cout << "scissor. "; break;
    }
    
    return ((p_r_s) select);
}

p_r_s selection_by_player() {
    char c;
    p_r_s player_choice;

    prn_help(); //显示输入提示
    cout << "please select: "; cin >> c;
    switch(c) {
    case 'p': player_choice = paper;  cout << "you are paper. "; break;
    case 'r': player_choice = rock;  cout << "you are rock. "; break;
    case 's': player_choice = scissor;  cout << "you are scissor. "; break;
    case 'g': player_choice = game;  break;
    case 'q': player_choice = quit;  break;
    default : player_choice = help;  break;
    }
    return player_choice;
}



//文件: compare.cpp
//包括compare函数的实现

#include "p_r_s.h"

outcome compare(p_r_s player_choice, p_r_s machine_choice) {
    outcome result;

    if (player_choice == machine_choice) return tie;
    switch(player_choice) {
    case paper: result = (machine_choice == rock) ? win : lose; break;
    case rock: result = (machine_choice == scissor) ? win : lose; break;
    case scissor: result = (machine_choice == paper) ? win : lose; break;
    default: cout << " PROGRAMMER ERROR: Unexpected choice!\n\n";
             exit(1);
    }
    return result;
}



//文件：print.cpp
//包括所有与输出有关的模块。
//有prn_game_status，prn_help和report函数

#include "p_r_s.h"

int win_cnt = 0, lose_cnt = 0, tie_cnt = 0; //模块的内部状态

// 模块的内部状态：
// 该模块中的所有函数都可以使用。
// 其它模块中的所有函数都不能使用。
// 所以通常用全局变量来定义模块的内部状态。

void report(outcome result) {
    switch(result) {
    case win: ++win_cnt;
        cout << "You win. \n"; break;
    case lose: ++lose_cnt;
        cout << "You lose.\n"; break;
    case tie: ++tie_cnt;
        cout <<"A tie.\n"; break;
    default: cout << "PROGRAMMER ERROR!\n\n";
        exit(1);
    }
}

void prn_game_status() {
    cout << endl;
    cout << "GAME STATUS:" << endl;
    cout << "Win: " << win_cnt << endl;
    cout << "Lose: " << lose_cnt << endl;
    cout << "Tie: " << tie_cnt << endl;
    cout << "Total: " << win_cnt + lose_cnt + tie_cnt << endl;
}

void prn_help() {
    cout << endl
    << "The following characters can be used:\n"
    << " p for paper\n"
    << " r for rock\n"
    << " s for scissors\n"
    << " g print the game status\n"
    << " h help, print this list\n"
    << " q quit the game\n";
}
```

## 2、库的设计与实现
**库的设计与实现：**
库的接口是一个头文件(.h)。库的用户必须了解的内容，包括库中函数的原型声明、符号常量、自定义类型（枚举、结构体、“类”等）的定义。
库的实现表现为一个源文件(.cpp)。库的用户不需要了解的内容，主要是函数的定义。库的这种实现方法称为信息隐藏，如有需要可以对源文件加密。

**随机函数库的设计：**
头文件的格式与石头、剪刀、布游戏中的一样。头文件中，每个函数声明前应该有一段注释，告诉用户怎么使用这些函数。
```cpp
//文件：Random.h
//随机函数库的头文件

#ifndef __random_h
#define __random_h

//函数：RandomInit
//用法：RandomInit()
//作用：此函数初始化随机数种子
void RandomInit();

//函数：RandomInteger
//用法：n = RandomInteger(low, high)
//作用：此函数返回一个low到high之间的随机数，包括low和high
int RandomInteger(int low, int high);

#endif


//文件：Random.cpp
//该文件实现了Random库

#include <cstdlib>
#include <ctime>

#include "Random.h"

//函数：RandomInit
//该函数取当前系统时间作为随机数发生器的种子

void RandomInit() {
    srand(time(NULL));
}

// 函数：RandomInteger
// 该函数将0到RAND_MAX的区间的划分成high - low + 1 个
// 子区间。当产生的随机数落在第一个子区间时，则映射成low。
// 当落在最后一个子区间时，映射成high。当落在第i个子区间时
// (i从0到high-low)，则映射到low + i

int RandomInteger(int low, int high) {
    return (low + (high - low + 1) * rand() / (RAND_MAX + 1));
}
```
