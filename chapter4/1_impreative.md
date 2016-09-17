## 命令式编程

命令式范式侧重于描述程序的执行过程，它由一系列的命令组成，通过语句（statements）改变程序中的状态（status）。

```java
if(x > 0) y = x;
else y = -x;
```

#### 语句（statements）

语句分为基本语句和复合语句两种，其中基本语句包括：断言（assertion）、赋值（assignment）、跳转（goto）、返回（return）、调用（call）。复合语句又包括区块（block）、循环（do-loop, for-loop, while-loop）条件（if, if-else）、分支（switch）、伴随（with）、异常（try-catch）。

**基本语句**

基本语句通常用来声明、赋值、返回变量，执行函数。

**赋值**

赋值语句用于对变量进行赋值操作。

```java
int a = 2;
```

**返回**

返回语句用于返回计算结果。

```java
public static int add(int a, int b){ return a + b;}
```
```javascript
function abs(a){ return a > 0 ? a : -a;}
```

**断言**

断言一般用来判断表达式结果的真假。

```java
//Java 断言

assert(1 > 0);System.out.println("1 large than 0"); 
//1 large than 0

assert(1 < 0);System.out.println("1 small than 0"); 
//Exception in thread "main" java.lang.AssertionError
```

也有些测试库用断言来比较期望值与实际值是否相等。

```java
//Java junit断言

assertEquals(2, 1+1);
//true
```

```javascript
//JavaScript jasmine断言

expect(1+1).toBe(2); 
//true
```

**调用**

调用一般用于执行定义的函数。

```javasript
add(1, 3);
abs(-4)
```

**跳转**

跳转可以使程序直接到指定标记处继续执行，正因为如此goto语句使程序难以维护和理解，所以大多数语言在设计层面已经舍弃了goto，即使在保留了goto语句的高级语言（C、C++）中也不建议使用。

```c
//C goto语句

int n = 0;
loop: if(n < 3){ n++; goto loop;}
```

**复合语句**

命令式编程中最具代表性的语句就是条件、循环和分支。这些语句负责程序的逻辑控制。

**条件**


if：如果表达式成立，则执行语句。

```
if([表达式]) [语句]
```

```java
if(x == 1) 			System.out.println("one")；
//one
```

if-else if：依次判断表达式，如果表达式n成立则执行语句n，结束判断。

```java
if([表达式1])         [语句1]
else if([表达式2])    [语句2]
else if([表达式3])    [语句3]
```

```java
int x = 2;
if(x == 1)       System.out.println("one");
else if(x == 2)	 System.out.println("x is two");
else if(x == 2)	 System.out.println("x is 2");
//x is two
```

if-else：如果表达式成立，则执行语句1，否则执行语句2。

```java
if([表达式]) 			[语句1]
else   				 [语句2]
```

```
int x = 1;
if(x == 1) 	System.out.println("x is 1");			
else 				System.out.println("x is not 1");
//x is 1
```

**循环**

for：执行初始化表达式，然后执行条件判断表达式，如果成立则执行语句和增量表达式并重复之前的执行过程，如果不成立则结束循环。

```java
for([初始化表达式]; [条件判断表达式]; [增量表达式])	[语句]
```

```java
//Java for循环

for(int i=0; i<=2; i++)
	System.out.print(i+1);
  //123
```

while：执行条件判断表达式，如果满足则执行语句。

```
while([条件判断表达式]) 		[语句]
```

```java
int i = 1;
while(i<=3){
	System.out.print(i);
	i+=1;
}
//123
```

do-while：首先执行语句，然后执行条件判断表达式，如果成立则重复上述操作，否则结束循环。

```java
int i = 1;
do{
	System.out.println(i);
	i++;
}while(i<=3);
//123
```

**分支**

分支语句和条件分支功能类似，部分语言对switch语句有优化，所以多路分支时switch效率相对要高，不过switch语句不易维护，所以出于这方面考虑建议用if-else if语句替换，大多数情况下switch和if-else if可以相互替换，但是对于不可列举的情况下只能用if-else if。

switch：依次判断变量表达式和常量判断表达式n是否相等，如果相等则执行语句n，否则执行默认分支语句。

```java
switch([变量表达式]){
	case [常量判断表达式1]
		[语句1]
		break;
  case [常量判断表达式2]
    [语句2]
    break;
  case [常量判断表达式3]
    [语句3]
    break;
	default
		[默认分支语句]
}
```

```java
int a = 1;
switch(a){
	case 1:
		System.out.println(1);
		break;
	case 2:
		System.out.println(2);
		break;
	case 3:
		System.out.println(3);
		break;
	default:
		System.out.println("other number");
		break;
}
//1
```

**异常**

异常处理的作用是在程序正常运行中对不合法的输入进行控制和提示，防止程序崩溃。

try-catch-finally：执行异常语句如果发现异常则执行异常处理语句，最后无论是否存在异常都会执行终止语句。

```
try { [异常语句]; }
catch{ [异常处理语句]; }
finally{ [终止语句]; }
```

```java
try{
	int a = 1/0;
}catch(Exception e){
	System.out.println(e.toString());
	//java.lang.ArithmeticException: / by zero
}finally{
	System.out.println("finally");
	//finally
}
```

#### 命令式语言发展历程

**机器语言**

机器语言是由计算机直接执行的语言，它们一般由八进制或者十六进制的字节码组成，这种代码只能在指定的机器上运行，因此很难移植。

```
4944 3303 0000 001f 1c09 5453 5345 0000
000d 0000 004c 6176 6635 362e 342e 3130
```

1960年汇编语言被开发出来，汇编语言提供了一些标签和符号，通过汇编语言可以编写出可读性更高的源代码，然后将源代码编译为机器可执行的字节码。

汇编语言的出现，使程序的编写得到了极大的便利。

```assembly
;汇编语言
INC COUNT        ;计数器COUNT加1 
MOV TOTAL, 48    ;变量TOTAL赋值为48
ADD AH, BH       ;将寄存器AH和BH相加
```

可以看出除了复杂的语言特性外，从汇编语言开始就有了类似变量赋值、运算的概念。

**过程式语言**

过程式语言按照一定的流程顺序执行，在执行过程中可以调用其他程序或函数。其主要用到的语句有流程控制语句(条件、分支、循环)和函数调用。

这里我们用C语言来看下过程式语言是如何解决八皇后问题的。

> 八皇后问题最早是由国际象棋棋手马克斯·贝瑟尔（Max Bezzel）于1848年提出，问题在于如何在一个8×8的国际象棋棋盘上放置八个皇后，使得任何一个皇后都无法直接吃掉其他的皇后（任两个皇后都不能处于同一条横行、纵行或斜线）。

![八皇后](/images/eight_queens.png)

如上图我们在棋盘第四列第五行位置放置了一个皇后，按照八皇后的游戏规则，图中所有灰色部分都是不允许放置皇后的，为了方便说明我们把这些区域称作不可置位置；相反所有白色区域是允许放置皇后的，我们称作可置位置。然后我们用坐标（4，3）来表示皇后的位置，并规定从左到右依次为首列、第一列、第二列、第三列...从上到下依次为首行、第一行、第二行...

有了如上约定后，我们就可以介绍如何通过回溯法获得八皇后的结果集了。

1. 从首列按首行到第七行的顺序依次放置一个皇后，每当放置一个皇后执行下一步。
2. 在第一列中选择第一个可置区域放置一个皇后（行坐标最小的那个），执行下一步。
3. 跳转到下一列，查找最小行标的可置区域放置皇后，如果不存在可置区域，则回溯到上一行，重置上一行的皇后到下一个最小行标的可置区域。
4. 重复第三步，直到到达第七列在可置区域放置皇后，此时为一个有效解。
5. 将有效的皇后位置记录，输出。

我们配合下面这张图来说明下回溯的过程：

![](/images/eight_queens_steps.png)

1. 当我们在(0, 0)位置放置一个皇后后，第一列最小行标的可置区域为(1, 2)，在此放置皇后。
2. 重复上述流程中的第四步操作，当第五列时发现无可置区域，回溯到第四列。
3. 回溯到第四列选择下一个最小行标的可置区域(4, 7)后，放置皇后，继续下一列。

接下来我们看看过程式的C代码是如何描述上述执行过程的：
```c
//C 八皇后问题

#include <stdio.h>

int is_safe(int rows[8], int x, int y)  
{
  int i;
  if (y == 0)    return 1;
  for (i=0; i < y; ++i) {
    if (rows[i] == x || rows[i] == x + y - i || rows[i] == x - y +i)
      return 0;
  }
  return 1;
}

void putboard(int rows[8])  
{
  static int s = 0;
  int x, y;
  printf("\nresult %d:\n", ++s);
  for (y=0; y < 8; ++y) {
    for (x=0; x < 8; ++x)
      printf(x == rows[y] ? "| o " : "|   ");
    printf("|\n");
  }
}

void eight_queens(int rows[8], int y)
{
  int x;
  for (x=0; x < 8; ++x) {
    if(is_safe(rows, x, y)) {
      rows[y] = x;
      if(y == 7)    putboard(rows);
      else          eight_queens(rows, y+1);
    }
  }
}

int main()
{
  int rows[8];
  eight_queens(rows, 0);
  return 0;
}
```
这里简单说明下这段代码。

1. 先看主函数main，这里声明了一个数组rows，用来描述从首行到第七行。
2. main函数中调用了eight_queens函数，传入了rows和0，rows上面已经说过了，用来描述行。0表示从第一列开始插入皇后到可置区域。
3. 在eight_queens函数中x表示当前列下行坐标，is_safe函数返回当前坐标(x, y)是否为可置区域。如果是可置区域则放置皇后并记录皇后坐标`rows[y] = x`，这里为了节约内存，用了一维数组来记录，数组的索引用来记录y坐标，值用来记录横坐标，则皇后我坐标实际为（rows[y], y)。
4. 然后我们来看看is_safe函数，这个函数会检查当前这个位置（坐标）是否会与之前列下的皇后同行、同对角、反对角线（即判断这个位置的横坐标rows[i]是否会与之前任何一个皇后的横坐标x，对角线横坐标x + y - i，斜对角线横坐标x - y +i相等）。如果有相等则说明该坐标为不可置区域，返回0，否则说明是可置区域返回1。
5. 最后回到eight_queens，当列坐标y=7且is_safe返回1时，说明回溯算法走到最后一列并且找到了一个可置区域放置了皇后，那么这个路径就是个有效解，执行putboard输出所有皇后的坐标。我们通过rows[y] = x来判断的这个位置是不是皇后，是的话输出o来模拟棋子。

来看一看输出的结果：
```
result 1:
| o |   |   |   |   |   |   |   |
|   |   |   |   | o |   |   |   |
|   |   |   |   |   |   |   | o |
|   |   |   |   |   | o |   |   |
|   |   | o |   |   |   |   |   |
|   |   |   |   |   |   | o |   |
|   | o |   |   |   |   |   |   |
|   |   |   | o |   |   |   |   |

result 2:
| o |   |   |   |   |   |   |   |
|   |   |   |   |   | o |   |   |
|   |   |   |   |   |   |   | o |
|   |   | o |   |   |   |   |   |
|   |   |   |   |   |   | o |   |
|   |   |   | o |   |   |   |   |
|   | o |   |   |   |   |   |   |
|   |   |   |   | o |   |   |   |
...
result 92:
|   |   |   |   |   |   |   | o |
|   |   |   | o |   |   |   |   |
| o |   |   |   |   |   |   |   |
|   |   | o |   |   |   |   |   |
|   |   |   |   |   | o |   |   |
|   | o |   |   |   |   |   |   |
|   |   |   |   |   |   | o |   |
|   |   |   |   | o |   |   |   |
```
八皇后问题有92个解，这里就不一一列举了。

**面向对象语言**

在1980年左右，在命令式范式的基础上追加了对象特性的语言开始快速的发展起来，这就是面向对象语言。

1. 1980年Smalltalk
2. 1985年C++
3. 1987年Perl
4. 1990年Python
5. 1991年Visual C++
6. 1994年PHP和Java
7. 1995年Ruby
8. 2002年.NET Framework(C#、VB.NET)

面向对象语言将数据和方法打包到一个结构中，通常称作为对象。对象内的数据和方法只有对象本身可以访问。程序不再由流程式的函数调用组成，而是通过对象及对象间的关系构成。

这里用C++来重复上面的八皇后问题，对比下面向对象和过程式两种范式的区别。

```c-plus
//C++ 八皇后问题

#include <iostream>
using namespace std;

class EightQueens{
  int rows[8];
  public:
    bool IsSafe(int x, int y){
      int i;
      if (y == 0)    return 1;
      for (i=0; i < y; ++i) {
        if (rows[i] == x || rows[i] == x + y - i || rows[i] == x - y +i)
          return false;
      }
      return true;
    }

    void PutBoard(){
      static int s = 0;
      int x, y;
      cout << "\nresult" << ++s << ":\n";
      for (y=0; y < 8; ++y) {
        for (x=0; x < 8; ++x)
          x == rows[y] ? cout << "| o " : cout << "|   ";
        cout << "|\n";
      }
    }

    void Solution(int y){
      int x;
      for (x=0; x < 8; ++x) {
        if (IsSafe(x, y)) {
          rows[y] = x;
          if(y == 7)    PutBoard();
          else          Solution(y+1);
        }
      }
    }
};

int main()
{
  EightQueens q;
  q.Solution(0);
}
```

这里整体逻辑和C语言实现是相似的，唯一不同的在于代码的组织方式，像C++这种面向对象语言，我们可以把数据与函数打包到对象中管理，在这里我们将八皇后问题所需要用到的方法和数据都打包到EightQueens对象中，对比C语言你会发现所有函数都不需要显示传递参数rows了。rows作为EightQueens对象的属性允许对象内的任何函数直接访问。最后声明了EightQueens的一个实例q，然后通过对象q调用Solution函数，传入起始行参数0来查询八皇后问题的有效解。

对比过程式语言，面向对象语言更侧重于对数据与函数的封装，以及封装后对象与对象之间关系的处理。后面我会用一个章节来详细说明面向对象语言的特点，这里就不过多解释了。
