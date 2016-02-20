#### 类型

在谈语言设计上针对类型采取的不同策略之前，我们首先简单说明下什么是类型。

类型可以看作是内存对采用统一存储方式的数据而定义的一个统一的称谓。

比如在Java中我们把用4字节来存储一个数字那么这个数字就是整型（int）的，而我们用8个字节来存储这个数字，那么它就是长整型（long）的。而在JavaScript中我们用来存储这个数字必然要花费掉8个字节，因为在JavaScript中存储数字所用的Number类型就是8字节的。

当然也可以更简单的说，类型是用来区分不同种类的数据而为其定义的统一的称谓。

比如Java中整型数据的数值范围要比长整型小，字符串存储的是字符，它们都可以看作是不同的数据类型。而在JavaScript中数字类型就是Number甚至没有对小数和整数进行区分，所以这个时候所有的数字被统一看作为一种数据类型。

另外一种区分类型的说法是，类型是用来统一划分一些数据是否可以直接进行运算的。

比如数字和数字是可以直接求和运算的，因为数字1和数字2都被划分为数字类型，而数字和字符是不能直接相加的。

常用的数据类型有数字类型、字符类型、布尔类型、集合类型、自定义类型等。其中数字类型下Java有划分为Long Number、Short Number、 Decimal Nmuber；同理字符类型有如C语言的Char、JavaScript的String、Ruby的Symbol（这个其实更接近于原子类型，类似的像Erlang的Atom，这个本质可以看做为一个不需要变量名的字符串，所以就不细分了）；集合类型有Array、List、Hash；以及自定义类型的Class、Object、Function等。

关于集合类型主要有三种：同类型元素构成的集合、不同类型元素构成的集合以及键值对构成的集合，可能不同的语言下会有不同的划分和命名比如Ruby的Array是不区分元素类型的，而Java的Array是区分的。Ruby中的键值对叫做Hash，而JavaScript中是用Object代替。另外还有一中叫做Range的集合元素，用来表示一个范围的数值，有些语言中会用它作为循环迭代子。

总之不同的语言会对数据类型进行不同精度的划分，不同的语言可能也会使用不同的别名，但是其本质都是类似的。可能你在面向对象范型下更多的用自定义类型的Class或者Object，而函数式下更多的就是集合类型了，甚至也有基于模式匹配的语言甚至不需要定义变量的概念，这些都是后话了。

有了数据类型点概念就可以说明强／弱类型和动／静类型了。


####强类型和弱类型

这里的强与弱是对数据类型灵活程度对一个形容，我会通过以下几个方面来说明其差异。

**1.在变量声明时**

强类型语言中的变量时是知道其数据类型的，而弱类型语言中变量在声明时你无法知道其具体类型，只有在使用时才能明确。

```java
int intVariable;        //这里很明显这个变量是个数字。   
```
```javascript
var unknowVariable;     //这里通过变量声明无法判断其具体类型。
```
也许你想说我在用Java的范型时也无法得知变量具体的类型，但不得不承认你所用的范型是你自己定义的一个类这一点你是知道的。而在上面的JavaScript中我们甚至无法区别`unknowVariable`是一个内置的变量类型还是一个自定义对象的实例。

除此之外你可能还会拿出Scala说它的`val`和`var`也是无法区分类型的，但是在Scala的代码中你随处都可以看见`a: Int`、`b: Double`这样的变量定义，因为Scala确实是一个强类型语言，而`val`和`var`只是在使用时进行了必要的类型转换，同时Scala本身是不支持变量声明不初始化的，所以代码中的变量初始化其实已经告诉你`val`究竟是一个什么类型了。
```scala
val uninitializeVariable;       
//=> error: only classes can have declared but undefined memebers
```

**2.在作为函数参数时**

强类型语言中要求传入函数的实参必须与函数定义时的形参类型一致。而弱类型语言中要灵活的多。

```java
public class StrongTyped{
    public void mustBeInt(int params){
        System.out.println("params is permited"); 
    }
    public void mustBeString(String params){
        System.out.println("params is permited"); 
    }
     public static void main(String []args){
        StrongTyped strongTyped = new StrongTyped();
        String stringParams = "this params is a string";
        
        strongTyped.mustBeInt(stringParams);
        //=> error: method mustBeInt in class StrongTyped cannot be applied to given types:
        
        strongTyped.mustBeString(stringParams);
        //=> params is permited
     }
}
```
```javascript
function paramsIsWeaktyped(params){
    if(typeof params === "number") console.log("params is number");
    if(typeof params === "string") console.log("params is string");
}
var number = 1
,   string = "this is a string";

paramsIsWeaktyped(number);
//=> params is number

paramsIsWeaktyped(string);
//=> params is string
```

**3.变量运算时**

这是最容易拿来说明强弱类型语言的差异的。在强类型语言中，不同类型的变量是无法直接运算的，而弱类型语言在处理这种运算时不会抛出异常，来看下面几个例子。

在Java中`int`类型不能和`String`类型直接运算，在加法运算中`int`类型会被隐式转换为`String`，然后进行`String + String`的字符串合并运算。

而在减法运算中，首先字符串不存在减法运算，所以`int`类型无法隐式转换为`String`类型进行`String - String`运算，其次`String`类型无法隐式转换为`int`类型，所以`int`与`String`的减法运算在**编译时**会报错。

```java
int number = 1;
String string = "this is a string";
String stringNumber = "1";

System.out.println(number + string);
//=> 1this is a string

System.out.println(number + stringNumber);
//=> 11

System.out.println(number - string);
//=> error: bad operand types for binary operator '-'
```
接下来我们看下Java中显示转换下会如何。

```java
int number = 1;
String string = "this is a string";
String stringNumber = "1";

System.out.println(number + Integer.parseInt(stringNumber));
//=> 2

System.out.println(number -  Integer.parseInt(stringNumber));
//=> 0

System.out.println(number - Integer.parseInt(string));
//=> Exception in thread "main" Java.lang.NumberFormatException: For input string: "this is a string"
```

可以看出当我们显示的把一个内容为数字的`String`转换为`int`时，会按照两个`int`类型的规则进行加减运算。

而当我们显示的把一个内容为非数字的`String`转换为`int`时会抛出一个**运行时**异常：字符串无法格式化为数字。

另外如果你在Ruby中对`String`和`Number`进行加法运算的话，也会报运行时错误。

```Ruby
1 + "1"
//=> String can't be coerced into Fixnum

1 + "this is a string"
//=> String can't be coerced into Fixnum

```

接下来我们看下弱类型的C++和JavaScript是如何处理两种不同数据类型的运算的。

```c++
#include <iostream>

using namespace std;

int main()
{
    int number = 1;
    char character1 = '1';
    char characterA = 'A';
    
    cout << number + character1 << endl;    //=> 50
    cout << number + characterA << endl;    //=> 66
    cout << number - character1 << endl;    //=> -48
   return 0;
}
```
可以看到无论`char`是字母还是数字在C++中都会转换为表示这个字符的ASCII码，然后与`int`进行运算。

接下来看下JavaScript中是如何的。

```
var number = 1
,   string = "this is a string"
,   stringNumber = "1";

console.log(number + string);       //=> 1this is a string

console.log(number + stringNumber); //=> 11 

console.log(number - string);       //=> NaN

console.log(number - stringNumber); //=> 0

```
可以看出JavaScript的运算结果是在Java的基础上有所变化，`number` + `string` 仍旧是做字符串合并运算，而`number` - `string`则变成了数字减法运算，而对无法转换为数字的字符串进行数学运算也不会返回错误，取而代之的是返回`NaN`（Not a Number，是JavaScript内置的一个特殊值）。

值得一提的是，在大多数语言中除零运算都会报错，比如Java和Ruby中会报运行错误，而C++中会报编译错误，但是在JavaScript中除零会返回另外一个特殊值`Infinity`（表示无限大的值）。
















