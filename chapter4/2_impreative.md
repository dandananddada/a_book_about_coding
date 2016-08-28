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

**1. if：**如果`表达式`成立，则执行`语句`。

```
if([表达式]) [语句]
```

```java
if(x == 1) 			System.out.println("one")；
```

**2. if-else if：**依次判断表达式，如果`[表达式*]`成立则执行`[语句*]`，结束判断。

```java
if([表达式1])         [语句1]
else if([表达式2])    [语句2]
else if([表达式3])    [语句3]
```

```java
int x = 2;
if(x == 1)			System.out.println("one");
else if(x == 2)		System.out.println("2");
else if(x == 2)		System.out.println("two");
//=> 2
```

**3. if-else：**如果`[表达式]`成立，则执行`[语句1]`否则执行`[语句2]`。

```java
if([表达式]) 			[语句1]
else   				[语句2]
```

```
int x = 1;
if(x) 				[语句1]
else 				[语句2]
```

**循环**

**1. for：**执行`[初始化表达式]`，然后执行`[条件判断表达式]`，如果成立则执行`[语句]`和`[增量表达式]`，继续执行`[判断表达式]`，如果不成立则结束循环。

```java
for([初始化表达式]; [条件判断表达式]; [增量表达式])	[语句]
```

```java
for(int i=0; i<=2; i++)
	System.out.print(i+1);
//=>123
```

**2. while：**执行`[条件判断表达式]`，如果满足则执行`[语句]`。

```
while([条件判断表达式]) 		[语句]
```

```java
int i = 1;
while(i<=3){
	System.out.print(i);
	i+=1;
}
//=>123
```

**3. do-while：**首先执行`[语句]`，然后执行`[条件判断表达式]`，如果成立则重复上述操作，否则结束循环。

```java
int i = 1;
do{
	System.out.println(i);
	i++;
}while(i<=3);
//=>123
```

**分支**

`switch`和`if-else if`功能类似，部分语言对switch语句有优化，所以多路分支时switch效率相对高些，不过`switch`语句不易维护，所以处于这方面考虑可以用`if-else if`语句替换，大多数情况下`switch`和`if-else if`可以相互替换，但是对于不可列举的情况下只能用`if-else if`。

**1. switch：**如果依次判断`[变量表达式]`和`[常量判断表达式*]`是否相等，如果相等则执行`[语句*]`，否则执行`[默认分支语句]`.

```java
switch([变量表达式]){
	case [常量判断表达式*]
		[语句*]
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
//=>1
```

**异常**

异常处理的作用是在程序正常运行中对用来对不合法的输入进行控制和提示，防止程序崩溃。

**1 try-catch-finally：** 执行`[异常语句]`如果发现异常则执行`[异常处理语句]`，最后无论是否存在异常都会执行`[终止语句]`。

```
try { [异常语句] }
catch{ [异常处理语句] }
finally{ [终止语句] }
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


