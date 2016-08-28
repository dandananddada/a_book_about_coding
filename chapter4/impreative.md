## 命令式编程

命令式范式侧重与描述程序的执行过程，它由一些列的命令组成，通过语句（statements）改变程序中的状态。

```
if(x > 0) y = x;
else y = -x;
```

#### 语句（statements）

语句分为基本语句和复合语句两种，其中基本语句包括：断言（assertion）、赋值（assignment）、跳转（goto）、返回（return）、调用（call）。复合语句又包括区块（block）、循环（do-loop, for-loop, while-loop）条件（if, if-else）、分支（switch）、伴随（with）、异常（try-catch）。

**赋值**

赋值语句用于对变量进行赋值操作。

int a = 2;

**返回**返回语句用于返回计算结果。

```java
public static int add(int a, int b){ return a + b;}
function abs(a){ return a > 0 ? a : -a;}
```

**断言**

断言一般用来判断表达式结果的真假。

```java
assert(1 > 0);System.out.println("1 large than 0"); //1 large than 0
assert(1 < 0);System.out.println("1 small than 0"); //Exception in thread "main" java.lang.AssertionError
```

也有些测试库用来比较期望值与实际值是否相等。

```java
//junit
assertEquals(2, 1+1);
```

```javascript
//jasmine
expect(1+1).toBe(2); //true
```

**调用**

函数调用，用于执行定义的函数。

```javasript
add(1, 3);
abs(-4)
```

**跳转**

跳转到指定标记，因为goto语句使程序难以理解，所以即使在保留了goto语句的高级语言（C\/C++）中也不建议使用。

```c
int n = 0;
loop: if(n < 3){ n++; goto loop;}
```

**条件**
命令式编程中最具代表性的语句就是条件、循环和分支。这些语句负责程序的逻辑控制。

1. 如果`表达式`成立，则执行`语句`。

```
if([表达式]) [语句]
```

2. 如果依次判断`表达式*`，如果成立则执行`语句*`，结束语句，如果任何`表达式*`都不成立则执行`else`语句。

```java
if([表达式1])         [语句1]
else if([表达式2])    [语句2]
else if([表达式3])    [语句3]
else                 [else语句]
```

```java
if(x == 1) System.out.println("one")；
```

`javaif(x > 0) System.out.println("x is positive number")；
else if(x`

**循环**

**分支**

**异常**

