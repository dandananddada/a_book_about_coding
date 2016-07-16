# 函数式编程

函数式编程是通过数学函数求值来处理程序计算，它代码简洁易懂（指接近自然语言，易于理解），开发效率（注意是开发效率，不是运行）。

####特点

他有如下特点：

1. 函数为一等公民
2. 声明式
3. 值不可变
4. 引用透明

**函数为一等公民**

一等公民（first class）简单的说就是可以当做值来使用的，比如用于对变量初始化，作为函数、方法的返回结果。函数作为一等公民就是说函数可以当做参数传递，当做返回值引用。这就意味着代码段及代码段内包含的变量也是可以传递的。

```haskell
map (+3) [1,2,3]
```
如上`map`是一个函数，`(+3)`也是一个函数（而且这个函数中包含了一个变量3用作操作数），`[1,2,3]`是一个数据参数，这里就是把函数`(+3)`当做参数传递给了另一个函数`map`。

**声明式**

函数式编程是一种声明式编程范式(declarative programming paradigm)，声明式一般通过表达式计算、函数声明来代替命令式编程中的逻辑控制。

```haskell
isZero:: Int->String
isZero 0 = "yes"
isZero _ = "no"

isZero 3    --=>no
```
如上声明了一个函数，`isZero`，`Int->String`表示接受一个整数类型为参数，返回一个字符串类型。然后声明了`isZero 0 = "yes"`表示当参数为`0`时返回`yes`，及`isZero _ = "no"`表示其余任意参数都返回`no`。

`isZero`会按照声明的顺序对输入参数进行判断，先判断`isZero 0`，满足则返回`yes`，不满足执行下一个模式，一般最后一个模式都用`_`匹配任意值，相当于`if else`中的`else`。

看下与之对应的命令式如何表述`isZero`。

```java
public static String isZero(int num){
  if(num == 0)  return "yes";
  else          return "no";
}

System.out.print(isZero(0));    //=>yse
```

**值不可变**

函数式编程是没有副作用（side effect）的，函数式要求值是不可修改的，状态是不可改变的，对于输入参数进行函数运算不会破坏源数据，会生成一个新的相似（因为运算会改变数据的值）的数据作为运算结果返回。

函数运算也只依赖于输入参数，不会改变函数外部数据的值或状态，面向对象语言通过封装解决状态改变的问题，而函数式语言则通过函数变化尽量减少状态的改变。


**引用透明**

引用透明（Referential transparency）是指函数的运算不会受到外部变量的影响，其计算结果只依赖于输入参数，输入参数相同则返回结果相同。

在报错函数式语言纯度（purely）和引用透明的情况下有如下优势：

1. 如果当前函数表达式返回值不需要了，可以直接删除，不会影响到函数表达式以外的数据。
2. 多次调用一个纯函数输入参数相同，返回结果相同，不会有副作用。
3. 替换两个纯函数表达式的执行顺序不会影响执行结果，因此适合做并行开发。
4. 无副作用的表达式运算允许编译器自由结合表达式的运算结果。
5. 

####对比命令式编程、面向对象编程

1. 命令式编程是按照程序是一系列改变状态的命令来建模的一种编程风格，函数式编程将程序描述为表达式和变换以数学方程的形式建立模型。
2. 命令式编程一次循环完成多个任务，更注重性能。
3. 面向对象编程中通过封装不确定因素使代码更容易被人理解，而函数式编程通过尽量减少不确定因素来使代码容易被人理解。
4. 面向对象提倡真对具体的问题建立专门的数据结构及操作，函数式则提倡使用几种基本的数据结构（数组、列表）及针对这几种结构高度优化的操作来完成程序任务。
5. 函数式编程讲程序描述为表达式和变换，像数学方程一样建立模型同时需要尽量的减少可变状态。开发者需要用高阶函数来调整低层运转机构的运作。开发者关注的是更高层的抽象，而非底层的实现细节。
6. 函数式编程能在更细小的层面上重用代码。

###完美数的例子：

完美数是指除了自身以外的所有他的正约数只和等于本身的数字，比如6(1+2+3=6,其中6的非自身的正约数有1，2，3)。

我们来用完美数的例子对比下函数式和命令式两种编程范式。

先整理下如何判断一个数是否为完美数。

1. 取1到这个数之间的所有数。
2. 把这些数中能被它整除的筛选出来。
2. 对筛选出来的数字求和。
3. 判断是否与这个数自身相等。

这里我们用JavaScript来演示：

```
//命令式
(function(){
  var factors = []
  ,   number
  ,   sum = 0
  ,   result;

  number = 496;

  getFactors();  
  aliquoSum();
  result = isPerfect();

  if(result)   console.log(number + " is perfcet");
  else                  console.log(number + " is not perfect");

  function getFactors(){
    for(i = 1;  i < number; i = i+1){
       if(isFactor(i))  factors.push(i); 
    }
  }
  
  function isFactor(pontential){
    return number % pontential === 0;
  }

  function aliquoSum(){
    for(i = 0; i < factors.length; i = i+1){
      sum = sum + factors[i];
    }
  }

  function isPerfect(){
    return sum === number;
  }

})()
```

可以看到我们先用`getFactors`方法取得1到当前数之间所有数字然后通过`isFactor`方法找出可以被整除的数记录到`factor`数组。最后把数组中所有元素相加，判断与这个数字是否相等，如果相等说明是完美数。

好，接下来我们看看函数式是如何实现的（这里引用了JavaScript的函数式库Underscore.js）。

```javascript
//函数式
var _ = require("underscore")
,   number = 496;

if(
  _.isEqual(number,(
    _.reduce(
      _.filter(
        _.range(1, number)
        , function(n){ return number%n == 0 })
      , function(p,n){ return p+n }
      , 0)
    )
  )
){
  console.log(number + " is not perfect");
}
```

