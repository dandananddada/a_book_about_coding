# 第六章 声明式下的函数式概念

第四章引入命令式和声明式概念后，在本章将主要说明声明式范式下的函数式语言。

* 介绍函数式语言的特点。
* 介绍高阶函数及高阶函数相关概念。
* 介绍函数式语言其他概念。

### 6.1 函数式编程

函数式编程通过数学函数求值来处理程序计算，它具有代码简洁易读，更接近于自然语言，开发效率高等优点。

#### 函数式特点

他有如下特点：

* 函数为一等公民。
* 声明式。
* 值不可变。
* 引用透明。

**函数为一等公民**

一等公民（first class）是指可以像基本的值一样来使用、通信的那些数据或代码，简单的说就是可以直接作为值赋值给变量，或者由函数返回。这里函数作为一等公民就是说函数可以像参数一样传递给另一个函数，或者作为另一个函数的返回值。因为函数是一段代码，所以函数作为一等公民就意味着代码段及代码段内包含的变量也是可以传递的。

```haskell
--Haskell 函数作为参数传递给另一个函数
map (+3) [1,2,3]
```

如上map是一个函数，(+3)也是一个函数（这个函数传递了变量3作为加法的右操作数），这里把函数(+3)当做参数传递给了另一个函数map（[1,2,3]是map函数的另一个参数）。

**声明式**

函数式编程是一种声明式编程范式，声明式通过表达式计算、函数声明来代替命令式编程中的逻辑控制。

```haskell
--Haskell 函数定义

isZero:: Int->String
isZero 0 = "yes"
isZero _ = "no"

isZero 3    --no
```

如上声明了一个函数isZero，其中Int->String表示接受一个整数类型，返回一个字符串类型。然后声明了`isZero 0 = "yes"`表示当参数为0时返回yes。最后`isZero _ = "no"`表示除此之外任意参数都返回no。

isZero会按照声明的顺序对输入参数进行判断，先判断输入参数是否为0，如果为0则返回yes，否则执行下一个模式，一般最后一个模式都用`_`匹配任意值，相当于if else中的else。对比声明式编程看下命令式如何实现isZero。

```java
//Java 命令式编程

public static String isZero(int num){
  if(num == 0)  return "yes";
  else          return "no";
}

System.out.print(isZero(0));    //yes
```

**值不可变**

* 函数式编程是没有副作用的。
* 函数式要求值是不可修改的，状态也是不可改变的。
* 对输入参数进行函数运算不允许修改源数据，相应的会生成一个新的拷贝作为运算结果。
* 函数运算只依赖于输入参数，不会改变函数外部数据的值或状态。
* 面向对象语言通过封装解决状态改变的问题，而函数式语言通过函数变换避免状态改变。

**引用透明**

引用透明是指函数的运算不会受到外部变量的影响，其计算结果只依赖于输入参数，输入参数相同则返回结果相同。引用透明的特性使函数式语言使其具有如下优势：

* 如果当前函数表达式返回值不再需要，可以直接删除，不会对函数表达式外数据产生影响。
* 多次调用一个纯函数输入相同参数，返回结果相同，且不存在副作用。
* 替换两个纯函数表达式的执行顺序不会影响执行结果，因此适合做并行开发。
* 无副作用的表达式运算允许编译器自由结合表达式的运算结果，可以将多个函数组合使用。

#### 对比命令式编程、面向对象编程

* 命令式编程是按照程序是一系列改变状态的命令来建模的一种编程风格，函数式编程则将程序描述为表达式和变换，以数学方程的形式建立模型。
* 命令式编程一次循环完成多个任务，更注重性能，而函数式一个任务可能需要多次循环，更注重语义。
* 命令式编程是对数据进行封装，而函数式则对行为进行封装。
* 命令式编程中通过封装不确定因素使代码更容易被人理解，而函数式编程通过尽量减少不确定因素来使代码容易被人理解。
* 命令式编程提倡对具体的问题建立专门的数据结构及操作，而函数式则提倡使用几种基本的数据结构（数组、列表）并组合针对这几种结构高度优化的操作来完成程序任务。
* 函数式编程将程序描述为表达式和变换，像数学方程一样建立模型。更注重高层的抽象，而非底层的细节，开发者需要用高阶函数调整底层运转。
* 函数式编程能在更细小的层面上重用代码。

##### 完美数例子

上面像说明书一样列举了函数式的特点，并对比了与命令式的区别。但这些终究是教科书一样的文案，没有实际的例子是难以认清函数式编程的真面目的，下面我们通过完美数例子来观察命令式编程与函数式编程的区别。

>完美数是指除了自身以外的所有他的正约数之和等于其本身的数字，比如6(1+2+3=6,其中6的非自身正约数有1，2，3)。

在给出代码之前，先说明下如何判断一个数是否为完美数。

1. 取1到这个数之间的所有数。
2. 把这些数中能整除它的数筛选出来。
3. 对筛选出来的数字求和。
4. 判断是否与这个数自身相等。

这里我们用JavaScript实现完美数例子：

```javascript
//JavaScript 命令式实现完美数

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
  else         console.log(number + " is not perfect");
  
  //取得所有正公约数
  function getFactors(){
    for(i = 1;  i < number; i = i+1){
       if(isFactor(i))  factors.push(i); 
    }
  }
  
  //判断是否为公约数
  function isFactor(pontential){
    return number % pontential === 0;
  }
  
  //对所有正公约数求和
  function aliquoSum(){
    for(i = 0; i < factors.length; i = i+1){
      sum = sum + factors[i];
    }
  }

  //判断是否为完美数
  function isPerfect(){
    return sum === number;
  }

})();    
//496 is perfect
```

可以看到我们先用for循环法取得1到当前数之间所有数字，然后通过isFactor方法找出可以整除的数记录到factors数组。最后把数组中所有元素相加，判断是否与这个数字相等，如果相等则说明这个数字是完美数。接下来我们看看函数式是如何实现完美数的（这里引用了JavaScript的函数式库underscore.js）。

```javascript
//JavaScript 函数式编程实现完美数

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
)
  console.log(number + " is perfect");    //496 is perfect
```
我们从最里层的调用开始看：\_\.range(1, number)会帮我们生成一个从1到number的数组，然后我们把这个数组作为参数应用到filter函数，这个函数会把表达式`number%n == 0`为真的数字返回，也就是正公约数。然后我们把这些过滤后得到的数字传递给reduce函数，并通过匿名函数`function(p,n){ return p+n }`求和，最后将求和得到的结果传入isEqual函数，判断是否与number相等。

这里可以看出函数式编程通常将数据作为输入，通过一个接一个的函数进行映射、过滤、折叠处理，最终返回出一个计算结果。对比命令式编程，函数式代码更加简洁清晰，容易阅读。

针对上述代码，我们还可以通过链式调用进行优化，使代码更加简洁清晰。

```javascript
//JavaScript 通过链式调用使函数式代码更加语义化

var number = 496
,   isPerfect = _.chain(_.range(1, number))
    .filter(function(n){ return number%n == 0 })
    .reduce(function(p,n){ return p+n }, 0)
    .isEqual(number)
    .value();
if(isPerfect)  console.log(number, "is perfect");    //496 is perfect
```
通过\_\.chain函数将数组打包到管道后就可以用链式调用对数据进行操作了，在操作的最后通过value函数执行计算并返回最终结果。

### 6.2 高阶函数

在函数类型介绍时已经提到过高阶函数的概念，现在来详细介绍下高阶函数。

当一个函数的参数是函数或者返回值是一个函数时，这个函数就是高阶函数，也就是说高阶函数必须满足以下两个条件之一：

* 传入参数为函数类型。
* 返回值为函数类型。

####工具类高阶函数

在介绍其他和高阶函数相关的概念之前，我们先看下针对列表操作而设计的三个最基础的高阶函数。

##### 映射（map）

map函数会接受一个集合类型参数（比如列表、数组、链表）和一个函数类型参数，然后将集合中的每一个元素应用到函数，并把运算结果组装成一个新的集合返回。我们来看下下面这个例子：

```java
//Java 把集合中每个元素值加三

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3);
        List<Integer> mappedNumbers = numbers
                                        .stream()
                                        .map(i -> i + 3)
                                        .collect(Collectors.toList());
        System.out.println(mappedNumbers);    //[4, 5, 6]
    }
}
```

Java8中加入了stream库来提供对函数式编程的支持，通过stream库可以使用流畅的API来操作数据（类似于underscore中的chain）。

简单说明下上面的代码：

1. 首先声明一个数组numbers，并通过stream方法打包成提供集合操作的stream对象。
2. 执行map操作，取出数组中每个元素做+3运算，这里用到了Java8的lambda表达式，i就相当于传入的元素。
3. 通过collect方法求值返回新的数组并赋值给mappedNumbers。

接下来对比下纯函数式语言Haskell的map函数。

```haskell
--Haskell map函数
map (+3) [1, 2, 3]    --[4, 5, 6]
```

同样这里的(+3)是一个函数类型参数，[1, 2, 3]是一个列表，map的作用就是将列表中的每一个元素都应用到(+3)函数上，然后取计算结果构成新的数组返回。

关于上面的(+3)函数，我们可以像Java8中使用lambda一样，通过匿名函数将(+3)的细节补全。

```haskell
--Haskell 通过匿名函数补全(+3)函数细节
map (x\ -> x+3) [1, 2, 3]    --[4, 5, 6]
```
直接隐藏掉参数x可以使代码更加简洁。

**过滤（filter）**

filter函数也是接受一个集合类型参数和一个函数类型参数，同map一样将集合中每个元素应用到函数，并将计算结果为真的元素组装成一个新的集合返回。

```java
//Java 通过filter返回数组中值小于3的元素

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
        List<Integer> filteredNumbers = numbers
                                        .stream()
                                        .filter(i -> i < 3)
                                        .collect(Collectors.toList());
        System.out.println(filteredNumbers);    //[1, 2]
    }
}
```
同map函数不同的是filter只返满足计算结果的元素。

用haskell实现相同的代码。

```haskell
--Haskell filer函数
filter (<3) [1, 2, 3, 4]    --[1,2]
```

**折叠（fold/reduce）**

折叠的执行顺序相对要复杂一些，折叠函数会将一个初始累积值和集合中的第一个元素应用到函数的第一次累积运算，然后将返回结果与集合中第二个元素应用到函数的第二次累积运算，如此反复，直到最后一个元素结束累积运算，返回最终运算结果。当没有初始累积值作为参数传入时，第一次累积运算就会取集合中第一个和第二个元素作为操作数。

```java
//Java 利用折叠对集合中所有元素求和

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
        Integer total = numbers
                            .stream()
                            .reduce(0, (i,j) -> i+j);
        System.out.println(total);    //10
    }
}
```
如上代码，reduce方法接受两个参数，第一个是累积初始值，第二个是一个累积函数，这个累积函数又需要两个参数，一个是累积结果，另一个是集合的下一个元素。整个方法可以理解为把集合中所有元素依次执行累积运算。

```haskell
--Haskell foldl函数
foldl (+) 0 [1, 2, 3, 4]    --10
```
同理上述Haskell代码中累积操作的缺省值也是0。

需要额外说明的是，折叠方法一般分为左折叠和右折叠，两者的区别在于：

* 左折叠从第一个元素开始累积，右折叠从最后一个开始累积。
* 左折叠中列表元素作为右操作数参与运算，而右折叠中列表元素作为左操作数参与运算。

关于第二条来看如下例子：

```haskell
foldl (-) 0 [1, 2]    -- -3

foldr (-) 0 [1, 2]    -- -1
```

foldl中首先拿列表中最左边元素1和缺省值0进行减法运算，1作为右操作数，所以是`0-1=-1`，接着-1作为累积结果和列表中后一个元素继续运算，此时2作为右操作数，即`-1-2=-3`，所以左折叠结果为-3。foldr中拿列表中最右边元素2和缺省值0进行减法运算，2作为左操作数，所以是`2-0=2`，接着2作为累积结果和列表前一个元素继续运算，1作为左操作数，即`1-2=-1`，所以右折叠结果为-1。

通常会依据将列表元素是作为左操作数还是作为右操作数来决定是用左折叠还是右折叠，但对于满足交换律的运算，比如加法、乘法，左折叠和右折叠的运算结果是相同的。另外右折叠因为是从最后一个元素开始累积，所以右折叠可以计算无限列表，但是左折叠却不行。

```haskell
--Haskell foldr处理无限列表

take 3 $ foldr (:) [] [1..]  --[1, 2, 3]
take 3 $ foldl (:) [] [1..]  --Occurs check: cannot construct the infinite type
```

**其他高阶函数**

一般提供函数式编程的语言都会提供map、filter、reduce这三个函数。对于列表操作而言，这三个函数就像命令式语言里的for、if语句一样不可或缺。除此之外还有一些高级函数也是很重要的，但是并非所有支持函数式的语言都提供这些函数。

* flip翻转函数的两个参数。
* max返回两个数字中大的一个。
* repeat无限循环一个数据。
* take取列表前面指定个数元素等。

这里单独说明下zipWith函数，zipWih会将两个列表作中的元素按顺序依应用到函数，然后将运算结果组装成一个新的列表返回。

```haskell
--Haskell zipWith函数
zipWith (*) [1,2,3] [2,3,4]    --[2, 6, 12]
```
zipWith会拿第一个列表的第一个元素1和第二个列表的第一个元素2应用到函数\(\*\)，并把运算结果作为第一个元素填充到新的列表，重复上述逻辑对后续元素依次执行运算，最终得到列表[2, 6, 12]。


#### 柯里化

##### 柯里化

柯里化的主要目的是单一化函数参数，也就是通过函数可以作为值传递这种特性，将一个参数与要应用的函数打包成新的函数。通过这种方式将所有函数都封装为单一参数的形式。

比如说上面Haskell中常见的加法运算正常情况下应该按`(+) 1 2`这样执行，`(+)`是函数名（haskell中加法运算符是个函数）而1和2是应用于函数的两个参数。通过柯里化将一个参数和函数打包后其执行过程就变成了`(+1) 2`，其中2是应用到函数`(+1)`中的参数，这样就达到了函数参数单一化的目的。

>实际上haskell中所有函数都是单参数的。

我们用Java重写上面的例子看看。

```java
//Java 柯里化单一化函数参数

import java.util.function.IntFunction;
import java.util.function.IntUnaryOperator;

public class Main {
  public static void main(String[] args) {
    IntFunction<IntUnaryOperator> curriedAdd = a -> b -> a + b;
    IntUnaryOperator adder5 = curriedAdd.apply(5);

    System.out.println(adder5.applyAsInt(4));                 //9
    System.out.println(curriedAdd.apply(5).applyAsInt(6));    //11
  }
}
```
简单说明下上述代码：

1. 首先声明了一个参数和返回类型都为Int的函数接口curriedAdd，在lambda表达式`a -> b -> a + b`中a和b表示参数，a+b表示返回值，每一个参数或返回值之间都用符号`->`链接。
2. 在curriedAdd的基础上应用了一个参数5声明了一个参数和返回值都为Int的函数接口adder5。
3. adder5就是柯里化后的函数，当一个参数作用于它时，它会把5作为和接收到的参数应用到curriedAdd执行加法操作并返回运算结果。

##### 部分施用

和柯里化类似，部分施用也是用来减少参数个数的，不同的是部分施用是通过提取代入一部分参数值，使一个多参数函数变为较少数目参数的函数，而不是必须转换为单参数的函数。我们来看下下面这个计算abc和的函数，是如何通过部分施用简化函数参数个数的。

```java
//Java 部分施用封装部分参数

@FunctionalInterface
interface TriFunction<T, U, V, R> {
  R apply(T a, U b, V c);
}

public class Main {
  public static int add(int x, int y, int z) {
    return x + y + z;
  }
  public static <T, U, V, R> BiFunction<U, V, R> partial(TriFunction<T, U, V, R> f, T x) {
      return (y, z) -> f.apply(x, y, z);
  }
  public static void main(String[] args) {
    BiFunction<Integer, Integer, Integer> partialFunction = partial(Main::add, 1);
    
    System.out.print(partialFunction.apply(2,3));          //6
    System.out.print(partial(Main::add, 2).apply(3,4));    //9
  }
}
```
简单说明下上述代码：

1. 首先定义一个接口TriFunction，这个接口接受三个参数T、U、V并返回R，这里R是一个函数。
2. 声明一个原始的a+b+c方法add。
3. 声明了一个返回了BiFunction接口（它和自定义的TriFunction接口类似，接受两个参数U和V并返回R）的方法partial，它接受两个参数TriFunction和T，其中TriFunction是一个具有默认函数的接口，T是TriFunction的第一个参数。同样partial方法也返回一个函数（接口），这个函数需要两个参数y和z。
4. 通过partial构造一个新的方法partialFunction，在构造这个方法时需传入要打包的方法和它的一个参数。之后我们就可以通过打包后的函数，传入其他两个参数来等价执行add方法了。
5. 我们也可以像`partial(Main::add, 2).apply(3,4)`这样不显示的指定打包函数，就像之前的stream操作一样支持连续传递参数。

细心观察会发现我在上述说明中用到`函数（接口）`这样的说明，这是因为在Java8中函数仍旧不是一等公民，诸如TriFunction、BiFunction这些函数接口，本质上是一种接口而不是函数，实际上返回的仍旧是接口类型。只不过Java8针对接口增加了默认函数概念，当函数接口执行apply方法时正是执行了默认函数，因此看起来这个函数接口就像一个函数一样在运作。

总之我们还是可以通过打包函数的方式将一个需要传递三个参数的函数拆分为两次调用，第一次传递一个参数，第二次传递两个参数。这样通过函数打包的方式来减少参数个数的方式就是部分施用。

##### 对比函数调用、部分施用和柯里化

最后我们还是以Haskell为例来对比下普通的函数调用、部分施用和柯里化三者之间的区别。我们定义一个函数，接受三个参数，第一个参数表示一个区间的下限，第二个参数表示这个区间的上限，第三个参数是一个需要判断是否在区间内的数字，最后我们把比较结果以字符串的方式展示。

```haskell
--Haskell inRange函数判断数字是否在区间内

inRange:: Int -> Int -> Int -> String
inRange a b c
  |     a > c = "small than minimum" 
  |     b < c = "large than maximum"
  |     otherwise = "in range"
```

不用担心上面的语法，你只要记得这个inRange函数能够判断一个数字是否在区间内就可以了，我们主要来看下面几种调用：

```haskell
(inRange 1 10 5)         --in range
((inRange 1 10) -2)      --small than minimum
(((inRange 1) 10) 12)    --large than maximum
```

1. 第一个调用我们打包了一个函数，这个函数传入了三个参数，这有点像我们在命令式编程里书写的代码，拿到三个参数后比较，然后分情况返回结果。

2. 第二个调用我们打包了两个函数，第一个函数接受两个参数用来生成一个区间，第二个函数接受一个参数作为匹配值，这就像部分施用，我们通过打包函数的方式分组或者简化了参数个数。

3. 第三个调用每次传入一个参数，我们把每次传入的参数记录起来，并在最后一个函数中执行所有的判断，这就是柯里化。

当然Haskell是自动柯里化的，也就是说无论你函数的参数列表定义了多少参数，实际执行的时候都会被抽离成单参数的形式，也就是说第一个调用及第二个调用实际上和第三个调用的执行过程是一样的。这个例子的前两种解释说明只是为了帮你区分部分施用和柯里化的区别。

#### 函数组合

如果说柯里化是将一整个函数拆分成一段一段的单个函数，依次传参执行的话。那么函数组合就像是反过来的过程，是将几个函数组合成一个新的函数然后传参执行。不过不管是柯里化还是函数组合最终都是打包成一个只接收一个参数的函数。

柯里化是把参数传入第一个函数，执行后返回一个新的函数，然后把第二个参数应用到这个新函数，如此反复。而函数组合是把参数传递给第一个函数，执行后返回一个值，然后把值传入第二个函数，如此反复。所以柯里化看起来是一个函数连续调用了几个参数，而函数组合是多个函数嵌套调用了一个参数。

<p style="text-align: center">
  <div class="img" style="
    background-position: 0px -40px;
    height: 40px;
    background-image: url(/images/curry_and_function_composition.png);
    height: 160px;
    width: 540px;
    background-repeat: no-repeat;
    margin: auto;">
  </div>
</p>

在数学中`f(g(x))`可以写作`f·g(x)`，同样为了便于书写和阅读，Haskell也提供了`f.g(x)`这样的表达方式。

接下来我们通过加法函数的嵌套来说明下函数组合。

```haskell
--Haskell 函数嵌套
(+3)((+5) 1)    --9
```
上面是正常的函数嵌套，首先1应用到(+5)函数，返回结果为6，然后6应用到(+3)函数，返回结果9。

接下来看下函数组合如何表述

```haskell
--Haskell 函数组合
(+3).(+5) $ 1    --9
```
上面我们用`.`将(+3)和(+5)打包成了一个新函数，方便理解我们把这个新函数叫做(+3).(+5)，然后我们通过`$`运算符指明将1应用到新函数(+3).(+5)。

函数组合本身还是按照正常的函数调用顺序执行，也就是说上面的例子其实还是1先应用到(+5)返回6，(+3).(+5)这种写法只是为了方便表述和阅读。

#### lambda表达式

我们在讲匿名函数的时候已经提到过lambda，其实你也可以把lambda看做是一次性的匿名函数（至少Haskell是如此的）。

我们来看一个例子，对比一下lambda的作用，假如我们想要计算一个数组中所有数字的平方和是多少，在不使用lambda的情况下我们必须先要给出一个求平方和的函数作为foldl的一个参数。

```haskell
--Haskell 求平方和函数

squareSum:: Num a => a->a->a
squareSum a b = a + b ^2

foldl squareSum 0 [1,2,3,4]    --30
```
但是这个平方和函数并不容易维护，如果我现在想要的操作是立方和，那么我出于语义上的考虑还需要修改squareSum的名字，而且这个函数本身看起来也并不简洁。

替换为lambda表达式：

```haskell
--Haskell 匿名函数替换平方和函数
foldl (\a,b -> a+b^2) 0 [1,2,3,4]    --30
```

在haskell中`\`用来声明一个lambda表达式，因为它看起来像是λ所以叫做lambda表达式。


便于对比说明，我们用Java重现刚才的函数。
```java
//Java lambda表达式

public static void main(String[] args) {
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
  Integer total = numbers
                  .stream()
                  .reduce(0, (i,j) -> i+j*j);
  System.out.println(total);    //30
}
```

可以看到和Haskell类似`(i,j) -> i+j*j`就是一个lambda表达式，我们会传入i和j两个参数，并返回i+j*j。


### 6.3 其他概念

#### 闭包（closure）

闭包是函数及其相关引用环境组合而成的一个整体，为了方便解释，你把闭包看作是一个容器，在这个容器中包含一个函数，和几个参数，函数可以像访问自己内部定义的变量一样访问容器中的参数。

先看下面这段代码：

```haskell
--Haskell map'函数

map' list = map (+3) list
map' [1,2,3]    --[4,5,6]
```

这是一个高阶函数，我们把列表中每一项都应用了(+3)函数，我们对它进行抽象，让他变成`map (+x) list`。

```haskell
--Haskell map'函数闭包应用

map' x list = map (+x) list
map' 3 [1,2,3]    --[4,5,6]
```

做了如上改进，我们让map'传入两个参数，一个是加法运算的操作数另一个是被操作的列表，其中map就是一个容器，(+x)就是容器中的函数。接下来在map'得到参数x为3时，就变成了`map (+3) list`，我们再把(+3)用匿名函数的方式展开就变成了：`map (\x->x+3) list`，这时我们看到3变成了函数`(\x->x+3)`中的一部分，而3本身不是函数中定义的，而是通过容器map'传入的。这种函数自由访问容器中外部变量的特性就是闭包，整个容器就是这个函数和它外部变量的**闭包**。

#### 递归（recurse）

在命令式编程中我们通常是通过索引来获取数组中元素的，但在函数式中我们通常会将列表分成头和尾两部分，然后通过递归尾部（将尾部再次分为头和尾，尾部越来越短直至为空）来访问列表中的每一个元素。

```haskell
--Haskell 通过递归将列表中元素取反

negatedEvery:: Num a=>[a]->[a]
negatedEvery [] = []
negatedEvery (x:xs)  = -x : negatedEvery xs

negatedEvery [1,2,3,4]    --[-1,-2,-3,-4]
```

上面代码中函数negatedEvery作用是将列表元素取反。首先我们看第三行，x:xs是将列表拆分为头部x和尾部xs，然后分别对x和xs赋值。此时头部元素是一个数值x，所以可以直接对其取反，赋值为-x。而尾部元素仍旧是一个列表，所以尾部将作为一个新的列表重新应用到negatedEvery函数。然后我们看第二行，这里表示如果元组为空则返回空，也就是说当我们不断递归xs到negatedEvery函数时，只要列表不是无限的终究会执行到`negatedEvery [] = []`，此时列表将执行`[] = []`退出递归，结束函数执行。最终返回一个所有元素都执行了取反运算的新列表。

接下来我们用递归来解决斐波那契数列问题。

>斐波那契数列又称黄金分割数列，是数学家列昂纳多·斐波那契以兔子繁殖为例而提出的，因为也称作兔子数列。

斐波那契数列满足如下计算规则：

1. F(0) = 1
2. F(1) = 1
3. F(n) = F(n-2) + F(n-1)

斐波那契数列本身就是典型的递归实现，我们根据计算规则可以明确三种情况：

1. n=0时值为0
2. n=1时值为1
3. n为其他值时，递归F(n-1)+F(n-2)

先用Java实现如上逻辑。

```java
//Java 通过递归实现斐波那契数列

class Recursive {
  static LongUnaryOperator func =
          x -> (x == 1 || x == 0) ? x :
          Recursive.func.applyAsLong(x -1) + Recursive.func.applyAsLong(x - 2);

  public static long fibonacci(int n){
    return func.applyAsLong(n);
  }
}

public class Main {
  public static void main(String[] args) {
    System.out.print(Recursive.fibonacci(30));
  }
}
```
简单说明上述代码：

1. 首先声明一个LongUnaryOperator函数接口，这个函数接口约定传入参数和返回类型都是long类型。
2. 接着用lambda表达式对输入参数x应用三元运算符，如果x值为0或者1则返回1，否则返回`F(n-1) + F(n-2)`。
3. 最后声明一个取斐波那契数的方法，将传入参数转换为long类型应用到之前声明的LongUnaryOperator函数接口。

同理Haskell的实现方式类似。

```haskell
--Haskell 实现斐波那契数列

fibonacci::Integer->Integer
fibonacci 0 = 0
fibonacci 1 = 1
fibonacci n = fibonacci (n-1) + fibonacci (n-2)
```

#### 尾调用（tail call）

尾调用是指一个函数里的最后一个动作是一个函数调用，这个调用的返回将作为函数的返回值返回。如果函数在最后调用了自身，则称作尾递归。

通常情况下函数在调用时会记录当前调用位置以及内部变量等信息，以便计算函数的返回值。但是尾调用函数本身就是函数的返回值，所以记录尾调用函数的调用位置和内部变量等信息是没有必要的。因此尾调用可以减少栈空间的使用。不过单纯的尾调用即使存储了不必要的调用信息也不会浪费太多栈内存，但是尾递归调用就大不相同了，我们来看下尾递归调用下记录调用信息和不记录调用信息两种情况下栈的变化。

先看下这段没采用尾调用代码：

```haskell
--Haskell 阶乘函数

factorial 0 = 1
factorial n = n * factorial(n-1)

factorial 3
```
```haskell
--Haskell 阶乘函数运算过程

factorial 3
3 * (factorial 2)
3 * (2 * (factorial 1))
3 * (2 * (1 * (factorial 0)))
3 * (2 * (1 * 1))
3 * (2 * 1)
3 * 2
6
```
每次调用需要记录函数调用位置，以及内部变量，然后再依次求值，最后释放内存空间。

用尾递归重写上面的代码：

```haskell
--Haskell 尾递归重写阶乘函数

tail_factorial x = factorial' x 1 where
  factorial' 1 y = y
  factorial' x y = factorial' (x-1) $! (x*y)

tail_factorial 3
```

尾递归优化后，只需要记录当前函数下的内部变量，不需要花费更多额外的空间记录调用函数的位置及调用函数的内部变量。

```haskell
--Haskell 阶乘函数尾递归优化后运算过程

factorial' 3 1
factorial' 2 3
factorial' 1 6
6
```
在Haskell中尾递归优化是默认支持的，而像Java、Ruby、Python等这些面向对象语言，解释器并没有针对尾递归调用进行优化处理，不过Java8提供函数式支持后有针对尾递归进行了优化，ES6标准也规定了JavaScript语言必须实施尾递归优化。也就是说在这些后续支持了尾递归优化的语言，在使用尾递归处理问题时不会像以前一样再出现栈溢出的问题了。

```javascript
//JavaScript 尾递归导致栈溢出

var stackOverFlow = function (n){
  if(n == 0)  return true;
  else        return stackOverFlow(n-1);
}

stackOverFlow(100000);  
//Uncaught RangeError: Maximum call stack size exceeded
```

#### 记忆（memoization）

记忆的作用是用来存储一个函数的返回值，因为函数式具有输入相同的参数必然返回相同计算结果的特性，所以可以把同一调用结果记录起来，以减少计算开支。这种为了避免重复计算同一个函数调用而引入的机制就叫作记忆。

通常我们会把输入参数作为key，把输出结果作为value存储在一个缓存结构中（哈希或者元组，面向对象用哈希来存储键值对，函数式用元组来存储键值对），在函数调用时先遍历这个缓存结构中是否存在当前调用的key，如果存在则返回缓存中的value，不存在则计算，并插入这对键值到缓存结构中。

我们还是以斐波那契数列为例说明，在不使用缓存机制时斐波那契数列的计算是非常慢的。
```haskell
--Haskell 不使用记忆，执行斐波那契函数

fibonacci 30
{- 
  832040
  (2.09 secs, 554,439,712 bytes)
-}
```

添加记忆机制后

```haskell
--Haskell 添加记忆后执行斐波那契函数

memoized_fib :: Int -> Integer
memoized_fib = (map fib [0 ..] !!)
   where fib 0 = 0
         fib 1 = 1
         fib n = memoized_fib (n-2) + memoized_fib (n-1)
         
memoized_fib 30
{-
  832040
  (0.01 secs, 100,520 bytes)
 -}
```

这样每当执行memoized_fib函数时都会把计算结果缓存起来，下次执行的时候就可以省去计算直接使用了。因为斐波那契数列存在大量的重复运算，所以缓存的添加让执行效率有了质的改变。

来看下Java8版本的实现。

```java
//Java 添加记忆重写斐波那契数列

public class Main {
  private static Map<Integer,Long> memo = new HashMap<>();
  static {
      memo.put(0,0L); 
      memo.put(1,1L); 
  }
  public static long fibonacci(int x) {
      return memo.computeIfAbsent(x, n -> (fibonacci(n-1) + fibonacci(n-2)));
  }

  public static void main(String[] args) {
      System.out.print(fibonacci(30));
  }
}
```

我们重点来看fibonacci这个函数，它调用了memo的computeIfAbsent方法，这里computeIfAbsent是Java8对Map接口新增的方法，它传入两个参数，一个是键值，另一个是函数接口。当key（这里key就是x）值不在map中时会将key作为键，以key为参数应用到函数运算得到的返回值作为value追加到map中。最后因为lambda表达式中没有区分当x=0和x=1这两种特殊情况，所以我们干脆直接在map中添加key为0和1的值，这样当x=0或者x=1时就避开了错误的函数调用。

#### 惰性求值（lazy evaluation）

惰性求值是指表达式不会在它被绑定到变量之后就立刻求值，而是在该值被真正使用的时候才求值，惰性求值有两个优点：

* 可以提高计算性能
* 能构造无限列表（因为惰性求值只关心列表中要求算的那个数，并不关心后面是否还有其他数据）。

某些编程语言默认就是惰性求值的，比如Haskell，另外也有些语言提供了惰性求值的函数或语法，比如Java8的stream操作就是惰性求值的。我们先通过haskell看一个无限列表的例子。

假设我们有这样一个需求：求所有三的倍数的数字之和。

```haskell
--Haskell 惰性求值实现无限列表

let mod3 = filter (\x -> mod x 3 == 0) [1..]
take 5 mod3                --[3, 6, 9, 12, 15]
foldl (+) 0 $ take 5 mod3  --45
```
我们创建了一个mod3函数，这个函数过滤出所有三的倍数（但不会马上计算）。之后用take先后取了前五个三的倍数，因为惰性求值的缘故，mod3此时才会计算，并且只会计算到第五个元素。这就是一个惰性求值实现无限列表求值的例子。

接下来我们看看Java8中stream是如何表现出惰性求值的。
```java
//Java steam惰性求值实例

public static void main(String[] args) {
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
  numbers.stream().filter(n-> {
    System.out.print(n);
    return n > 2;
  });
}
```
执行上面这段代码不会输出任何结果，说明filter内的函数并没有执行。这是因为stream是惰性求值的，如果想得到链式操作的返回结果需要在最后执行一个及时求值的方法。

```java
//Java 通过及时求值函数计算stream返回值

public static void main(String[] args) {
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
  List<Integer> results = numbers.stream().filter(n-> {
      System.out.print(n);    //=>12345
      return n > 2;
  }).collect(Collectors.toList());
}
```
和之前不同我们在Stream操作最后调用了`collect(Collectors.toList())`方法，这是一个及时求值的方法，它会把之前的表达式执行并返回计算结果，因此这时`System.out.print(n);`就会把numbers的所有元素都打印出来了。


#### 不相交联合体（disjoint union）

##### Either

在函数式编程中经常会遇到需要返回两种不同类型的情况，为了满足这种需求而设计了不相交联合体。不相交联合体可以存储两种不同类型中的某一种类型。一般会用Either类来表述这种结构，Either有一个左值和一个右值，但是左值和右值只能选择一个。

```haskell
--Haskell 左值和右值

Left "This is left value"
Right 2

```

Either类型另外一个重要的作用就是处理异常，很多函数式语言是没有异常处理机制的，因为异常的设计和函数式语言无副作用的思想是冲突的，异常是有副作用的，同时异常会引导程序进入异常处理流程而不是期望的返回值。而函数式编程是以值为根本的，所以在函数式语言中通过把异常信息记录在Either类型的左值中，正确值记录在Either类型的右值中来避免异常产生的副作用。

```haskell
--Haskell 通过Either包装异常信息

div3:: Float->Float->Either String Float
div3 x 0 = Left "Divison by zero"
div3 x y = Right (x / y)

div3 3 0    --Exception: divide by zero
div3 3 3    --1
```
上面定义了一个除3的方法，当除数为0时，返回左值，给出错误信息`除数为0`，其他情况下执行除运算并返回右值。

Java本身没有提供Either类，开发人员可以通过这种思想构造Either类，其他基于函数式的语言诸如Scala本身是内置Either类的。

**Maybe/Optional**

Maybe(Java/Scala中称作Optional)类型和Either类型类似，你可以把它看做是一种简单的异常场景，它也存储两个值，一个为空，另一个为有效值。你可以认为Maybe类型只对返回结果做了失败还是成功的判断，如果失败则返回空，如果成功则返回有效的计算结果。

```haskell
--Haskell Maybe类型

returnEven:: Int -> Maybe Int
returnEven a
  | a `mod` 2 == 0 = Just a
  | otherwise = Nothing
 
returnEven 2    --Just 2
returnEven 1    --Nothing
```
如上定义了一个返回偶数的函数returnEven，函数返回了一个Maybe类型。如果计算结果为偶数则返回当前值（Just是Haskell对Maybe类型中有效值的包装，这里不用在意），否则返回Nothing。

Java8也提供了Optional类型用来代替null值。通过Optional代替null表示值不存在，可以避免一些不必要的麻烦，Optional也可以用来判断一个变量是否存在值。

```java
//Java Optional使用

Optional emptyOptional = Optional.empty();
Optional<String> a = Optional.of("a");

System.out.print(emptyOptional.get());    
//java.util.NoSuchElementException: No value present

System.out.print(a.get());                
//a

```

#### 纯度（purely）

之前在介绍函数式特点的时候有提及过纯度，纯度的特性有一定的优点但也存在很多问题，比如很多操作（混合运算）并非是顺序无关的，这些操作是有副作用的（输入结果相同，函数执行顺序不同，返回结果不同），因此对于纯函数式的语言而言就要面对一些棘手的问题（但并非所有支持函数式编程的语言都是纯函数式的）。

我们来通过一个过平衡木的例子来说明Haskell是如何处理因函数执行顺序而产生的副作用的。

平衡木游戏的规则如下：

1. 用向左倾斜和向右倾斜的程度表示一个人的平衡程度，向左倾斜用负数表示，向右倾斜用正数表示。
2. 向左平衡程度和向右平衡程度的和的绝对值表示平衡值，如果平衡程度值小于3则人是平衡的。
3. 人每向前移动一步，则随机追加左右倾斜值，计算平衡值。将平衡值传给下一步。

```haskell
--Haskell 平衡木游戏

type Rate = Int
type Balance = (Rate, Rate)

toLeft:: Rate->Balance->Balance
toLeft n (left, right) = (left-n, right) 

toRight:: Rate->Balance->Balance
toRight n (left, right) = (left, right+n) 
```

我们用Rate表示平衡倾斜程度，用Balance表示平衡程度，其中left指向左倾斜程度，right指向右倾斜程度。这样我们定义了向左倾斜函数toLeft和向右倾斜函数toRight，这两个函数都要求输入当前平衡度及平衡倾斜数，然后返回新的平衡度。

我们做如下调用，向左倾斜1 -> 向右倾斜1 -> 向左倾斜2。

```haskell
--Haskell 平衡木游戏
toLeft 2 (toRight 1 (toLeft 1 (0,0)))    --(-3, 1)
```
到目前为止是没有问题的，我们在这个基础上继续， 向右倾斜1 -> 向左倾斜3 -> 向右倾斜2

```haskell
toRight 1 (toLeft 3 (toRight 2 (-3,1)))    --(-6, 4)
```
这时返回的结果仍旧是保持平衡的：`|-6+4|<3`。但实际上在向左倾斜3的时候就已经失去平衡而摔倒了，所以是不可能再向右倾斜2从而恢复到平衡状态的。因此我们需要在失去平衡的时候返回一个失败信息，并且这个信息会一致传递下去。

我们做如下修改：

```haskell
--Haskell 平衡木游戏

type Rate = Int
type Balance = (Rate, Rate)

toLeft:: Rate->Balance->Maybe Balance
toLeft n (left, right)
  | abs (left -n + right) < 3 = Just (left-n, right)
  | otherwise = Nothing

toRight:: Rate->Balance->Maybe Balance
toRight n (left, right) 
  | abs (left + right + n) < 3 = Just (left, right+n)
  | otherwise = Nothing
```

我们对要传递给下一步的平衡状态包装为Maybe类型，如果失去平衡则返回Nothing，否则返回Just。这样当我们接受到Nothing的时候就只会传递Nothing到下一步了。

为了让一个值应用到函数并返回一个包装类型我们要用到monad（就是下面的`>>=`函数）。

```haskell
return (0,0) >>= toLeft 2 >>= toRight 1 >>= toLeft 1      --Just(-3,1)
return (-3,1) >>= toRight 1 >>= toLeft 3 >>= toRight 2    --Nothing
```
可以看到这回返回了“摔倒”，同时monad不仅将值应用到了函数包装类型，同时还提供了函数的链式调用，这种写法更加干净易懂。另外，针对`toRight 1 (toLeft 3 (toRight 2 (-3,1)))`这种嵌套调用，我们也可以定义一个`-:`方法实现函数的链式调用。

```haskell
--Haskell 实现链式调用

x :- f = f x
(0,0) -: toRight 1 -: toLeft 3 -: toRight 2    --(-3,3)
```
通过上面的例子可以看到为了处理产生副作用的调用，Haskell提供了诸如Maybe、Monad这样的概念。而对于允许副作用的语言处理其相同的问题就简单的多，这也是纯函数式对比命令式的一个缺点。