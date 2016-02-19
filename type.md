#### 类型

在谈语言设计上针对类型采取的不同策略之前，我们首先简单说明下什么是类型。

类型可以看作是内存对采用统一存储方式的数据而定义的一个统一的称谓。

比如在Java中我们把用4字节来存储一个数字那么这个数字就是整型（int）的，而我们用8个字节来存储这个数字，那么它就是长整型（long）的。而在JavaScript中我们用来存储这个数字必然要花费掉8个字节，因为在JavaScript中存储数字所用的Number类型就是8字节的。

当然也可以更简单的说，类型是用来区分不同种类的数据而为其定义的统一的称谓。

比如Java中整型数据的数值范围要比长整型小，字符串存储的是字符，它们都可以看作是不同的数据类型。而在JavaScript中数字类型就是Number甚至没有对小数和整数进行区分，所以这个时候所有的数字被统一看作为一种数据类型。

另外一种区分类型的说法是，类型是用来统一划分一些数据是否可以直接进行运算的。

比如数字和数字是可以直接求和运算的，因为数字1和数字2都被划分为数字类型，而数字和字符是不能直接相加的。

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


接下来我们看下弱类型的c++和JavaScript是如何处理两种不同数据类型的运算的。

















