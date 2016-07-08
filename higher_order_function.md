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

    System.out.println(adder5.applyAsInt(4));    //=>9
    System.out.println(adder5.applyAsInt(6));    //=>11
  }
}
```

1. 首先声明了一个参数和返回类型都为Int的函数接口curriedAdd，在lambda表达式`a -> b -> a + b`中`a`和`b`表示参数，`a+b`表示返回值，每一个参数或返回值之间都用`->`链接。
2. 在curriedAdd的基础上应用了一个参数5，声明了一个参数和返回值都为Int的函数接口adder5。
3. adder5就是柯里化后的函数，当一个参数作用于他时，他会把默认参数5和接受的参数应用到curriedAdd然后执行加法操作返回结果。


柯里化和部分施用的作用都是用来调节函数的参数个数。
不同点是柯里化一般用来将多参数函数转换为一连串单参数函数，它测中于这个变换的过程。而部分施用是通过提取代入一部分参数值，使一个多参数函数变为较少数目参数的函数。

柯里化的结果是返回链中的下一个函数，而部分施用是把参数的取值绑定到用户操作中提供的具体值上，从而减少函数的参数个数。