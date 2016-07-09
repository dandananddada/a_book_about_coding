# 高阶函数

在介绍函数时已经说过，高阶函数必须至少满足以下两个条件之一：
1. 传入参数为函数类型
2. 返回值为函数类型

简单的说函数类型可以像其他数据类型一样当做值在函数中传递。


####工具类高阶函数

在介绍其他和高阶函数相关的概念之前，我们先看下针对列表操作而设计的三个最基础的高阶函数。

**映射（map）**

map会接受一个集合类型（比如列表、数组、链表）和一个函数，然后将集合中的每一个元素作为参数传入这个函数，并把返回结果重新组装成集合返回。

我们来看下面这个例子，把集合中每个元素的值都`+3`然后返回。

```java
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
        System.out.println(mappedNumbers);    //=>[4, 5, 6]
    }
}
```
Java8支持函数式编程，提供了stream库，通过stram可以使用更加流畅的API来操作数据。
1. 首先声明一个数组numbers，并通过stream方法打包成提供集合操作的stream对象。
2. 执行map操作，取出数组中每个元素做`+3`运算。
3. 通过collect方法求值并返回新的数组mappedNumbers。

接下来看下纯函数式语言Haskell的map函数
```haskell
map (+3) [1, 2, 3]
-- =>[4, 5, 6]
```
同样`(+3)`是一个函数参数，`[1, 2, 3]`是一个列表（你可以理解为数组，因为Haskell中列表是定长非异质的，这跟Java的数组是一样的）。map的作用就是将列表中的每一个元素都应用到(+3)的函数上。

**过滤（filter）**

filter接受一个集合类型和一个函数，将集合中每个元素传入函数，并将满足函数返回值为真的元素重新组装成集合返回。

```java
/* 通过filter返回数组中值小于3的元素 */
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
        System.out.println(filteredNumbers);    //=>[1, 2]
    }
}
```
同样haskell接受一个列表`[1, 2, 3, 4]`并将其中每个元素传入函数`(<3)`，并把返回值为真的（值小于3）元素组装到列表返回。
```haskell
filter (<3) [1, 2, 3, 4]
-- =>[1,2]
```

**折叠（fold/reduce）**

折叠会用一个初始值和集合中的第一个值应用到函数进行累计操作，并将返回结果与集合中第二个元素进行累计操作，如此反复，最终返回一个累计结果。

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
        Integer total = numbers
                            .stream()
                            .reduce(0, (i,j) -> i+j);
        System.out.println(total);    //=>10
    }
}
```
如上reduce方法接受两个参数，第一个是累计初始值，第二个是一个累计函数（这里是加法运算），这个累计函数又需要两个参数，一个是累计结果，一个是集合类型中下一个元素。整个方法可以理解为把集合中所有元素以此执行累计方法，第一次执行时因为没有累计结果，所以传入了一个缺省值。

```haskell
foldl (+) 0 [1, 2, 3, 4]
-- => 10
```
同理Haskell中0为累计操作的缺省值。

需要额外说明的是，折叠方法一般分为两种，左折叠和右折叠。两者的区别是
1. 左折叠从第一个元素开始累计，右折叠从最后一个开始累计。
2. 左折叠中列表元素作为右操作数参与运算，而右折叠中列表元素作为左操作数参与运算。

关于第二天来看如下例子：
```haskell
foldl (-) 0 [1, 2]    --=>
foldr (-) 0 [1, 2]    --=>
```
`foldl`中拿列表中最左边元素1和缺省值0进行减法运算，1作为右操作数，所以是`0-1=-1`，接着-1作为累计结果和列表下一个元素继续运算，即`-1-2=-3`，所以左折叠结果为-3。
`foldr`中拿列表中最右边元素2和缺省值0进行减法运算，2作为左操作数，所以是`2-0=2`，接着2作为累计结果和列表上一个元素继续运算，即`1-2=-1`，所以右折叠结果为-1。

通常依据将列表中元素作为左操作数还是右操作数来决定是用左折叠还是右折叠。

另外右折叠因为是从最后一个元素开始累计，所以右折叠可以计算无限列表，但是左折叠却不行。

**其他高阶函数**

一般提供函数式编程的语言都会提供`map`、`filter`、`reduce`三个函数，对于列表操作，这三个函数就像命令式语言里的`for`、`if`语句一样不可或缺。

除此之外还有另外一些高级函数也是很重要的，但是有些非纯函数式的语言可能没有提供，比如：
1. `flip`翻转函数的两个参数。
2. `max`返回两个数字中大的一个。
3. `repeat`无限循环一个数据。
4. `take`取列表前面指定个数元素等。

这里单独说明下zipWith函数。

`zipWih`会将两个列表依照函数按序合并为一个列表。
```haskell
 zipWith (*) [1,2,3] [2,3,4]    --=>[2, 6, 12]
```
`zipWith`会拿第一个列表的第一个元素`1`和第二个列表的第一个`2`元素应用`(*)`函数，并把结果作为返回列表的第一个元素，依次执行，合并的列表为`[2, 6, 12]`(1\*2=2, 2\*3=6, 3\*4=12)。


####柯里化

**柯里化**

柯里化的主要目的就是单一化函数参数，也就是通过函数可以作为值传递这种特性，将一个参数与要应用的函数打包成新的函数。通过这种方式将所有函数都封装为单一参数的形式。

比如说上面Haskell中常见的加法运算正常情况下`(+) 1 2`应该这样执行，`(+)`是函数名（haskell中加法运算符是个函数）而`1`和`2`是两个参数。而实际上haskell所有函数都是单参数的，其执行过程是函数`(+)`和参数`1`组合成一个新的函数`(+1)`，然后参数`2`在应用于函数`(+1)`。这就是将多个参数通过柯里化分割为单参数的过程。

我们用Java重写上面的例子看看：
```java
import java.util.function.IntFunction;
import java.util.function.IntUnaryOperator;

public class Main {
  public static void main(String[] args) {
    IntFunction<IntUnaryOperator> curriedAdd = a -> b -> a + b;
    IntUnaryOperator adder5 = curriedAdd.apply(5);

    System.out.println(adder5.applyAsInt(4));                 //=>9
    System.out.println(curriedAdd.apply(5).applyAsInt(6));    //=>11
  }
}
```

1. 首先声明了一个参数和返回类型都为Int的函数接口curriedAdd，在lambda表达式`a -> b -> a + b`中`a`和`b`表示参数，`a+b`表示返回值，每一个参数或返回值之间都用`->`链接。
2. 在curriedAdd的基础上应用了一个参数5，声明了一个参数和返回值都为Int的函数接口adder5。
3. adder5就是柯里化后的函数，当一个参数作用于他时，他会把默认参数5和接受的参数应用到curriedAdd然后执行加法操作返回结果。

**部分施用**

和柯里化类似，部分施用也是用来减少参数个数的，不同的是部分施用是通过提取代入一部分参数值，使一个多参数函数变为较少数目参数的函数，而不是必须转换为单参数的函数。

我们来看下下面这个`a+b+c`的函数，是如何通过部分施用将函数和一个参数打包成一个需要传入两个参数的新函数的。


```java
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
    System.out.print(partialFunction.apply(2,3));          //=>6
    System.out.print(partial(Main::add, 2).apply(3,4));    //=>9
  }
}
```

1. 首先定义一个接口TriFunction，接受三个参数T、U、V并返回R，这里R是一个函数。
2. 声明一个原始的`a+b+c`方法`add`。
3. 声明了一个返回 BiFunction接口（它和自定义的TriFunction接口类似，接受两个参数U和V并返回R）的方法partial，它接受两个参数TriFunction和T，其中TriFunction是一个函数，T是TriFunction的第一个参数。同样的的方法partial也返回一个函数，这个函数需要两个参数y和z。
4. 通过partial构造一个新的方法partialFunction，在构造这个方法时需要传入要打包的方法和它的一个参数。之后我们就可以通过打包后的函数，传入其他两个参数来等价执行add方法了。
5. 我们也可以像`partial(Main::add, 2).apply(3,4)`这样不显示的指定打包函数，像之前的`stream`操作一样连续传递参数。

总上我们还是通过打包的方式将一个需要传递三个参数的函数拆分为两步，第一次传递一个参数，第二次传递两个参数。这样通过函数打包的方式来减少传递参数的方式就是部分施用。

**总结**

最后我们还是以Haskell为例来说明，定义一个函数，接受三个参数，第一个参数表示一个区间的下限，第二个参数表示这个区间的上限，第三个参数用来判断是否在这个区间，并把比较结果以字符串方式返回。
```haskell
inRange:: Int -> Int -> Int -> String
inRange a b c
  |     a > c = "small than minimum" 
  |     b < c = "large than maximum"
  |     otherwise = "in range"
```
不用担心上面的语法，你只要记得这个inRange函数的作用就可以，我们主要来看下面几种调用：
```haskell
(inRange 1 10 5)         --=>in range
((inRange 1 10) -2)      --=>small than minimum
(((inRange 1) 10) 12)    --=>large than maximum
```

第一个调用我们打包了一个函数，这个函数传入了三个参数，这个就有点像我们在命令式编程里书写的代码，拿到三个参数后比较，然后分情况返回结果。

第二个调用我们打包了两个函数，第一个函数接受两个参数用来生成一个区间，第二个函数接受一个参数作为匹配值，这就像部分施用，我们通过打包函数的方式分组或者简化了参数个数。

第三个调用，每次传入一个参数，我们把每次传入的参数记录起来，并再最后一个函数中执行所有的判断，这就是柯里化。

当然Haskell是自动柯里化的，也就是说无论你函数的参数列表定义了多少参数，实际上执行的时候都会被抽离成单参数的形式。我这里的例子只是帮你区分部分施用和柯里化的区别。

####函数组合

如果说柯里化是将一整个函数拆分成一段一段的单个函数，然后依次传参执行。那么函数组合更像是反过来的过程，是将几个函数组合成一个新的函数然后传参执行。

柯里化是把参数传入第一个函数，执行后返回一个新的函数，然后把第二个参数应用到返回的函数，依次；
而函数组合是把参数传递给第一个函数，执行后返回一股值，然后把值传入第二个函数，依次。所以柯里化看起来是一个函数连续调用了几个参数，而函数组合是多个函数嵌套调用了一个参数。



####lambda



