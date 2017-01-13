# 第四章 从算法聊编程语言的划分

和第一章相对，程序由数据和算法组成，第一章从数据类型方面对编程语言进行了对比，这一章将从算法方面对编程语言进行对比。当然所说的算法并非计算机算法课程里所说的数学相关的逻辑知识，就像程序通过计算机的数据类型来表达数据的概念，也通过范式表达算法的概念，这里所说的算法就是编程范式。

1. 命令式编程范式相关概念及子范式。
2. 声明式编程范式相关概念及子范式。

### 命令式编程

命令式范式侧重于描述程序的执行过程，它由一系列的命令组成，通过语句（statements）改变程序中的状态（status）。

```java
if(x > 0)  y =  x;
else       y = -x;
```

#### 语句（statements）

语句分为基本语句和复合语句两种，其中基本语句包括：断言（assertion）、赋值（assignment）、跳转（goto）、返回（return）、调用（call）。复合语句又包括区块（block）、循环（do-loop, for-loop, while-loop）条件（if, if-else）、分支（switch）、伴随（with）、异常（try-catch）。

**基本语句**

基本语句通常用来声明、赋值、返回变量，执行函数。

**赋值**

赋值语句用于对变量进行赋值操作。

```java
//Java 变量赋值
int a = 2;
```

**返回**

返回语句用于返回计算结果。

```java
//Java 函数返回值
public static int add(int a, int b){ return a + b;}
```
```javascript
//JavaScript 函数返回值
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
assertEquals(2, 1+1);    //true
```

```javascript
//JavaScript jasmine断言
expect(1+1).toBe(2);    //true
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
//Java 条件判断
if(x == 1)  System.out.println("one");    //one
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
else if(x == 2)  System.out.println("x is two");
else if(x == 2)  System.out.println("x is 2");
//x is two
```

if-else：如果表达式成立，则执行语句1，否则执行语句2。

```java
if([表达式])       [语句1]
else           [语句2]
```

```java
int x = 1;
if(x == 1)  System.out.println("x is 1");     
else        System.out.println("x is not 1");
//x is 1
```

**循环**

for：执行初始化表达式，然后执行条件判断表达式，如果成立则执行语句和增量表达式并重复之前的执行过程，如果不成立则结束循环。

```java
for([初始化表达式]; [条件判断表达式]; [增量表达式]) [语句]
```

```java
//Java for循环

for(int i=0; i<=2; i++)
  System.out.print(i+1);    //123
```

while：执行条件判断表达式，如果满足则执行语句。

```
while([条件判断表达式])    [语句]
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

#### 命令式语言划分

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

<img src="/images/eight_queens.png" alt="八皇后" style="
    float: right;
    margin: -25px -40px -25px -50px;
">

<p style="margin-top: 25px;">
如上图我们在棋盘第四列第五行位置放置了一个皇后，按照八皇后的游戏规则，图中所有灰色部分都是不允许放置皇后的，为了方便说明我们把这些区域称作不可置位置；相反所有白色区域是允许放置皇后的，我们称作可置位置。然后我们用坐标（4，3）来表示皇后的位置，并规定从左到右依次为首列、第一列、第二列、第三列...从上到下依次为首行、第一行、第二行...
</p>

<p style="clear: both">有了如上约定后，我们就可以介绍如何通过回溯法获得八皇后的结果集了。</p>

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
result 1:                                result 2:
| o |   |   |   |   |   |   |   |        | o |   |   |   |   |   |   |   |
|   |   |   |   | o |   |   |   |        |   |   |   |   |   | o |   |   |
|   |   |   |   |   |   |   | o |        |   |   |   |   |   |   |   | o |
|   |   |   |   |   | o |   |   |        |   |   | o |   |   |   |   |   |
|   |   | o |   |   |   |   |   |        |   |   |   |   |   |   | o |   |
|   |   |   |   |   |   | o |   |        |   |   |   | o |   |   |   |   |
|   | o |   |   |   |   |   |   |        |   | o |   |   |   |   |   |   |
|   |   |   | o |   |   |   |   |        |   |   |   |   | o |   |   |   |
...
result 91:                               result 92:
|   |   |   |   |   |   |   | o |        |   |   |   |   |   |   |   | o |
|   |   | o |   |   |   |   |   |        |   |   |   | o |   |   |   |   |
| o |   |   |   |   |   |   |   |        | o |   |   |   |   |   |   |   |
|   |   |   |   |   | o |   |   |        |   |   | o |   |   |   |   |   |
|   | o |   |   |   |   |   |   |        |   |   |   |   |   | o |   |   |
|   |   |   |   | o |   |   |   |        |   | o |   |   |   |   |   |   |
|   |   |   |   |   |   | o |   |        |   |   |   |   |   |   | o |   |
|   |   |   | o |   |   |   |   |        |   |   |   |   | o |   |   |   |
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

```cpp
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

### 声明式编程

声明式范式的编程风格是只描述问题的逻辑关系，而不关系具体解决过程。通常声明式程序由一系列的逻辑条件组成，程序执行时会找出满足这些逻辑的情况作为结果返回。简单的说声明式编程解决问题的思路是通过陈述（声明）语句描述问题是什么而不是通过流程控制语句描述问题该如何解决。这样做的好处能尽可能的减少副作用，甚至无副作用，因此声明式编程在处理并行问题时更有优势。

满足声明式编程风格的范式或者说子范式（我个人觉得范式之间没有明确的继承关系，而是互相穿插的）包括如下：

1. 约束编程
2. 领域专属语言
3. 逻辑编程
4. 函数式编程

在介绍声明式编程的子范式之前，我想先介绍一些声明式及其子范式下的编程技巧（或者说是编程风格）。

#### 声明式编程技巧

**列表推导式**

声明式编程中常用的数据结构为列表和元组，通常一个列表作为输入，输出一个变化后的新列表。列表推导式也是这样的技巧，将一个规则约束于原列表，推导出一个满足这个规则的新列表。

```haskell
--Haskell 列表推导
[ x * 2 | x <- [1,2,3,4], x > 2]    --[6,8]
```

列表推导式语法和数学中集合推导式是一致的，`{x * 2 | x ∊ N, 2 < x < 5 }` 表示取2到5之间的正整数乘2。同理，上述代码表示取集合中满足x > 2的元素做x * 2运算，然后将结果集打包成一个新列表（[6, 8]）返回。

之前强调过声明式语言的编程特点是不描述具体执行过程，只描述具体规则，由机器推导运算结果。那么应用到列表推导式是如何表现的呢？

我们来看这个例子，求解三边长度都在20以内的直角三角形各边长分别为多少。

```haskell
--Haskell 通过列表推导式过计算出符合条件的三角形

[(a,b,c) | c <- [1..20], b <- [1..c], a <- [1..b], c^2 == b^2 + a^2]
--[(3,4,5),(6,8,10),(5,12,13),(9,12,15),(8,15,17),(12,16,20)]
```

上面代码为了排除重复的三角形，限制了三边长度关系`a<b<c`。同样如果我们想在上述条件不变的情况下，取出三边长度总和在10到25之间的三角形，只需要追加约束条件`a+b+c > 10, a+b+c < 25`。

```haskell
--Haskell 通过列表推导式过计算出符合条件的三角形

[(a,b,c) | c <- [1..20], b <- [1..c], a <- [1..b], c^2 == b^2 + a^2, a+b+c > 10, a+b+c < 25]
--[(3,4,5),(6,8,10)]
```

**模式匹配**

命令式编程中用来控制分支逻辑的语法有case和if两种，因为case难以阅读，所以多数情况下使用if-else分支结构，对于多种情况就会出现大量的if-else分支，形成一个树一样的结构。

声明式编程中通过模式匹配解决了分支难以阅读的问题，模式匹配通过检查数据结构与条件匹配来决定是否执行相应操作。下面通过一个简单的例子来看看模式匹配是如何工作的。


```haskell
--Haskell 对输入的阿拉伯数字进行匹配返回对应的英文

numberToEnglish:: Int->String
numberToEnglish 1 = "one"
numberToEnglish 2 = "two"
numberToEnglish 3 = "three"
numberToEnglish 4 = "four"
numberToEnglish 5 = "five"
numberToEnglish 6 = "six"
numberToEnglish 7 = "seven"
numberToEnglish 8 = "eight"
numberToEnglish 9 = "nine"
numberToEnglish 0 = "zero"
numberToEnglish _ = "not between 0 to 9"

numberToEnglish 4     --four

numberToEnglish 12    --not between 0 to 9
```

这里我们定义了函数numberToEnglish并规定了输入类型Int及返回类型String，然后按顺序定义了十个模式，分别对参数为0-10的情况定义了处理逻辑，最后用`_`表示任意参数，相当于default或else。

我们以参数param = 4为例来说明执行过程：

1. 首先会匹配第一个模式param = 1，不符合条件，则匹配下一个模式param = 2，
2. 重复1，当匹配到param = 4时，匹配成功，则返回"four"。

同理param = 12对前面10个模式都不匹配，所以执行param = _ 返回"not between 0 to 9"。

**哨兵（Guard）**

和模式匹配一样，哨兵也是声明式语言中的分支逻辑，如果一定要对比的话，模式匹配有点像case而哨兵更像if-else，每一个哨兵都是一个布尔表达式，如果表达式值为true，就会执行对应的操作。值为false则对下一个哨兵求值。

我们通过BMI（体重指数）计算来看下哨兵是如何工作的。

>BMI指数（Body Mass Index），即身体质量指数，是用体重公斤数除以身高米数平方得出的数字，是目前国际上常用的衡量人体胖瘦程度以及是否健康的一个标准。

```haskell
--Haskell 计算体质数

getBMI:: Double->String
getBMI bmi
 | bmi <= 18.5 = "underweight"
 | bmi <= 25.0 = "normal"
 | bmi <= 30.0 = "overweight"
 | otherwise = "obese"

getBMI 27    --overweight
```

我们以bmi = 27为例来说明：

1. 首先计算第一个哨兵27 <= 18.5计算结果为false，则计算下一个哨兵。
2. 当计算第三个哨兵27 <= 30.0时，计算结果为true，则执行对应操作返回字符串"overweight"。
3. 最后的ohterwise和模式匹配里的`_`作用类似相当于命令式语言中的else。

**合一、绑定**

合一和绑定都是赋值操作，我们先看看`prolog`的合一。

```prolog
%Prolog 合一

combine(X, Y, Z):- X = 1, Y = 2, Z = 3.

combine(1,2,3).    %yes
```

combine对元组(X, Y, Z)执行了合一操作，这时元组中满足X = 1, Y = 2, Z = 3，因此执行`combine(1,2,3).`返回yes。

好，我们了解了合一的语法后，接下来看看如何用haskell的合一和绑定来重写上面的getBMI函数。

```haskell
--Haskell 通过绑定重构getBMI

getBMI:: Double->Double->String
getBMI weight height
  | bmi <= normal     = "underweight"
  | bmi <= overweight = "normal"
  | bmi <= obese      = "overweight"
  | otherwise         = "boese"
 where bmi = weight/height^2
       (normal, overweight, obese) = (18.5, 25.0, 30.0)

getBMI 70 1.72    --normal
```

在讲解这段代码之前我们先说下where的作用：where相当于指令式语言里的赋值操作，它可以记录一个操作结果，方便多次引用。其作用域限定在当前函数定义内。

接下来我们来看看getBMI函数，与之前不同，它传入了两个参数：weight和height，用来计算BMI。然后用where将weight/height^2的计算结果绑定到bmi，以方便多次使用。同时用合一对元组(normal, overweight, obese)进行赋值，用来作为BMI参数的临界值。

#### 满足声明式风格的编程范式

接下来我会简单介绍下约束编程和领域专属语言，然后相对详细的说明下逻辑编程，至于函数式编程，我会拿出一章来详细说明，和面向对象一样也不详细解释了。

**约束编程**

约束编程规定了变量之间的一种约束关系，它不强调具体要执行哪一步计算，只是规定了变量的一些属性。简单的说，就是数学中方程式的概念，约束只定义了方程式的定义域，并没有指明如何求解具体解，但解必须是满足这个域的。

比如以`y = x + 1, x ∊ {1,2}`为例，依照这个方程式的约束我们可以得知y的值域是{2,3}。

约束式编程一般作为其他范式的一种补充，我们来看看在基于逻辑范式的prolog和基于函数范式的haskell下是如何表现的：

```prolog
%Prolog 方程式求解

equation(X, Y) :- X == 1, Y is X + 1.
equation(X, Y) :- X == 2, Y is X + 1.
```

执行：

```prolog
equation(1, Y).    %Y = 2
equation(3, Y).    %no
```

这里定义了一个推断equation，当断言x == 1成立时，则执行is运算将Y绑定为X+1，Prolog通过断言和推断返回查询结果Y=2。如果断言不成立，程序无法查询到Y值，则返回no。

```haskell
--Haskell 方程式求解

equation x
 | x == 1 = x + 1
 | x == 2 = x + 1
 | otherwise = error "no"
```

执行：

```haskell
equation 1    --2
equation 3    --Exception: no
```

这里定义了一个函数equation，对传入参数x依次匹配各个模式，如果满足当前模式则返回操作结果，否则执行下一个模式。当传入参数为1时，满足第一个模式返回x+1=2，当传入参数为3时，前两个模式都不匹配，因此执行otherwise抛出一个异常。

**领域专属语言**

领域专属语言（DSLs：Domain-specific languages）是针对某一特定问题而设计的语言，常见的领域专属语言如正则表达式、结构化查询语言（linq）、标记语言（html）。

应用场景：

1. 作为命令行工具或者编译程序的标准用户输入接口，比如grep的正则表达式匹配。
2. 通过编程语言的宏机制实现内部DSL拓展语言的表述能力，比如Lisp的mcro，Ruby的DSL。
3. 作为某语言的内置库，增强其表达能力，比如微软的Linq。
4. 用通用编程语言解决一个特定的问题，嵌入到宿主程序中，比如用perl实现一个正则引擎。

**逻辑编程**

逻辑编程由三个重要部分组成，分别是：断言、推断和查询，断言用来描述客观事实；推断是针对断言作出的推测；查询就是要解决的一个问题。一般逻辑式语言通过断言和推断描述出问题是什么，然后通过查询语句将问题交给程序运算，程序会根据断言和推断计算出符合查询的结果。

谈到逻辑式编程，最具代表性的语言就是prolog，因此接下来的例子都用prolog来说明。

我们用断言和推断来实现一个等量代换的例子，在数学中等量代换是指如果a = b, b = c，那么a = c。这里断言是a = b和b = c，推断是a = c。那么用prolog来实现断言和推断则如下：

```prolog
equal(a,b).
equal(c,b).

equalToo(X,Y) :- equal(X,Z), equal(Y,Z).
```

其中前两行是断言描述了a=b，c=b，最后一行推断表示：a = c的前提条件是：a = b且 c = b。

我们运行这段程序，然后执行如下两个查询：

```prolog
equalToo(a,c).    %yes
```

接下来看看prolog是如何解决汉诺塔问题的

> 汉诺塔是法国数学家爱德华·卢卡斯提出的一个数学问题，传说印度某间寺院有三根柱子，柱子上串有64个金盘。寺院里的僧侣依照一个古老的预言，以一定的规则来移动这些盘子；预言说当这些盘子移动完毕，世界就会灭亡。

汉诺塔的游戏规则如下：
1. 有三根杆子A，B，C。A杆上有N个（N>1）穿孔圆盘，盘的尺寸由下到上依次变小。
2. 要求按3、4的规则将所有圆盘移至到C杆。
3. 每次只能移动一个圆盘。
4. 大盘不能叠在小盘上面。

在说明具体实现代码之前，我们先理清汉诺塔的解决思路。

![](/images/hanoi.png)

上图可以看出我们将A中圆盘移动到C的过程中有一个关键的状态点：一个柱子上按顺序叠放着n-1个圆盘，而另一个柱子上放着当前最大的圆盘（抛开C不管，只看柱子A、B）。

在这个状态点我们把最大的圆盘移至到C，就恢复到了最初的状态：一个柱子上按顺序叠放着n个圆盘，而另一个柱子上没有圆盘。实际上要把圆盘按顺序叠放到C柱子，我们只需要不断的重复操作达到这两个状态点就可以了。

因为声明式语言不关心具体的操作步骤，所以我们根本不需要考虑通过怎样的移动可以达到这两个状态，我们只需要强调这两种状态的产生时机就可以了。

```prolog
%Prolog 汉诺塔问题

move(1,A,B,C) :-
 write('move top disk from '),
 write(A),
 write(' to '),
 write(C),
 nl.
move(N,A,B,C) :-
 N>1,
 M is N-1,
 move(M,A,C,B),
 move(1,A,B,C),
 move(M,B,A,C).
```

我们定义了一个移动函数，第一个参数表示当前存在的圆盘数，其余A、B、C参数分别表示图示中的三根圆柱，A，作为起始圆柱，B作为辅助圆柱，C作为终止圆柱。

1. 当只存在一个圆盘时，我们可以直接把圆盘从起始圆柱A移动到终止圆柱C。
2. 当存在一个以上（N个）圆盘时，我们先把上面N-1个圆盘从起始圆柱A移动到辅助圆柱B，然后再将起始柱A最下面的圆盘移动到终止圆柱C。这时B就变成了之前的起始圆柱，而A则变成了辅助圆柱，继续重复之前的逻辑将剩余的N-1个圆盘从起始圆柱B移动到终止圆柱C，而A则作为辅助圆柱。

最后我们定义起始圆柱A、辅助圆柱B、终止圆柱C、圆盘数4，然后输出。

```prolog
move(3, 'A', 'B', 'C').

/* 输出 
move top disk from A to C
move top disk from A to B
move top disk from C to B
move top disk from A to C
move top disk from B to A
move top disk from B to C
move top disk from A to C
*/
```

可以看到在上述代码中我们并没有关心具体细节，只需要串联关键状态，计算机会自动帮我们计算具体步骤，这就是逻辑式语言的优势。

最后我们再来看一个更加纯粹的只关心规则声明的例子：四色定理。

> 如果在平面上划出一些邻接的有限区域，那么可以用四种颜色来给这些区域染色，使得每两个邻接区域染的颜色都不一样。

<img src="/images/coloring.png" style="width: 240px; height: 210px; float:right; margin-left: 20px; margin-top: -20px">


<p>我们用四种颜色（红、绿、蓝、黄）对左侧图中9个区域进行染色，根据四色定理我们可以使任意相邻的两个区域都不同色。
</p>

<p>首先列出相邻两个区域不同颜色着色的所有方案：</p>

<p style="clear: both"></p>

```prolog
%Prolog 断言相邻区域不同色

different(red, green). different(red, blue). different(red, yellow).
different(green, red). different(green, blue). different(green, yellow).
different(blue, red). different(blue, green). different(blue, yellow).
different(yellow, red). different(yellow, green). different(yellow, blue).
```

接下来我们以从A到I的顺序确保其相邻区域都满足于着色方案：

```prolog
%Prolog 图示相邻区域颜色不同

coloring(A, B, C, D, E, F, G, H, I) :-
 different(A, B), different(A, C), different(A, D), different(A, E),
 different(B, C), different(B, E), different(B, F),
 different(C, D), different(C, F), different(C, G),
 different(D, E), different(D, G), different(D, H),
 different(E, F), different(E, H), different(E, I),
 different(F, G), different(F, I),
 different(G, H),  different(H, I).
```

最后对所选11个区域着色，执行查询：

```prolog
coloring(A, B, C, D, E, F, G, H, I).
```

得到结果：

```prolog
A = red
B = green
C = blue
D = green
E = blue
F = red
G = yellow
H = red
I = green?
Action (; for next solution, a for all solutions, RET to stop) ?
```

如果输入a我们还能得到其他九中涂色方案，就不一一列举了。

**函数式编程**

函数式语言主要特点在于其减少甚至避免了程序中的副作用，保证了函数的输入相同则输出相同。因此函数式编程适用于并行开发以及一些严格需求无副作用的业务。

副作用：在函数或者表达式中如果其计算对外部产生影响，比如全局变量的修改、参数值的修改、抛出异常或者调用有副作用的函数，那么这个函数或者表达式就是有副作用的。

函数式语言具有函数作为一等公民、引用透明、值不变等特性，它通过函数组织代码逻辑，将元组和列表作为基本数据结构，通过高阶函数强化元组和列表的功能；通过柯里化单一化函数参数；通过惰性求值优化执行性能；

这些特性和技巧我将会用专门的一章来详细说明。

为了展现函数式语言的特点，我们用haskell来实现汉诺塔

```haskell
--Haskell 汉诺塔问题

hanoi :: Integer -> a -> a -> a -> [(a,a)]
hanoi 0 _ _ _ = []
hanoi n a b c = hanoi (n - 1) a c b ++ [(a,c)] ++ hanoi (n - 1) b a c 
hanoiIO :: Integer -> IO ()
hanoiIO n = mapM_ f $ hanoi n "A" "B" "C" where
  f (x,y) = putStrLn $ "move top disk from " ++ show x ++ " to " ++ show y
```

```haskell
hanoiIO 2

{- 输出
move top disk from "A" to "B"
move top disk from "A" to "C"
move top disk from "B" to "C"
-}
```
主要来看第三行，和之前的逻辑一样：
1. 首先将前n-1个圆盘，从起始圆柱a移动到辅助圆柱b。
2. 接下来将最后一个圆盘从起始圆柱a移动到终止圆柱c，然后递归这个函数，将剩下的n-1个圆盘从新的起始圆柱b借助新的起始圆柱a移动到终止圆柱。

这里因为每次移动返回的是一个元组类型的列表，代表从哪个圆柱移动到哪个圆柱，所以我们可以直接对最后一个圆盘的移动绑定为[(a,c)],也就是起始圆柱到终止圆柱。而我们在第二行中规定了当n=0时返回空，所以当n=1时函数实际执行了[]++[(a,c)]++[]，其中++是haskell中列表合并运算，所以n=1本身就返回了[(a,c)]，类似的我们上面执行的n=2则返回了[(a,b), (a,c), (b,c)]。

其递归过程如下：

```
hanoi 2 a b c => hanoi 1 a c b + [(a,c)] + hanoi 1 b a c
hanoi 1 a c b => hanoi 0 a b c + [(a,b)] + hanoi 0 c a b
hanoi 1 b a c => hanoi 0 b c a + [(b,c)] + hanoi 0 a b c
hanoi 0 _ _ _ => []
--------------------------------------------------------
hanoi 2 a b c => []+[(a,b)]+[]+[(a,c)]+[]+[(b,c)]+[] 
              => [(a,b), (a,c), (b,c)]
```

下面的hanoiIO只是将列表中的移动信息文本化输出，这里可以不必关心。