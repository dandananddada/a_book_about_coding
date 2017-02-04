# 第二章 数据类型中的值类型

继第一章第五节提到的值类型概念，这里将详细说明主流编程语言中常见的值类型，以及与这些数据类型相关的概念。

* 介绍布尔类型、以布尔类型为主的类型转换问题。
* 介绍数字类型、数字在内存中如何存储。
* 介绍字符串类型、常用的字符编码。
* 介绍不常见的原子类型。


### 布尔类型

布尔类型用于逻辑真假判断，表示真时值为true，表示假时值为false。

```Java
//Java 布尔类型
if(true)        System.out.println("true");    //true
```
布尔类型最基础的用法是逻辑判断，因不同语言对类型转换的处理方式不同，使布尔类型在使用上有些区别。我想通过类型转换来进一步说明在不同语言下布尔类型使用上的一些差异。

#### 类型转换

**隐式转换**

弱类型语言中可以用数字类型代替逻辑判断。
```javascript
//Javascript 布尔类型使用
if(1)           console.log("true");    //true
```
因为弱类型语言会将数字类型隐式转换为布尔类型，然后进行逻辑判断。

这里以JavaScript为例来说明boolean类型和其他类型的隐式转换。

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

[1] 在JavaScript中布尔类型会转化为数字类型，其中true会转换为1，false会转换为0。

[2] 强制等于`===`会禁止类型转换，所以这里1不等于true

[3] 空字符会转换为true。

[4] 非空字符和布尔类型始终不等。

[5] 通过[4]可知是非空字符本身即不是true也不是false，但是取反后是false，两次取反就是true。

[6] 布尔类型转换为非空字符返回true,转换为空字符返回false。

[7] 这里undefined和null类型与布尔类型比较都返回false，但是取反后表示真，所以有些教程会说undefined、nill和NaN都表示false，这个说法不是很准确，但是在if表达式中确实又是如此应用的。


```javascript
//JavaScript 布尔类型隐式转换

if(undefined)   console.log("will not execute");    //undefined返回为false，不执行后续操作
if(!null)       console.log("will execute");    //will execute
```

与JavaScript不同，并非所有弱类型语言都会把0当作false看待。

```ruby 
#Ruby 数字0转换为布尔类型true
if(0) puts "zero equals true" end    #zero equals true
```

可以看出在ruby中0等价于true，所以在使用0，1做条件判断时最稳妥办法还是用`==`运算返回一个布尔值。

```java
//Java 数字类型与布尔类型转化

int a = 1;
if(a == 1)  System.out.println("equals true");    //equals true
```
```Ruby
#Ruby 数字类型与布尔类型转化

a = 1
puts "equals true" if a != 0    #equals true
```

**显式转换**

一般强类型语言在处理布尔类型与其他类型间转换时，不像弱类型语言那么随意，通常需要显式转换。
```java
//Java 字符串显式转换为布尔类型

boolean a   = true == 1;          
//incomparable types: boolean and int    [1]

String  a   = "True";
String  b   = "another";
boolean c   = true == Boolean.parseBoolean(a);
//true                                   [2]

boolean d   = true == Boolean.parseBoolean(b);
//false                                  [2]
```

[1] Java中不允许数字类型与布尔类型间转换。

[2] Boolean.parseBoolean会将字符串true（不区分大小写）转换为布尔值true，而其他字符串都会转换为false。

另外Java可以对一个布尔类型实例化为null，但是null本身是不能和boolean值进行比较的。

```java
//Java 布尔类型与null间转换

Boolean a = true == null;                    //incomparable types: boolean and <null>

Boolean a = null
if(a == null) System.out.println("true");    //true
```

除了布尔类型本身的条件判断外，布尔类型间的与或运算也有着特殊的用途。

#### 与或运算

与运算：`[表达式A]&&[表达式B]`，其中表达式A和表达式B必须同时为真才返回true，否则都返回false。

或运算：`[表达式A]||[表达式B]`，其中表达式A和表达式B必须同时为假才返回false，否则都返回true。

基于上述运算规则，在与运算中表达式A为假则运算结果必然为false，所以表达式B本身就不需要计算了。同理在或运算中只要表达式A为真则表达式B也不需要计算了。

```java
//Java 与运算中如果表达式A为假，则表达式B不执行

boolean a = false;
boolean b = true;
boolean c = a && (b = !b);

System.out.println(b);    //true
```
```java
//Java 或运算中如果表达式A为真，则表达式B不执行

boolean a = true;
boolean b = false;
boolean c = a || (b = !b);

System.out.println(b);    //false
```

在动态类型语言中配合与或运算的特性，可以简化一些赋值语句。

```javascript
//JavaScript 通过逻辑判断进行赋值

var a, b = "";
if(b == "") a = "b should not empty";
else        a = b;

console.log(a);    //b should not empty
```

上述代码中如果b不为空则把a赋值为b，否则赋值为：`b should not empty`。

```javascript
//JavaScript 动态类型语言通过或运算去除if语句

var b = ""
,   a = b || "b should not empty";

console.log(a);    //b should not empty
```

与JavaScript类似，Ruby更是提供了`||=`运算符。

```ruby
#Ruby ||=运算符使用
a ||= "default value"
```
`||=`和`+=`类似，即：`a = a || "default value"`，因为a本身为空，所以表达式A返回false，继续执行表达式B对a赋值为：default value。于是Ruby中的`||=`就变成了`如果a没有被赋值过，则赋值为...`这样的命令，其实`||=`还是或运算特性的使用，只是Ruby中固定了一个运算符来表述。


### 数字类型

关于数字类型我想先简单讲下其在内存中的存储，然后通过Java和JavaScript中数字类型的划分来进一步说明。

####正整数

首先我们用二进制表示一个数字，比如123。


123 = 1+2+8+16+32+64 


123 = 1\*2<sup>0</sup>+1\*2<sup>1</sup>+0\*2<sup>2</sup>+1\*2<sup>3</sup>+1\*2<sup>4</sup>+1\*2<sup>5</sup>+1\*2<sup>6</sup>

换作二进制表示为：1111011，低位从右向左排序，即右边第一个数字表示2<sup>0</sup>。

我们以Java的short类型为例来说明，short类型采用16位存储一个数字，目前存储数字123需要使用7位空间，还剩下9位，剩余的高位（左侧）用0来填充，因此最终表示为`00000000 01111011`。同理如果你用int类型来存储的话就是一长串的0：`00000000 00000000 00000000 01111011`，因为int类型用32为来存储一个数字。

####负整数与符号位

如果你还记得初中数学课本里讲的数轴就能猜到如何表示负数，没错，因为正数和负数是对称的，所以我们只需要在正数的基础上加上一个符号就可以了，这个符号就是符号位，符号位在最高位，这样一个16位的short类型能表示的正整数就要折半了，即2<sup>15</sup>个，所以short类型正整数能表示的最大值就是2<sup>15</sup>-1 = 32767。方便理解我们用3位来说明：

| 正整数| 二进制 | 负整数| 二进制 | 
| -- | -- | -- | -- |
| 1 | 001 | -1  | 101 |
| 2 | 010 | -2  | 110 |
| 3 | 011 | -3  | 111 |
| 0 | 000 | -0  | 100 |
可以看出3位能表达的正整数为2<sup>3-1</sup>-1=3个数。

####负整数与补码

只说数据类型不说类型间的运算是无意义的。接下来我们看下正整数的加减运算如何处理。

这里以4+7为例来说明加法运算：
```
+4:  0| 0 1 0 0     
(+)           
+7:  0| 0 1 1 1
_______________
=    0| 1 0 1 1

+11: +| 8+0+2+1
```

以12-9为例来说明减法运算：

```
+12: 0| 1 1 0 0
(-)           
+9:  0| 1 0 0 1
_______________
=    0| 0 0 1 1

+3:  0| 0+0+2+1
```

可以看出正整数加减法和我们想要的结果是一致的。

接下来看下负整数加减法，这里说明下负整数与负整数加减运算（不运算符号位）。

以(-8)+(-6)为例说明负整数加法运算
```
-8:  1| 1 0 0 0     
(+)           
-6:  1| 0 1 1 0
_______________
=    1| 1 1 1 0

-14: -| 8+4+2+0
```

以(-6)-(-5)为例
```
-6:  1| 0 1 1 0     
(-)           
-5:  1| 0 1 0 1
_______________
=    1| 0 0 0 1

-1:  -| 0+0+0+1
```

好极了，到目前为止都是理想中的结果，那么接下来看个不如人意的例子。
```

-3: 1| 0 0 1 1     
(+)           
+3: 0| 0 0 1 1
______________
=   1| 0 1 1 0

-6: -| 0+4+2+0
```

可以看出-3+3=-6这是不正确的运算结果，我们期待的结果应该是
-3+3=0。这里为了得到正确的结果，我们先反向构造出一个和为0的情景：
```
-3: 1| 0 0 1 1     
(+)           
+3: 0| 1 1 0 1
______________
=   1| 0 0 0 0

0:  -| 0+0+0+0
```

可以看出在`0011`的基础上，按位取反然后加1（1*2<sup>0</sup>）即为`1101`，这里按位取反再加一后得到的`1101`就是`0011`的补码。我们可以通过补码的定义修补数学意义上相反数和不为零的漏洞。

接下来我们用补码来表示负数

| 正整数| 二进制 | 负整数| 二进制 | 
| -- | -- | -- | -- |
| 1 | 001 | -1  | 111 |
| 2 | 010 | -2  | 110 |
| 3 | 011 | -3  | 101 |
| 0 | 000 | -0  | 100 |

因为`000`已经可以表示0了，为了避免资源的浪费我们可以用`100`表示-4，这样一来一个有符号的、16位的、补码表示的类型就可以表示2<sup>15</sup>-1个正整数和2<sup>15</sup>个负整数了，范围就是：-32768 ~ 32767。

####小数和精度

二进制与十进制的小数转换同整数一样。这里用110.11来说明二进制小数到十进制的转换。

110.11 = 1 \* 2<sup>2</sup>+1 \* 2<sup>1</sup>+0 \* 2<sup>0</sup>+1 \* 2<sup>-1</sup>+1 \* 2<sup>-2</sup> = 6.75

十进制小数转换为二进制，规则为

1. 将小数位乘以二。
2. 取结果的整数位，作为高位。
3. 取结果的小数位重复上述操作，如果结果小数位为0则结束运算。

我们将0.725转换为二进制。

1. 0.125 * 2 = 0.25，取整为0，取0.25循环
2. 0.25*2 = 0.5， 取整为0， 取0.5循环
3. 0.5*2 = 1.0，取整为1，取0结束
4. 按顺序拼接取整后数字到高位：001

这样0.625表示为二进制就是001。
对于小数的存储，常见的方式有32位和64位两种，在32位中把第32位作为符号位，第31位到23位这8位作为小数位，剩下的23位用来表示整数。在64位中把第64位作为符号位，第63位到52位这11位作为小数位，剩下的52位用来表示整数位。

>以上小数存储规则可以参考IEEE754规范

####小数运算误差

如果你尝试过用二进制表示0.3或者0.2就会发现，即使用64位（双精度）来表示，其实也只能是一个近似值，因为0.3这种小数是无法通过有限位的二进制表示的。

这样就引出了一个有趣的问题，我们以JavaScript来说明，在JavaScript中所有数字都是按照64位（11位小数位，52位整数位）方式存储的，这样我们得到0.3、0.2、0.1的二进制表示如下：
```
0.3： 0.0100 1100 110
0.2： 0.0011 0011 001
0.1： 0.0001 1001 100
```
我们取0.3与0.2的后三位做减法与0.1的后三位比较
```
0.3-0.2:  101
0.1:      100
```
很明显两者是不相等的，所以如下代码是合乎情理的：

```javascript
//JavaScript 0.2-0.3 != 0.1

0.3 - 0.2 === 0.2 - 0.1;    //false
```

这不是哪一门编程语言所存在的问题，这是采用了IEEE754规范的计算机语言都兼备的问题，所以在计算机程序运算中尽可能采用小单位运算，比如`0.3T = 300KG`，因为你知道的，计算机存储300要比存储0.3准确的多。

基本的数字存储说完后，我们看看不同语言对数字类型的划分是什么样的：

####Java的数字类型

|名称 | 类型 | 位数 | 符号位 | 范围 |
| -- | -- | -- | -- | -- | -- |
|字节型| byte | 8 | 有 | [-128, 127] |
|短整型| short | 16 | 有 | [-32768, -32767]|
|整型| int | 32 | 有 |[-2<sup>31</sup>, 2<sup>31</sup>-1]
|长整型| long | 64 | 有 |[-2<sup>63</sup>, 2<sup>63</sup>-1]
|浮点型| float | 32 |有| IEEE 754 binary floating point |
|双精度浮点型| double | 64 | 有 | IEEE 754 binary floating point |


* 一般小计量的数字存储，可以用byte和short来节省内存开支。
* 对于货币这种需要精确数值的，不能用float和double，可以用Java提供的BigDecimal类。
* 在Java8以后int和long可以用来表示无符号位的整数，取值范围分别为：[0, 2<sup>32</sup>-1]，[0, 2<sup>64</sup>-1]

####其他语言数字类型

我们以Java的数字类型为标准，来对比下其他语言的数字类型有哪些不同。

**JavaScript和Ruby**

同Java不同，JavaScript和Ruby这样的弱类型语言，本身数字类型没有做特别细致的划分，它们都用Number类型表示数字，但细节处理上JavaScript和Ruby又是不同的。

JavaScript并没有区分整数和小数，所有的数字都是遵循IEEE 754 标准采用64位float进行表示，而Ruby本身则是有区分Integer、Float和Rational。Integer又分为Fixnum和Bignum；Rational表示实数，类似于Java的BigDecimal类。

**Python**

对比Ruby提供的实数类型，Python和R则提供了一个更广泛的数据类型：complex number（复数类型），它表示为a+bj的形式，其中a和b都是浮点类型，a表示实部，b表示虚部。

另外Python的数字类型还包括int、long和float，这一点和Ruby其实是相似的，不同的是Python的long类型是没有长度限制的，而int类型超过长度限制则会转换为long类型（这就是为什么有人会说Python只有long和float两种数字类型）。

**基于JVM的二代语言**

另外像Scala、Groovy这种基于JVM的二代语言，其数字类型是沿用自Java的，无非是Scala作为弱类型语言提供了类型推断机制，在使用数字类型时无需特别声明。

另外一门基于JVM的Lisp方言Clojure，它没有效仿Java的数字类型，而是采用了Integers和Float两种类型，其中Integers又区分为Decimal Integers（整数类型）、Octal Numbers（八进制数）、Hexadecimal Numbers（十六进制数）、Radix Numbers（进制类型）。类似的Perl语言也提供了Hexadecimal（十六进制）和Octal（八进制）两种类型。

Clojure的进制类型可以把任意进制数转换为十进制数，因此如果你用的语言提供了这样的类型，在处理进制转换时相当方便。

这里单独说明下Clojure的进制类型，上面我们讲过了二进制的算法，八进制和十六进制的计算方法和二进制类似，不同的是为了和十进制区分，八进制以0开头，而十六进制以0x开头。
```
0175
0x1A
```
我们将这两个数字表示为10进制：

<pre style="font-size: 0.85em">0175  = 1 * 8<sup>2</sup> + 7 * 8<sup>1</sup> + 5 * 8<sup>0</sup> = 125
0x2E = 2 * 16<sup>2</sup> + 14 * 16<sup>1</sup> + 10 * 16<sup>0</sup> = 746</pre>


注意16进制中用A表示10，B表示11，C表示12，依此类推，所以E表示14。而Clojure的Radix Numbers可以定义进制的底数，比如刚刚的0175就可以表示为8r175，0x2EA就可以表示为16r2ea，r前表示底数，r后表示值。因为数字从0-9共10个，字母从a-z共26个，所以底数的范围是1-36，即Clojure的Radix Numbers最高可以计算36进制数转换。
```Clojure
;;Clojure 11进制数12转换为十进制数
(println (format "%s" 11r12)))    ;;13
```
**微软系语言F#、C#、VB.net**

我们以C#为例说明，C#除了提供了像Java中byte、short、int、long、double、float这些数字类型外，还特别针对有符号的short、int、long提供了无符号的ushort、uint、ulong，这些无符号类型都无法表示负整数，但相反的却可以表示两倍于符号类型的正整数。


### 字符类型

在说明字符类型之前，我想先说明下字符编码。

####字符编码

计算机是只识别二进制的，所以和数字一样我们要想办法把字符也映射为一串01的记号，数字类型采用十进制与二进制的转换解决了这个问题。对于字符类型，我们通过制定一个字符到二进制数的映射表来实现字符与二进制数的转换。

**ASCII**

我们用两个字节，即8位（相应可以表示256个符号）来表示一些我们常用的字符，这一套字符对照表就是ASCII码，但是ASCII码只使用了后7位（第八位置为0）规定了与常用的128个字符的对应关系，这128个字符包含了a-z、A-Z、0-9以及一些标点符号和控制符号。

ASCII码是针对英文字符的规范标准，但不同的国家使用的语言不同，不同的国家规定的对照表也就不同，用多种不同的对照表去解析一个字符就会出现不同的结果。正因为不同编码规定的同一个二进制数表示的字符不同，所以如果编码和解码的规则不一致，就会出现乱码问题。因此我们需要一个通用的编码、解码规范来避免同一二进制数解析为不同的字符。

**Unicode**

为了解决上述问题，提出了Unicode字符集，Unicode规定了字符和二进制数的对应关系，但没有给出具体的存储方式。因为英文字符用一个字节就足以表示，但是汉语字符可能要需要2个字节才能表示。如果以一字节位单位进行存储，就无法满足汉语字符所需的存储空间，如果以两字节为单位进行存储，英文字符就会浪费掉一部分存储空间。

基于上述问题，又提出了几套不同的存储方案。

**UTF-8**

UTF-8采用变长的存储方式解决了上述问题，为了让计算机能区分变长字符存储所需的字节数，UTF-8做出如下规定：

* 对于单字节字符，字节第一位是0，后面七位为这个符号的Unicode码，因此英文字母的Unicode码实际上和ASCII码是一致的。
* 对于需要N个字节来存储的符号，第一个字节的前N位为1，第N+1位为0；后面N-1个字节的前两位都是10。比如三个字节存储的字符对应为:`1110xxxx 10xxxxxx 10xxxxxx`

这里UTF-8只是Unicode编码的一种存储实现，相应的存储方式还有UTF-16和UTF-32。

**UCS-2**

在早期统一字符集的时候存在过两个团队，一个是Unicode团队，另个一就是UCS团队，UCS团队提出的UCS-2编码采用了两个字节16位的方式来存储一个字符，UCS-2是向下兼容ASCII的，也就是说UCS-2对ASCII中规定的字符与二进制数的对应关系予以保留，在此基础上再追加其他字符。但随后Unicode推出了UCS-2的超集Unicode-16，因为不需要两套规范，所以UCS-2就被并入Unicode-16。

**UTF-16**

UTF-16规定用十六进制、两个字节来存储字符，其范围是`0x0000 - 0xFFFF`
但两字节只能存储2<sup>16</sup>即65535个字符，这是不够的，因此要存储更多字符就需要两个存储单元。但是这就又出现一个问题，怎么判断两个存储单元是表示两个字符还是表示一个字符。为了解决这个问题，UTF-16编码规范将`0xD800 - 0xDFFF`这段设置为空，不对应任何字符。然后又把这一段空间分成两段：
`0xD800-0xDBFF`和`0xDC00-0xDFFF`，对于超出`0xFFFF`的字符追加的那一个存储单元映射为这两段的数值，并放在前面。这样计算机检测到第一个存储单元的两个字节在`0xD800-0xDBFF`和`0xDC00-0xDFFF`范围时就知道需要和后一个存储单元配合来定位为一个字符了。正是因为这个策略导致单个存储单元的UTF-16编码所能表示的字符数量要少。

    目前Java、C#、Object-C、JavaScript这些语言内部编码都采用的UTF-16（严格的说JavaScript采用的是UCS-2编码）。

####字符长度

刚刚也说过有些特殊字符需要两个存储单元来保存，所以采用UTF-16编码的语言通过length函数来取这个字符的长度时，就会返回2。

```java
//Java 字符𥊍长度
System.out.println("𥊍".length());    //2
```
```javascript
//JavaScript 字符𥊍长度
console.log("𥊍".length);    //2
```
```ruby
#Ruby 字符𥊍长度
puts "𥊍".length    #1
```
注意Ruby、Go、Rust这些语言已经开始使用UTF-8作为内部编码了，所以Ruby对特殊字符取长度返回1。

####字符和字符串

在Java和C++中字符串并不是一个原始的数据类型，而是一个字符序列 ，是一个类，字符才是基本数据类型。但是JavaScript和Ruby中字符串是作为基本数据类型提供的，相应的并没有细化字符的概念，而字符只是长度为1的字符串。
```java
//Java 字符和字符串

public static void main(String args[]){
  char normal = 'a';
  char unicode = '\u039A';
  String str = "string";
  char[] chars = {'s', 't', 'r', 'i', 'n', 'g'};
  String charstr = new String(chars);
  
  System.out.println(normal);    //a
  System.out.println(unicode);   //K
  System.out.println(str);       //string
  System.out.println(charstr);   //string
}
```
```ruby
char = 'a'
string = 'string'

puts char      #a
puts string    #string
```
注意虽然Java中String是一个类，但字符串拷贝的时候是值类型，而不是引用类型。
```java
//Java 字符串是值类型，拷贝修改不会影响源

public static void main(String args[]){      
  char[] chars = {'s', 't', 'r', 'i', 'n', 'g'};
  String charStr = new String(chars);
  String equalCharStr = charStr;

  charStr = "string charStr";
  equalCharStr = "string equalCharStr";

  System.out.println(charStr);          //string charStr
  System.out.println(equalCharStr);     //string equalCharStr

  String str = "string";
  String equalStr = str;

  str = "string str";
  equalStr = "string equalStr";

  System.out.println(str);             //string str
  System.out.println(equalStr);        //string equalStr
}
```
Ruby和JavaScript中字符串本身就作为原始类型定义，所以字符串拷贝自然也是值类型拷贝。
```ruby
#Ruby 字符串拷贝，也是值类型拷贝

str = "string"
equalStr = str

str = "string srt"
equalStr = "string equalStr"

puts str         #string str
puts equalStr    #string equalStr
```

####字符串修改及内存优化

基础类型与栈内存分配中有介绍，因为字符串是值类型，所以字符串频繁修改时会消耗大量内存，因此部分语言本身提供了类来优化内存的使用。

```java
//Java StringBuffer优化字符串内存占用

public static void main(String args[]){
  char[] chars = {'a', 'b', 'c'};
  String s = new String();
  StringBuffer sb = new StringBuffer();

  for(char c : chars){
    s+= c;
    sb.append(c);
  }
  System.out.println(s);     //abc
  System.out.println(sb);    //abc
}
```


### 原子类型

原子类型是函数式语言中常用的数据类型，它就像是一个常用的、不变的字符，因为有些字符会被多次使用，对于这样的字符多次赋值给不同的字符串有些不必要，同时也存在内存的浪费。所以一些语言中加入了原子类型专门存储这种常用的字符串文本。

Ruby中用`:`修饰原子类型，比如:red就是文本值为red的原子类型（注意Ruby中原子类型其实叫做符号类型symbol）。
Erlang中用`''`单引号包裹表示原子类型，比如'red'就是文本值为red的原子类型（Erlang中原子类型用单引号包裹，字符串类型用双引号包裹）。

```ruby
#Ruby 字符串类型与原子类型比较

string_a = "string"
string_b = "string"
symbol_a = :string
symbol_b = :string

puts string_a.object_id    #70329489970500
puts string_b.object_id    #70329489970480
puts symbol_a.object_id    #158248
puts symbol_b.object_id    #158248
```

可以看出虽然都是相同的文本字符，但是用字符串类型会分别存储，而符号类型则会指向同一个存储地址，对于一个不变的文本信息用符号类型可以降低内存的开销，并提高程序性能。

另外symbol也常作为对象的key来使用，因为key是不允许改变的，而且同一模板下的对象key值也是相同的。
```ruby
#Ruby 用符号表示散列的键

prices = {
  :banana => 2,
  :apple  => 3,
  :pear   => 2.5
  #ruby中用=>绑定Hash键值对。
}
```

