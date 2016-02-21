## 1.3 静态类型和动态类型

这里的动与静是针对变量在声明（declare）时是否绑定了指定的数据类型的一种描述，通常静态语言在声明时要指定一个具体的数据类型，而动态语言则不需要。

通常的说法是静态类型是指在编译时确定数据类型的语言，而动态类型是在运行时确定数据类型的。

经常有人把动态类型和弱类型混在一起，他们觉得用`var`去声明一个变量不去指定具体的类型，那么这样的语言就是动态的，当然不能说这样的解释是完全错误的，但是确实是不准确的。

**1.在变量声明时**

静态类型语言在变量声明时是知道其数据类型的，而动态类型语言中变量在声明时是无法确定具体类型的，只有在使用时才能明确。

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

也许你想说我在用Java的范型时也无法得知变量具体的类型，但不得不承认你所用的范型是你自己定义的一个类这一点你是知道的。而在上面的JavaScript中我们甚至无法区别`unknowVariable`是一个内置的变量类型还是一个自定义对象的实例。

除此之外你可能还会拿出Scala说它的`val`和`var`也是无法区分类型的，但是在Scala的代码中你随处都可以看见`a: Int`、`b: Double`这样的变量定义，因为Scala确实是一个型语言。ß而`val`和`var`只是Scala引入了类型推断机制，这一点可以在Scala变量不支持只声明不初始化看出来。
```scala
val uninitializeVariable;       
//=> error: only classes can have declared but undefined memebers
```

同时因为动态类型语言对变量与数据类型绑定的宽松要求，导致在函数传参时相对与静态类型语言也要灵活的多。

```java
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
        //=> error: method mustBeInt in class StaticTyped cannot be applied to given types:
        
        StaticTyped.mustBeString(stringParams);
        //=> params is permited
     }
}
```
```javascript
function paramsIsDynamicTyped(params){
    if(typeof params === "number") console.log("params is number");
    if(typeof params === "string") console.log("params is string");
}
var number = 1
,   string = "this is a string";

paramsIsDynamicTyped(number);
//=> params is number

paramsIsDynamicTyped(string);
//=> params is string
```

动态类型提供了更加灵活的编程方式，你不必因为函数参数类型而困扰，你也可以在需要使用一个变量时省去变量声明。不过同介绍强/弱类型时所描述的一样，你在获得更高的灵活性的同时，自己也要付出更多的代价保证代码的准确，因为一个很常见的拼写错误在动态类型下是无法察觉的，而静态类型语言却会给出一个明确的编译错误。因为这样的特性，静态类型语言往往更容易提供一个有好的IDE。

在JavaScript一个拼写错误的变量初始化会变为全局对象的一个属性。
```javascript
var zero;
zaro = 0;
console.log(window.zaro, zero);     //=> 0 undefined
```
而在Java中因为变量要求必须声明，所以会抛出一个编译错误。

```java
int zero;
zaro = 0;           //=> error: cannot find symbol
```
其实这两个纬度的类型定义，都是在灵活性和避免小陷阱这个数轴上寻找一个折中点。作为开发人员你只要记住这是两个完全不同的概念就行了，也就是静态类型语言可能是强类型的，也可能是弱类型的。

下表就是一个好的例子：

|               | 静态类型  | 动态类型      |
|--             | --        | --            |
| **强类型**    | Java      | Ruby          |
| **弱类型**    | C++       | JavaScript    |

最好要说明的一点是，这里的强弱也不是绝对的一个值，比如C++可能只是有一点弱类型，总之这个`有一点`只是在强弱类型的数轴上可能它更加向弱类型偏一点，所以就被划分到弱类型了。
