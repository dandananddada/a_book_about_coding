## 1.3 静态类型和动态类型

这里的动与静是针对变量在声明（declare）时是否绑定了指定的数据类型而说的，通常静态语言在声明时要指定一个具体的数据类型，而动态语言则不需要。

静态类型在编译时确定具体数据类型，而动态类型是在运行时确定具体数据类型。

经常有人把动态类型和弱类型混在一起，他们觉得用 var 去声明一个变量不去指定具体的类型，那么这样的语言就是动态的，当然不能说这样的解释是完全错误的，但是确实是不准确的。

和对比强弱类型类似，我想通过变量声明和参数传递来说明动静类型的区别。

#### 变量声明

静态类型语言在变量声明时是要指定数据类型的，而动态类型语言中变量在声明时是无法确定具体类型，只有在使用时才能明确。

```java
int intVariable;        //这里很明显这个变量是个数字。   
```
```javascript
var unknowVariable;     //这里通过变量声明无法判断其具体类型。
```
在动态类型语言中你甚至可以不声明而直接使用一个变量。

```javascript
undeclaredVariable = "this variable is not declared";
//undeclaredVariable是一个未声明的变量，在JavaScript中这个变量会变为Global对象的一个属性。
```

也许你想说我在用Java范型时也无法得知变量的具体类型，但不得不承认你所用的范型是你自己定义的一个类，实质上类本身就是数据类型。而在JavaScript中我们无法判断`unknowVariable`是一个值还是一个对象。

除此之外你可能还会拿出Scala说它的val和var也是无法区分类型的，但在Scala的代码中你随处都可以看见 a: Int、b: Double 这样的变量声明，因为Scala确实是一个静态类型语言。而 val 和 var 只是Scala引入的类型推断机制，这一点可以在Scala用val或者var声明变量必须初始化看出来。
```scala
//Scala 在使用val声明变量时必须初始化，否则无法推断变量类型

val uninitializeVariable;       
//error: only classes can have declared but undefined memebers
```

#### 参数传递

因为动态类型语言对变量与数据类型绑定要求宽松，导致在函数传参时相对与静态类型语言也要灵活的多。

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
//JavaScript 函数参数类型检查宽松

function paramsIsDynamicTyped(params){
    if(typeof params === "number") console.log("params is number");
    if(typeof params === "string") console.log("params is string");
}

var number = 1
,   string = "this is a string";

paramsIsDynamicTyped(number);
//params is number

paramsIsDynamicTyped(string);
//params is string
```

动态类型提供了更加灵活的编程方式，你不必因为函数参数类型而困扰，你也可以在需要使用一个变量时省去变量声明。不过同介绍强弱类型时所描述的一样，你在获得更高的灵活性的同时，也要付出更多的代价保证代码的准确，因为一个很常见的拼写错误在动态类型下是无法察觉的，而静态类型语言却会给出一个明确的编译错误。正因为这样的特性，静态类型语言往往更容易提供一个友好的IDE。

在JavaScript一个拼写错误的变量初始化会变为全局对象的一个属性。
```javascript
//JavaScript 弱类型语言难以对错误拼写给出警告

var zero;
zaro = 0;

console.log(window.zaro, zero);     
//0 undefined
```
而在Java中因为变量必须声明才能使用，所以会抛出一个编译错误。

```java
//JavaScript 强类型语言变量必须声明并绑定具体类型，所以可以提供拼写错误警告

int zero;

zaro = 0;           
//error: cannot find symbol
```

其实强弱类型和动静类型一样，都是在灵活性和安全性这个数轴上寻找一个折中点。动态类型使语言更加灵活，但更容易出错。与此相对的，强类型使语言更加安全，但代码累赘。虽然两者都是在灵活性与安全性上做出折中，但两者是不同的概念，也就是说静态类型语言可能是强类型的，也可能是弱类型的。

下表就是一个好的例子：

|               | 静态类型  | 动态类型      |
|--             | --        | --            |
| **强类型**    | Java      | Ruby          |
| **弱类型**    | C++       | JavaScript    |

最后补充一点，所谓的强弱并不是绝对的概念，比如C++可能是有一点弱类型的，所谓的“有一点”只是在强弱类型的数轴上更加偏向弱类型而已。
