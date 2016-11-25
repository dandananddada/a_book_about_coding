# 布尔类型

布尔类型用于逻辑真假判断，表示真时值为true，表示假时值为falsJ。

```Java
if(true)        System.out.println("true");
//true
```
布尔类型最基础的用法是逻辑判断，但不同语言类型转换处理方式的不同，使布尔类型在使用上有些区别，我想通过类型转换来进一步说明在不同语言下布尔类型使用上的一些差异。

#### 类型转换

**隐式转换**

弱类型语言中可以用数字类型代替逻辑判断。
```javascript
//Javascript 布尔类型使用
if(1)           console.log("true");
```
其原理是弱类型语言会将数字类型隐式转换为布尔值。

我准备用JavaScript为例来说明boolean类型和其他类型的隐式转换。
```javascript
//JavaScript 布尔类型隐式转换
1   == true;            //true,   [1]
0   == false;           //true,   [1]
1   === true;           //false,  [2]
''  == true;            //false,  [3]
''  == false;           //true,   [3]
's' == true;            //fasle,  [4]
's' == false;           //false,  [4]
!'' == true;            //true,   [5]
!'s' == true;           //false,  [5]
!!'s' == true;          //true,   [5]
Boolean('s') == true;   //true,   [6]
Boolean('') == false;   //true,   [6]
undefined == false;     //false,  [7]
!undefined == true;     //true,   [7]
null == false;          //false,  [7]
!null == true;          //true,   [7]
```
1. 在JavaScript中boolean会转化为数字，其中true会转换为1，false会转换为0，所以相等。
2. 而强制等于===会禁止类型转换，所以这里1不等于true
3. 空字符会转换为true。
4. 非空字符和布尔类型始终不等。
5. 通过4可知是非空字符本身即不是true也不是false，但是取反后是false，两次取反自然就是true。
6. 通过Boolean强制转换非空字符就是true,空字符是false。
7. 这里undefined和null类型与布尔类型比较都返回false，但是取反后表示真，所以有些教程会说undefined、nill和NaN都表示false，这个说法不是很准确，但是在if表达式中确实又是如此应用的。

```javascript
//JavaScript 第一个if表达式判断为false，不执行内部代码。
if(undefined)   console.log("will not execute");
if(!null)       console.log("will execute");
//=>will execute
```

和JavaScript中0会转换为false不同，并非所有弱类型语言都会把0当做false看待。

```ruby 
#Ruby 0转换为true
if(0) puts "zero equals true" end
#=>zero equals true
```
可以看出在ruby中0等价于true，所以在使用0，1做条件判断时最稳妥办法还是用==运算返回一个布尔值。
```java
//java
//Java 数字类型与布尔类型转化（这里为了区分类型转换特别使用转化这个词）
int a = 1;
if(a == 1)  System.out.println("equals true");     
//equals true
```
```Ruby
#ruby 数字类型与布尔类型转化
a = 1
puts "equals true" if a != 0            
#equals true
```

**显式转换**

一般强类型语言在处理布尔类型与其他类型间转换时，不像弱类型语言那么随意，通常需要显式转换。
```java
boolean a   = true == 1;          
//incomparable types: boolean and int    [1]

String  a   = "True";
String  b   = "another";
boolean c   = true == Boolean.parseBoolean(a);
//true    								 [2]

boolean d   = true == Boolean.parseBoolean(b);
//false									 [2]
```
1. Java中不允许数字类型与布尔类型间转换。
2. `Boolean.parseBoolean`会将字符串true（不区分大小写）转换为布尔值true，而其他字符串都会转换为false。

另外Java可以对一个布尔类型实例化为null，但是null本身是不能和boolean值进行比较的。

```java
Boolean a = true == null;
//incomparable types: boolean and <null>

Boolean a = null
if(a == null) System.out.println("true");   
//true
```

除了布尔类型本身的条件判断外，布尔类型间的与或运算也有着特殊的用途。

#### 与或运算

**与运算（&&）：** [表达式A]&&[表达式B]，其中表达式A和表达式B必须同时为真才返回true，否则都返回false。

**或运算（||）：** [表达式A]||[表达式B]，其中表达式A和表达式B必须同时为假才返回false，否则都返回true。

基于如上运算规则，只要与运算中表达式A为假则返回值就必然为false，所以表达式B本身就不需要计算了。同理在或运算中只要表达式A为真则表达式B也不需要计算了。

```java
//Java 与运算中如果表达式A为假，则表达式B不执行

boolean a = false;
boolean b = true;
boolean c = a && (b = !b);
System.out.println(b);		
//true
```
```java
//Java 或运算中如果表达式A为真，则表达式B不执行

boolean a = true;
boolean b = false;
boolean c = a || (b = !b);
System.out.println(b);		
//false
```

在动态类型语言中配合与或运算的特性，可以简化一些赋值语句。

```javascript
//JavaScript 通过逻辑判断进行赋值

var a, b = "";
if(b == "") a = "b should not empty";
else        a = b;
console.log(a);		//b should not empty
```

上述代码中如果b不为空则把a赋值为b，如果b为空则a赋值为"b should not empty"。使用或运算简化if语句为：

```javascript
//JavaScript 动态类型语言通过与或运算简化赋值操作

var b = ""
,   a = b || "b should not empty";
console.log(a);		
//b should not empty
```

与JavaScript类似，Ruby更是提供了||=运算符。
```ruby
#Ruby ||=运算符使用

a ||= "default value"
```
||=和+=类似，即：`a = a || "default value"`，因为a本身为空，所以表达式A返回false，继续执行表达式B对a赋值：default value。于是在Ruby中的||=就变成了：“如果a没有被赋值过，则赋值为...”这样的命令，其实||=本质还是或运算特性的使用，只是Ruby中固定了一个运算符来表述。
```