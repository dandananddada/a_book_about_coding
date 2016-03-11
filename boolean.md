# 布尔类型

布尔类型用于逻辑真/假判断，值为`true`和`false`。
```Java
//java
if(true)        System.out.println("true");
```
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
```
1. 在JavaScript中boolean会转化为数字，其中`true`会转换为`1`，`false`会转换为`0`，所以相等。

2. 而强制等于`===`会禁止类型转换，所以这里`1`不等于`true`
3. 空字符会转换为true。
4. 非空字符和布尔类型始终不等。
5. 通过4可知是非空字符本身即不是`true`也不是`false`，但是取反后是`false`，两次取反自然就是`true`。
6. 通过Boolean强制转换非空字符就是`true`,空字符是`false`

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

另外并非所有动态类型语言都会把`0`当做`false`看待。
比如下面的ruby代码
```ruby
if(0) puts "zero equals true" end
#=>zero equals true
```
可以看出在ruby中`0`等价于`true`，所以在使用0，1做条件判断时最稳妥办法还是用`==`运算返回一个布尔值。
```java
int a = 1;
if(a == 1)  System.out.println("true");     //=>true
```
```ruby
a = 0
if(a == 0) puts "false" end             #=>false
```


