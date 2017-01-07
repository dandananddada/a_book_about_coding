# 高阶函数

在函数类型介绍时已经提到过高阶函数的概念，高阶函数必须至少满足以下两个条件之一：

1. 传入参数为函数类型
2. 返回值为函数类型

简单的说将函数类型当做值一样使用，在函数中传递或返回，这样的函数就是高阶函数。

####工具类高阶函数

在介绍其他和高阶函数相关的概念之前，我们先看下针对列表操作而设计的三个最基础的高阶函数。

**映射（map）**

map函数会接受一个集合类型（比如列表、数组、链表）参数和一个函数类型参数，然后将集合中的每一个元素应用到函数，并把运算结果组装成一个新的集合返回。

我们来看下下面这个例子：

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

Java8中加入了stream库来提供对函数式编程的支持，通过stream库可以使用流畅的API来操作数据（类似于之前用underscore.js介绍链式调用使用的chain）。

简单说明下上面的代码：

1. 首先声明一个数组numbers，并通过stream方法打包成提供集合操作的stream对象。
2. 执行map操作，取出数组中每个元素做+3运算，这里用到了Java8的lambda表达式，i就相当于传入的元素。
3. 通过collect方法求值并返回新的数组赋值给mappedNumbers。

接下来对比下纯函数式语言Haskell的map函数

```haskell
--Haskell map函数
map (+3) [1, 2, 3]    --[4, 5, 6]
```

同样这里的(+3)是一个函数类型参数，[1, 2, 3]是一个列表（你可以理解为数组，因为Haskell中列表是定长非异质的，这跟Java的数组是一样的）。map的作用就是将列表中的每一个元素都应用到(+3)函数上。

关于上面的(+3)函数，这里想多说明一些细节，其实你也可以像Java8中使用lambda一样，通过匿名函数将(+3)函数细节补全。

```haskell
--Haskell 匿名函数补全(+3)函数实现
map (x\ -> x+3) [1, 2, 3]    --[4, 5, 6]
```
而这里直接隐藏掉参数x传入的细节，使代码更加简洁。

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

和map函数执行类似，唯一不同的是map会返回所有计算后的元素，而filter只返满足计算结果的元素。

同样haskell中也是接受一个列表[1, 2, 3, 4]并将其中每个元素应用到函数(<3)，最后把计算结果为真的元素组装成新的列表返回。

```haskell
--Haskell filer函数
filter (<3) [1, 2, 3, 4]    --[1,2]
```

**折叠（fold/reduce）**

折叠的调用相对复杂一些，折叠函数将一个初始值和集合中的第一个元素应用到函数执行第一次累计运算，然后将返回结果与集合中第二个元素应用到函数进行下一次累计运算，如此反复，直到最后一个元素结束累计运算，最终返回一个累计的结果。当没有初始值作为参数传入时，第一次累计运算就会将集合中第一个和第二个元素作为参数，后续运算同上不变。

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
如上reduce方法接受两个参数，第一个是累计初始值，第二个是一个累计函数，这个累计函数又需要两个参数，一个是累计结果，一个是集合类型中的下一个元素。整个方法可以理解为把集合中所有元素依次执行累计运算（这里是加法运算），第一次执行时因为没有累计结果，所以传入了一个缺省值0。

```haskell
--Haskell foldl函数
foldl (+) 0 [1, 2, 3, 4]    --10
```
同理Haskell中0为累计操作的缺省值。

需要额外说明的是，折叠方法一般分为两种，左折叠和右折叠。两者的区别是
1. 左折叠从第一个元素开始累计，右折叠从最后一个开始累计。
2. 左折叠中列表元素作为右操作数参与运算，而右折叠中列表元素作为左操作数参与运算。

关于第二条来看如下例子：

```haskell
foldl (-) 0 [1, 2]    -- -3

foldr (-) 0 [1, 2]    -- -1
```

foldl中拿列表中最左边元素1和缺省值0进行减法运算，1作为右操作数，所以是`0-1=-1`，接着-1作为累计结果和列表下一个元素继续运算，2作为右操作数，即`-1-2=-3`，所以左折叠结果为-3。

foldr中拿列表中最右边元素2和缺省值0进行减法运算，2作为左操作数，所以是`2-0=2`，接着2作为累计结果和列表上一个元素继续运算，1作为左操作数，即`1-2=-1`，所以右折叠结果为-1。

通常依据将列表中元素作为左操作数还是右操作数来决定是用左折叠还是右折叠，但对于满足交换律的运算，比如加法、乘法，左折叠和右折叠的运算结果是相同的。

另外右折叠因为是从最后一个元素开始累计，所以右折叠可以计算无限列表，但是左折叠却不行。

**其他高阶函数**

一般提供函数式编程的语言都会提供map、filter、reduce三个函数，对于列表操作，这三个函数就像命令式语言里的for、if语句一样不可或缺。

除此之外还有另外一些高级函数也是很重要的，但是并非所有支持函数式的语言都提供这些函数。

1. flip翻转函数的两个参数
2. max返回两个数字中大的一个
3. repeat无限循环一个数据
4. take取列表前面指定个数元素等

这里单独说明下zipWith函数。

zipWih会将两个列表作为参数按顺序依应用到函数，然后将运算结果组装成一个新的列表返回。

```haskell
--Haskell zipWith函数
zipWith (*) [1,2,3] [2,3,4]    --[2, 6, 12]
```
zipWith会拿第一个列表的第一个元素1和第二个列表的第一个元素2应用到函数\(\*\)，并把运算结果作为第一个元素填充到新的列表，重复上述逻辑对后续元素依次执行运算，最终得到列表[2, 6, 12]\(1\*2=2, 2\*3=6, 3\*4=12\)。


####柯里化

**柯里化**

柯里化的主要目的就是单一化函数参数，也就是通过函数可以作为值传递这种特性，将一个参数与要应用的函数打包成新的函数。通过这种方式将所有函数都封装为单一参数的形式。

比如说上面Haskell中常见的加法运算正常情况下`(+) 1 2`应该这样执行，(+)是函数名（haskell中加法运算符是个函数）而1和2是两个参数。实际上haskell所有函数都是单参数的，其执行过程是函数(+)和参数1组合成一个新的函数(+1)，然后参数2再应用到函数(+1)。这就是将多个参数通过柯里化分割为单个参数的过程。

我们用Java重写上面的例子看看：
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
3. adder5就是柯里化后的函数，当一个参数作用于它时，他会把5作为默认参数和接收到的参数应用到curriedAdd然后执行加法操作并返回运算结果。

**部分施用**

和柯里化类似，部分施用也是用来减少参数个数的，不同的是部分施用是通过提取代入一部分参数值，使一个多参数函数变为较少数目参数的函数，而不是必须转换为单参数的函数。

我们来看下下面这个a+b+c的函数，是如何通过部分施用将函数和一个参数打包成一个需要传入两个参数的新函数的。

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

细心观察的话会发现我在上述说明中用到`函数（接口）`这样的说明，这是因为在Java8中函数仍旧不能作为值传递和返回，诸如TriFunction、BiFunction这些函数接口，本质上是一种接口而不是函数，实际上返回的仍旧是接口类型。只不过Java8中新增了接口的默认函数，这些函数接口都包含一个默认函数，而接口类型的apply方法正是执行了默认函数。因此看起来这个函数接口就像一个函数一样在运作。

总之我们还是可以通过打包函数的方式将一个需要传递三个参数的函数调用拆分为两次，第一次传递一个参数，第二次传递两个参数。这样通过函数打包的方式来减少参数个数的方式就是部分施用。

**总结**

最后我们还是以Haskell为例来对比下普通的函数调用、部分施用和柯里化间的区别。

这里我们定义一个函数，接受三个参数，第一个参数表示一个区间的下限，第二个参数表示这个区间的上限，第三个参数用来判断是否在这个区间，并把比较结果以字符串的方式返回。

```haskell
--Haskell inRange函数判断数字是否在区间内

inRange:: Int -> Int -> Int -> String
inRange a b c
  |     a > c = "small than minimum" 
  |     b < c = "large than maximum"
  |     otherwise = "in range"
```

不用担心上面的语法，你只要记得这个inRange函数的作用就可以，我们主要来看下面几种调用：

```haskell
(inRange 1 10 5)         --in range
((inRange 1 10) -2)      --small than minimum
(((inRange 1) 10) 12)    --large than maximum
```

第一个调用我们打包了一个函数，这个函数传入了三个参数，这有点像我们在命令式编程里书写的代码，拿到三个参数后比较，然后分情况返回结果。

第二个调用我们打包了两个函数，第一个函数接受两个参数用来生成一个区间，第二个函数接受一个参数作为匹配值，这就像部分施用，我们通过打包函数的方式分组或者简化了参数个数。

第三个调用，每次传入一个参数，我们把每次传入的参数记录起来，并在最后一个函数中执行所有的判断，这就是柯里化。

当然Haskell是自动柯里化的，也就是说无论你函数的参数列表定义了多少参数，实际执行的时候都会被抽离成单参数的形式，也就是说第一个调用及第二个调用实际上和第三个调用的执行过程是一样的。我这里这么说明只是帮你区分部分施用和柯里化的区别。

####函数组合

如果说柯里化是将一整个函数拆分成一段一段的单个函数，然后依次传参执行。那么函数组合更像是反过来的过程，是将几个函数组合成一个新的函数然后传参执行。不过不管是柯里化还是函数组合最终都是打包成一个只接收一个参数的函数。

柯里化是把参数传入第一个函数，执行后返回一个新的函数，然后把第二个参数应用到这个新函数，依次；
而函数组合是把参数传递给第一个函数，执行后返回一个值，然后把值传入第二个函数，依次。所以柯里化看起来是一个函数连续调用了几个参数，而函数组合是多个函数嵌套调用了一个参数。

<p style="text-align: center">
  <div class="img" style="
    background-position: 0px -40px;
    height: 40px;
    background-image: url(../images/curry_and_function_composition.png);
    height: 160px;
    width: 540px;
    background-repeat: no-repeat;
    margin: auto;">
  </div>
</p>

在数学中f(g(x))可以写作f·g(x)，同样为了便于书写和阅读，Haskell也提供了f.g(x)这样的表达方式。

我们通过加法函数的嵌套来说明下函数组合。

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

####lambda表达式

我们在讲匿名函数的时候已经提到过lambda，其实你也可以把lambda看做是一次性的匿名函数（至少Haskell是如此的）。

我们来看一个例子，对比一下lambda的作用：

假如我们想要计算一个数组中所有数字的平方和是多少，在不使用lambda的情况下我们必须先要给出一个求平方和的函数作为foldl的一个参数。

```haskell
--Haskell 求平方和函数

squareSum:: Num a => a->a->a
squareSum a b = a + b ^2

foldl squareSum 0 [1,2,3,4]    --30
```
但是这个平方和函数并不容易维护，如果我现在想要的操作是立方和，那么我出于语义的方便还需要修改squareSum的名字，而且这个看起来也并不是很简洁。

替换为lambda表达式：

```haskell
--Haskell 匿名函数替换平方和函数
foldl (\a,b -> a+b^2) 0 [1,2,3,4]    --30
```

在haskell中`\`用来声明一个lambda表达式，因为它看起来像是λ所以叫做lambda表达式。

同样的例子我们来看下Java版的实现。

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
