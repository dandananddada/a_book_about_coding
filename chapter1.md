# 从数据类型聊编程语言的划分

通常说的程序由数据和算法两部分组成，而数据可以映射到数据类型，算法可以映射到编程范式，因此本书将从数据类型和编程范式这两方面来聊编程语言。

第一章主要对与数据类型相关的几个对立的概念进行说明，这几个概念也直接影响着编程语言本身的特性。所以第一章重点是以数据类型为切入点，说明编程语言间的对立与区别。


* 第一节，引入数据类型概念。
* 第二节，在第一节的基础上，针对数据类型转换机制不同引入数据类型强弱的概念。
* 第三节，在第一节的基础上，针对类型检查机制的不同引入数据类型动静的概念。
* 第四节，说明编译类型语言和解释类型语言的区别。
* 第五节，对常见的两大类数据类型：基础类型和引用类型进行概述说明。


### 1.1 类型

在谈语言设计上针对数据类型采取的不同策略之前，我们首先简单说明下什么是数据类型。

数据类型可以看作是内存对采用统一存储方式的数据而定义的一个统一称谓，简单的说就是数据是如何在内存中存储的。比如在Java中用4个字节来存储的数字就是整型的，如果同一个数字用8个字节来存储，那么它就是长整型的。而在JavaScript中存储这个数字必然要花费掉8个字节，因为JavaScript中数字类型就是8字节的。再简单点说，数据类型是用来区分不同种类的数据而定义的统一称谓，比如Java中整数用整型存储，小数用浮点型，字母用字符类型来存储，它们就是不同的数据类型。而在JavaScript中没有对小数和整数进行区分，所以在JavaScript中所有的数字被统一归类为一数字类型（number）。

另外一种区分数据类型的方法是，数据间能否直接运算，能够直接运算则定义为同一种数据类型，不能直接运算则定义为两种不同的数据类型。比如数字和数字是可以直接求和的，所以数字1和数字2都被划分为数字类型，而数字和字符则不能直接运算，所以数字1和字符string是不同的数据类型。

#### 常见数据类型

常用的数据类型有数字类型、字符类型、布尔类型、集合类型、散列类型、函数类型、自定义类型等。其中因语言又有所不同。比如Java把数字类型又划分为long number、short number、 decimal nmuber，C用char来表示字符类型，而后续大多数语言则提提供了string表示字符类型。另外有些语言也会定义一些不常见的数据类型，比如表示集合的range（range是一个范围值，通常作为循环迭代子来使用）。总之不同的语言会对数据类型进行不同精度的划分，不同的语言也可能会使用不同的别名表示同一数据类型。

好了，有了数据类型点概念就可以说明强弱类型和动静类型了。

### 1.2 强类型和弱类型

这里的强与弱是对类型检查的严格程度的一种描述，强类型语言中对类型转换的要求相对于弱类型语言来说要更加严格。我们通过对比几种语言的类型转换来说明强弱类型的区别，而不同数据类型之间的运算最能体现不同语言对类型转换处理方式的不同。

>下面的例子中会用到Java、JavaScript、C++和Ruby，它们分别是强类型、弱类型、弱类型、强类型。


#### 类型转换

在强类型语言中，不同类型的数据是无法直接运算的，而弱类型语言在处理这种运算时不会抛出异常，来看下面几个例子。
    
在Java中int类型不能和string类型直接运算，在加法运算中int类型会被隐式转换为string类型，然后执行`string + string`的字符串合并运算。而在减法运算中，因为字符串不存在减法运算，所以int类型不会隐式转换为string类型进行`string - string`运算，另外因为Java中string类型无法隐式转换为int类型，所以int与string的减法运算会在**编译时报错**。

```java
//Java 隐式转换下整型与字符型加减运算

int number = 1;
String string = "this is a string";
String stringNumber = "1";

System.out.println(number + string);        //1this is a string
System.out.println(number + stringNumber);  //11

System.out.println(number - string);
//error: bad operand types for binary operator '-'
```

接下来我们看下Java显示转换下会如何处理上述运算。

```java
//Java 显示转换下整型与字符型加减运算

int number = 1;
String string = "this is a string";
String stringNumber = "1";

System.out.println(number + Integer.parseInt(stringNumber));   //2
System.out.println(number -  Integer.parseInt(stringNumber));  //0

System.out.println(number - Integer.parseInt(string));
//Exception in thread "main" Java.lang.NumberFormatException: For input string: "this is a string"
```

可以看出当我们显式的把一个内容为数字的string转换为int时，会按照两个int类型的运算规则进行加减运算。当我们显式的把一个内容为非数字的string转换为int时会抛出一个**运行时异常**：字符串无法格式化为数字。


同样在Ruby中对string和number进行加法运算也会报运行时错误。

```ruby
#Ruby 整型与字符型加减运算

1 + "1"                 #String can't be coerced into Fixnum
1 + "this is a string"  #String can't be coerced into Fixnum
```

接下来我们看下弱类型的C++和JavaScript是如何处理这两种数据类型的运算的。

```c++
//C++ 整型与字符型加减运算

#include <iostream>
using namespace std;

int main()
{
  int number = 1;
  char character1 = '1';
  char characterA = 'A';

  cout << number + character1 << endl;    //50
  cout << number + characterA << endl;    //66
  cout << number - character1 << endl;    //-48
  
  return 0;
}
```

```javascript
//JavaScript 整型与字符型加减运算

var number = 1
,   string = "this is a string"
,   stringNumber = "1";

console.log(number + string);        //1this is a string
console.log(number + stringNumber);  //11 
console.log(number - string);        //NaN
console.log(number - stringNumber);  //0

```
可以看到，在C++中无论char是字母还是数字都会转换为表示这个字符的ASCII码（数字表示），然后与int进行运算。在JavaScript中，运算结果像是在Java的基础上进行了处理。`number + string`的运算方式同Java一样，而 `number - string` 则变成了数字减法运算，对无法转换为数字的字符串进行数学运算也不会返回错误，取而代之的是返回了**NaN**（Not a Number，是JavaScript内置的一个特殊值）。值得一提的是，在大多数语言中除零运算都会报错，比如在Java和Ruby中会报运行时错误，而C++中会在编译时报错，但是在JavaScript中除零会返回另外一个特殊值 Infinity（表示无限大的值）。

也许通过上面的几个例子，你会总结出：可以用`int - string`判断一个语言是强类型还是弱类型，这个看上去确实很取巧，但是事实总在意料之外，来看下面这段Clojure代码。
    
```clojure
;Clojure 整型与字符型加减运算

;Clojure是前置运算符，(+ 1 "1")即1 + "1"
(+ 1 "1")   ;"11"
(- 1 "1")   ;0
```
Clojure是强类型语言，但是可以看出其`number + string`返回的是string，而`number - string`返回的是int，所以判断一门语言是强类型还是弱类型，最好还是看看官方是如何介绍的吧。


究竟一个语言是强类型还是弱类型还是取决于语言设计者所做出的偏好，弱类型语言可以让你的代码更简单明了，但为此付出的代价就是你可能会因为一时的疏忽而导致一些难以发现的问题。比如JavaScript处理字符串与数字转换时完全可以避免繁琐的API，而只用`+`或`-`操作符。

```javascript
//JavaScript 弱类型语言，通过隐式转换简化代码

"1" - 0;    //将字符串转换为数字
1 + "0";    //将数字转换为字符串
```
如我之前所说的，这样的隐式转换会隐藏着一些小陷阱。

```javascript
"1" == 1;         //true
```
所以在一些 if 判断时你不得不用`===`操作符，`===`表示禁止类型转换，判断数据是否相等。
```javascript
"1" === 1;        //false
```

总之强弱类型决定着语言对类型检查的严格程度，因此影响着类型转换的灵活性，也就间接的影响着你能否避开繁琐的API，用更加简短的方式实现相同的操作。这看起来更像是你要在代码的灵活性和安全性上做出一种折中的选择，不过这个选择权取决于语言的设计者，而开发者只能依照设计者的意愿来使用。

好，强/弱类型就到此为止吧，接下来说一说动静类型，猜一猜动静类型将会在哪一方面做出折中，而这种折中又会影响到语言在哪方面的表现呢？

### 1.3 静态类型和动态类型

这里的动与静是针对变量在声明（declare）时是否绑定了具体的数据类型而说的，通常静态语言在声明时要指定一个具体的数据类型，而动态语言则不需要。静态类型在编译时确定具体数据类型，而动态类型是在运行时确定具体数据类型。

经常有人把动态类型和弱类型混在一起，他们觉得用var去声明一个变量而不指定具体类型，这样的语言就是动态的。当然不能说这样的解释是完全错误的，但是确实是不准确的。和对比强弱类型类似，我想通过变量声明和参数传递这两方面来对比动静类型的区别。

#### 变量声明

静态类型语言在变量声明时是要指定具体的数据类型，而动态类型语言中在变量声明时无法确定具体类型，只有在使用时才能明确。

```java
//Java 声明一个整型变量
int intVariable;        //这里很明显这个变量是个数字。   
```
```javascript
//JavaScript 声明一个变量
var unknowVariable;     //这里通过变量声明无法判断其具体类型。
```
在动态类型语言中你甚至可以不声明而直接使用一个变量。

```javascript
//JavaScript 动态语言声明变量无须显式指定类型

undeclaredVariable = "this variable is not declared";
//undeclaredVariable是一个未声明的变量，在JavaScript中这个变量会变为Global对象的一个属性
```

也许你想说我在用Java范型时也无法得知变量的具体类型，但不得不承认你所用的范型是你自己定义的一个类，实质上类本身就是数据类型。而在JavaScript中我们无法判断unknowVariable是一个值还是一个对象（这里的对象相当于Java中的类）。除此之外你可能还会拿出Scala说它的val和var也是无法区分类型的。但在Scala的代码中你随处都可以看见`a: Int`、`b: Double` 这样的变量声明，因为Scala确实是一个静态类型语言，而 val 和 var 只是 Scala 引入的类型推断机制，这一点可以在Scala用 val/var 声明变量时必须初始化看出来。
```scala
//Scala 在使用val声明变量时必须初始化，否则无法推断变量类型

val uninitializeVariable;       
//error: only classes can have declared but undefined memebers
```

#### 参数传递

因为动态类型语言对变量与数据类型绑定的要求要宽松，导致在函数传参时相对与静态类型语言要灵活的多。

```java
//Java 函数参数类型检查更加严格

public class StaticTyped{
    public void mustBeInt(int params){
        System.out.println("params is permited"); 
    }
    public void mustBeString(String params){
        System.out.println("params is permited"); 
    }
    public static void main(String []args){
        StaticTyped StaticTyped = new StaticTyped();
        String stringParams = "this params is a string";
        
        StaticTyped.mustBeInt(stringParams);
        //error: method mustBeInt in class StaticTyped cannot be applied to given types:  
        StaticTyped.mustBeString(stringParams);
        //params is permited
     }
}
```
```javascript
//JavaScript 函数参数类型检查更加宽松

function paramsIsDynamicTyped(params){
    if(typeof params === "number") console.log("params is number");
    if(typeof params === "string") console.log("params is string");
}

var number = 1
,   string = "this is a string";

paramsIsDynamicTyped(number);    //params is number
paramsIsDynamicTyped(string);    //params is string
```

动态类型提供了更加灵活的编程方式，你不必再为函数参数类型而困扰，你也可以在需要使用一个变量时省去变量声明。不过同介绍强弱类型时所描述的一样，你在获得更高的灵活性的同时，也要付出更多的代价保证代码的准确，因为一个很常见的拼写错误在动态类型下是无法察觉的，而静态类型语言却会给出一个明确的编译错误。正因为这样的特性，静态类型语言往往更容易提供一个友好的IDE。

在JavaScript一个拼写错误的变量不会抛出异常，相反会变为全局对象的一个属性。
```javascript
//JavaScript 弱类型语言难以对错误拼写给出警告

var zero;
zaro = 0;
console.log(window.zaro, zero);    //0 undefined
```
而在Java中因为变量必须声明才能使用，所以会抛出一个编译错误。

```java
//JavaScript 强类型语言中变量必须声明并绑定具体类型，所以可以提供拼写错误警告

int zero;
zero = 0;     //error: cannot find symbol
```

其实强弱类型和动静类型一样，都是在灵活性和安全性这个数轴上寻找一个折中点。动态类型使语言更加灵活，但更容易出错，强类型使语言更加安全，但代码累赘。虽然两者都是在灵活性与安全性上做出折中，但两者是不同的概念，也就是说静态类型语言可能是强类型的，也可能是弱类型的。

下表就是一个好的例子：

|               | 静态类型  | 动态类型       |
|--             | --        | --           |
| **强类型**    | Java      | Ruby          |
| **弱类型**    | C++       | JavaScript    |

最后补充一点，所谓的强弱并不是绝对的概念，比如C++可能是有一点弱类型的，所谓的“有一点”只是在强弱类型的数轴上更加偏向弱类型而已。

### 1.4 编译型与解释型

聊过了强弱类型和动静类型后，剩下一个容易混进来的概念就是编译型和解释型了。有些人会把编译类型和静态类型混淆，就像把弱类型和动态类型混淆一样，好了，在开始编译型与解释型的介绍之前你最好先问问自己弱类型和动态类型分别是怎么确定的？

编译型和解释型并不是语言设计上的概念，而是语言实现上的特性。所以我们说某个语言只能是编译型或者解释型是不准确的。因为任何语言都是可以是编译型的或者解释型的，这取决于你如何实现这个语言，而不是如何设计这门语言。

#### 编译型

那么什么算作是编译型语言呢，将程序转换为特定机器能运行的机器码，在这些特定的机器上可以直接运行的，就是编译型语言。编译型的好处是源码是私密的（因为你只需要提供机器码就可以运行程序了），因此相对要安全些。除此之外编译型语言相比解释型语言运行速度要更快。但是编译型语言是无法跨平台，也就是机器码会受到操作系统的限制，并不是任何系统都可以正确运行。另外因为你不知道它的源码是如何实现的，所以编译型语言难以调试（指针对于用于运行的机器码）。

>C、C++、Object-C都是编译型实现的语言。

#### 解释型

解释型语言不是由机器直接运行的，解释型语言会通过一个中间程序：解释器，来执行代码。解释器会将源代码一行一行的转换为当前机器能够执行的代码然后运行。因此解释型语言的源码是公开的，而且因为逐行转换的缘故其速度相比编译型要慢。但因为解释器的缘故解释型语言是可以跨平台的，同时公开的源码和逐行解释也使其更易于调试和测试。

>PHP、JavaScript是解释型语言。

#### 混合型

像在动静类型和强弱类型里所描述的一样，语言并非是完完全全按照这样或者那样的规格来的区分的。也就是说并非所有语言都必须是完完全全是编译型的或者解释型的。有一些语言会将源码编译为中间码（字节码），然后目标机器再通过解释器解释中间码来运行。这样做就中和了两种类型的优缺点。这也是为什么有人说Java是编译类型，有人却说它是解释型的。其实这样的语言只是折中了这两种方案罢了。所以一定要给它定义个实现类型，那就叫做混合型好了。

>Java、Python、Ruby都是混合型的。

### 1.5 基础类型和引用类型

抛开在1.1中列举的数据类型分类方式，这里我想从另外一种分类方式来说明：基础类型（有些语言喜欢称作值类型，本章为了对比引用类型说明而采用基本类型名称，后续章节将采用值类型命名）和引用类型。


#### 基础类型与引用类型

**基础类型**
* 基础类型存储的是直接值。
* 基础类型的值存储在栈中。
* 基础类型是不可修改的。
* 基础类型进行比较时，直接值相等就相等。
* 基础类型在作为参数传递时，传递的是值。
* 一般数字类型、字符串类型、布尔类型都是基础类型。

**引用类型**
* 引用类型存储的是地址。
* 引用类型的值存储在堆中。
* 引用类型是可修改的（可变的）。
* 引用类型进行比较时，值相等，且地址相等才相等。
* 引用类型作为参数传递时，传递的是引用。
* 一般数组类型、对象类型、函数类型都是引用类型。

接下来对上面的五点进行详细说明。

#### 值的存储

基础类型在栈中存储直接值，而引用类型在栈中存储引用的地址，在堆中存储实际值。

<img src = "/images/primitive_and_reference_type.png" alt = "" style = "width: 380px; float: right; margin-top: -20px">


对于编译型语言而言栈，在编译时处理内存的分配的，在运行时处理堆的内存分配。所以栈的内存分配是静态的，堆的内存分配是动态的。栈中存储的数据是固定大小的，而堆中的数据是可变大小的。因为编译型语言在程序编译时处理了基础类型的内存分配，所以相比解释型语言在运行时的处理速度要快。对于基础类型和引用类型的存储情况可以参考右图。


#### 是否可变

基础类型是不可变的，这里的不可变不是指变量的值不可变，而是存储在栈中指定地址的值是不允许改变的。我们通过Ruby来说明值不可变的含义（因为获取一个值的栈地址对于编程来说并没有实际用途，所以Java一类的语言本身并没有提供类似的方法，多亏Ruby是支持的）。
```ruby
#Ruby 获取基础类型栈地址

a = 5
a.object_id    #11

a = 3
a.object_id    #7

b = 5
b.object_id    #11
```

这里我们先对变量a赋值为5，然后输出a对应在栈中的地址11（转换为二进制是0b1011），然后我们修改a的值，再次输出a的地址，会发现地址改变了。然后我们对b赋值为5并输出其地址，会发现也是11，这说明地址为11的内存中存储的值就是5，这是不会改变的，变量值的改变只是栈中指向内存的地址发生了改变。

引用类型是可变的，根据上述描述，可以猜测到所谓可变是指存储引用类型的地址所存储的值是可以改变的。数组是引用类型的，我们修改数组中元素的值，然后输出数组的地址信息进行对比。

```ruby
Ruby 获取引用类型栈地址

a = [1, 2, 3]
a.object_id       #70324083600480
a[0].object_id    #3

a[0] = 0
a.object_id       #70324083600480
a[0].object_id    #1

```
上面代码可以看出数组的值改变后，其地址指向并没有改变（仍旧是70324083600480）。这说明引用类型在地址指向不变的情况下，其值是可变的。观察`a[0].object_id`的输出，会发现这个地址改变了。因为a[0]存储的是一个数字类型，而数字类型本身是个基础类型，基础类型的地址指向存储的值是不可修改的，所以a[0]值的改变是因为地址指向的改变导致的（a[0]指向的地址从3变为1，对应的值从1变为0）。

```ruby
#Ruby 重新赋值引用类型，引用地址会改变

a = [1, 2, 3]
a.object_id    #70324083600480

a = [0, 1, 2]
a.object_id    #70324083654580
```

这里如果我们尝试重新赋值数组，则地址引用就会改变，值得一提的是Java是不允许对数组重新赋值的，Java只允许在数组初始化的时候整体赋值一次。

#### 相等比较

判断基础类型是否相等，只需要保证值相等即可。而引用类型相等的条件是保证引用的对象是同一个，也就是说栈中存储的地址必须指向堆中同一个位置。

```javascript
//JavaScript 基础类型、引用类型比较
 
a = 1;
b = 1;
a === b;    //true

a = [1, 2, 3];
b = [1, 2, 3];
a === b;    //false
```

```java
//Java 基础类型、引用类型比较

int a = 1;
int b = 1;
System.out.print(a == b);    //true

int a[] = { 1, 2, 3 };
int b[] = { 1, 2, 3 };
System.out.print(a == b);    //false
```

```ruby 
#Ruby 基础类型、引用类型比较

a = 1
b = 1
a.equal? b    #true

a = [1, 2, 3]
b = [1, 2, 3]
a.equal? b    #false
```

这里单独说明下ruby用`==`判断两个对象的值是否相等，用`equal?`判断是否为同一引用。

```ruby
#Ruby ==和equal?的区别

a = {:type=>"Object"}
b = {:type=>"Object"}
c = a

a == b        #true
a == c        #true
a.equal? b    #false 
a.equal? c    #true
```

#### 参数传递

基础类型传递的是值，而引用类型传递的是引用。基础类型发生修改是无副作用的（不会修改原变量），引用类型的修改是有副作用的。
```javascript
//JavaScript 基础类型、引用类型参数传递

var a = 1;
var o = { b: 1 };
var fa = function(a){  a = 2;  }
var fo = function(o){  o.b = 2;  }
f(a);
f(o);

console.log(a);      //1
console.log(o.b);    //2
```
```java
//Java 基础类型、引用类型参数传递

Class Demo{
  public static void mi(int a){  a = 2;  }
  public static void mr(int[] a){  a[0] = 2;  }
  public static void main(String args[]){
    int a = 1;
    int[] aa = { 1, 2, 3 };
    Demo.mi(a);
    Demo.mr(aa);

    System.out.print(a);        //1
    System.out.print(aa[0]);    //2
  }
}
```
对于变量拷贝也是如此，基础类型拷贝的是值，拷贝发生改变是不会影响到源的，而引用类型拷贝的是引用，拷贝的改变会破坏原始数据。

```javascript
//JavaScript 基础类型、引用类型拷贝

var a = 1
,   b = a
,   oa = { a: 1 }
,   ob = oa;
b    = 2;
ob.a = 2;

console.log(a);       //1
console.log(oa.a);    //2
```

```java
//Java 基础类型、引用类型参数传递

int a = 1, b = a;
int aa[] = { 1, 2, 3 }, ab[] = aa;
b = 2;
ab[0] = 2;

System.out.print(a);        //1
System.out.print(aa[0]);    //2
```

#### 基础类型和栈内存分配

字符类型是基础类型，所以在做字符串修改、合并、截取等操作时，字符串的每次改变都会在栈中分配一块新的内存记录新的值。

```ruby
#Ruby 字符串修改会从栈分配新内存

a = [ "A", "B", "C"]
s = "Array a contains: "
a.each do |i|
  s += i  
  puts "address of s is #{s.object_id}"
end
puts s
# output
# address of s is 70123772400880
# address of s is 70123772400780
# address of s is 70123772400700
# Array a contains: ABC
```

可以看出，如果上面的集合对象中存在大量的元素，这时对栈内存会造成极大的开销，对于这种需要多次修改的字符串，Java提供了StringBuffer类，Ruby提供了StringIO类，我们仍旧以Ruby为例来演示StringIO的效果。

```ruby
#Ruby StringIO类降低字符串修改的内存开销
require 'stringio'
a = ["a", "b", "c"]
s = StringIO.new
s << "Array a contains: "
a.each do |i|
  s << i
  puts "address of s is #{s.object_id}"
end
puts s.string
# output
# address of s is 70280643727180
# address of s is 70280643727180
# address of s is 70280643727180
# Array a contains: abc
```

可以看出这时s指向的是同一块地址，这样就可以避免频繁修改同一字符串变量造成栈内存大量开销的问题了。

#### 引用类型和深度拷贝

有时候我们在使用一个引用类型时希望操作后能保证原引用是不可破坏的（值不会被修改），这个时候我们就要用到拷贝，拷贝分为浅拷贝和深度拷贝，简单的说浅拷贝是只拷贝引用类型的第一层（假设一个对象的一个属性b也是对象，那么浅拷贝的对象产生修改时是无法保证b对象的属性不被破坏的），而深度拷贝会逐层拷贝这个引用类型的所有引用类型的属性。

可惜的是无论是Java还是JavaScript都没有提供深度拷贝的API，所有只能自己编写或者使用第三方库来实现深度拷贝。

当然最常用的实现思路是遍历这个对象的每一个属性，如果是引用类型则继续遍历这个引用，如果是基础类型就拷贝。另外一种相对简单但不稳定的思路是将对象序列化为JSON串，然后拷贝这个字符串，再反序列化为对象。我们采用第二个思路来看如下例子。

```javascript
//JavaScript 序列化方式实现深度拷贝

function deepClone(obj) {
  return JSON.parse(JSON.stringify(obj));
}
var o = {
  a: { b: 1, c: 2 },
  d: 3
}
var b = deepClone(o);

console.log(b);    //{ a: { b: 1, c: 2 }, d: 3 }

b.a = 5;
console.log(b);    //{ a: 5, d: 3 }
console.log(o);    //{ a: { b: 1, c: 2 }, d: 3 }
```
可以看出深度拷贝的对象重新赋值并不会影响到原引用的值。