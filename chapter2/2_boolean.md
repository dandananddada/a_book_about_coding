# 布尔类型

布尔类型用于逻辑真/假判断，值为`true`和`false`。
```Java
//java
if(true)        System.out.println("true");
```

#### 类型转换

**隐式转换**

一般弱类型语言中也会可以用数字类型代替同样的逻辑判断。
```javascript
//javascript
if(1)           console.log("true");
```
其原理是弱类型语言会将数字类型隐式转换为布尔值。
我准备用JavaScript为例来说明boolean类型和其他类型的转化。
```javascript
1   == true;            //=> true,  [1]
0   == false;           //=> true,  [1]
1   === true;           //=>false,  [2]
''  == true;            //=>false,  [3]
''  == false;           //=> true,  [3]
's' == true;            //=>fasle,  [4]
's' == false;           //=>false,  [4]
!'' == true;            //=>true,   [5]
!'s' == true;           //=>false,  [5]
!!'s' == true;          //=>true,   [5]
Boolean('s') == true;   //=>true,   [6]
Boolean('') == false;   //=>true,   [6]
undefined == false;     //=>false,  [7]
!undefined == true;     //=>true,   [7]
null == false;          //=>false,  [7]
!null == true;          //=>true,   [7]
```
1. 在JavaScript中boolean会转化为数字，其中`true`会转换为`1`，`false`会转换为`0`，所以相等。
2. 而强制等于`===`会禁止类型转换，所以这里`1`不等于`true`
3. 空字符会转换为true。
4. 非空字符和布尔类型始终不等。
5. 通过4可知是非空字符本身即不是`true`也不是`false`，但是取反后是`false`，两次取反自然就是`true`。
6. 通过Boolean强制转换非空字符就是`true`,空字符是`false`
7. 这里`undefined`和`null`类型与布尔类型比较都返回false，但是取反后表示真，所以有些教程会说`undefined``nill`和`NaN`都表示`false`，这个说法不是很准确，但是在if表达式中确实又是如此应用的。
8. 
```javascript
//第一个if表达式判断为false，不执行内部代码。
if(undefined)   console.log("will not execute");
if(!null)       console.log("will execute");
//=>will execute
```

**显式转换**

Java对于boolean类型的处理上和JavaScript有些不同，他禁止数字类型和布尔类型的转换，同时字符类型和布尔类型要通过API。
```java
boolean a   = true == 1;          
//=>incomparable types: boolean and int
String  a   = "True";
String  b   = "another";
boolean c   = true == Boolean.parseBoolean(a);
//=>true
boolean d   = true == Boolean.parseBoolean(b);
//=>false
```
上面的例子可以看出`Boolean.parseBoolean`会将字符串`true`（不区分大小写）转换为布尔值`true`，而其他字符串都会转换为`false`。

另外Java可以对一个布尔类型实例化为null，但是null本身是不能和boolean值进行比较的。
```java
Boolean a = true == null;
//=>incomparable types: boolean and <null>
Boolean a = null
if(a == null) System.out.println("true");   //=>true

```

最后要说明的是并非所有动态类型语言都会把`0`当做`false`看待。
比如下面的ruby代码
```ruby
if(0) puts "zero equals true" end
#=>zero equals true
```
可以看出在ruby中`0`等价于`true`，所以在使用0，1做条件判断时最稳妥办法还是用`==`运算返回一个布尔值。
```java
//java
int a = 1;
if(a == 1)  System.out.println("equals true");     //=>equals true
```
```Ruby
#ruby
a = 1
puts "equals true" if a != 0            #=>equals true
```

#### 与或运算

与布尔类型最为相关的运算莫过于与运算（&&）与或运算（||）了。

**与运算：** [表达式A]&&[表达式B]其中[表达式A]和[表达式B]必须同时为真才返回`true`，否则都返回`false`。

**或运算：** [表达式A]||[表达式B]其中`表达式A`和`表达式B`必须同时为假才返回`false`，否则都返回`true`。

因为与非运算的性质，所以在与运算中只要`表达式A`为假则`表达式B`就不会运算了。同理在或运算中只要`表达式A`为真则`表达式B`也不会运算了。

```java
boolean a = false;
boolean b = true;
boolean c = a && (b = !b);
System.out.println(b);		//true
```
```java
boolean a = true;
boolean b = false;
boolean c = a || (b = !b);
System.out.println(b);		//false
```

因为这样的特性再配合弱类型，可以简化一些赋值语句。

```javascript
var a, b = "";
if(b == "") a = "b should not empty";
else        a = b;
console.log(a);		//b should not empty
```

上述代码中我们把a赋值为b，如果b为空则设置一个缺省字符串"b should not empty"，可以使用或运算的特性配合弱类型简化if语句为：

```javascript
var b = ""
,   a = b || "b should not empty";
console.log(a);		//b should not empty
```
注意因为Java不支持字符串的与或运算，所以如果你在用Java还是规矩的使用`if-else`语句才符合Java语言的设计初衷。

与JavaScript类似，Ruby更是提供了`||=`运算符。
```ruby
a ||= "default value"
```
`||=`和`+=`类似即：`a = a || "default value"`，因为`a`本身就是空所以`表达式A`返回`false`，执行`表达式B`对`a`设置缺省值：`default value`。其本质还是或运算与弱类型的配合，只是固定了第一个表达式，于是在Ruby中的`||=`就变长了：`如果a没有被赋值过，则赋值为`这样的命令。
```