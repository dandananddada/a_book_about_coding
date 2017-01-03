
# 其他概念

####闭包

闭包（closure）是函数及其相关引用环境组合而成的一个整体，为了方便解释，你可以理解为闭包就是一个容器，在这个容器中包含一个函数，和几个参数，函数可以像访问自己内部定义的变量一样访问容器中的参数。

先看这段代码

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

做了如上改进，我们让map'传入两个参数，一个是加法运算的操作数另一个是被操作的列表，其中map就是一个容器，(+x)就是容器中的函数。接下来在map'得到参数x为3时，就变成了`map (+3) list`，我们再把(+3)用匿名函数的方式展开就变成了：`map (\x->x+3) list`，这时我们看到3变成了函数`(\x->x+3)`中的一部分，而3本身不是函数中定义的，而是通过容器map'传入的。

这种函数自由访问容器中外部变量的特性就是闭包，整个容器就是这个函数和它外部变量的`闭包`。

####递归

在命令式编程中我们通常是通过索引来获取数组中的元素，但在函数式中我们通常会将列表分成头和尾两部分，然后通过递归尾部（将尾部再次分为头和尾，尾部越来越短直至为空）来访问列表中的每一个元素。

```haskell
--Haskell 通过递归将列表中元素取反

negatedEvery:: Num a=>[a]->[a]
negatedEvery [] = []
negatedEvery (x:xs)  = -x : negatedEvery xs

negatedEvery [1,2,3,4]    --[-1,-2,-3,-4]
```

上面代码中函数negatedEvery作用是将列表元素取反。

首先我们看第三行，x:xs是将列表拆分为头部x和尾部xs，然后分别对x和xs赋值。此时头部元素是一个数值，所以可以直接对其取反，而尾部元素仍旧是一个列表，所以尾部将作为一个新的列表重新应用到negatedEvery函数。

然后我们看第二行，这里表示如果元组为空则返回空。也就是说当我们不断递归xs到negatedEvery函数时，只要列表不是无限的终究会执行到`negatedEvery [] = []`，此时列表将执行`[] = []`退出递归，结束函数执行。最终返回一个所有元素都执行了取反运算的列表。

接下来我们用递归来解决斐波那契数列问题，斐波那契数列满足如下计算规则：

1. F(0) = 1
2. F(1) = 1
3. F(n) = F(n-2) + F(n-1)

斐波那契数列本身就是典型的递归实现，我们根据计算规则可以明确三种情况：

1. n=0时值为0
2. n=1时值为1
3. n为其他值时，递归F(n-1)+F(n-2)

先用Java实现如上逻辑：

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

同理Haskell的实现方式类似：

```haskell
--Haskell 实现斐波那契数列

fibonacci::Integer->Integer
fibonacci 0 = 0
fibonacci 1 = 1
fibonacci n = fibonacci (n-1) + fibonacci (n-2)
```

**尾调用**

尾调用是指一个函数里的最后一个动作是一个函数调用，这个调用的返回将作为函数的返回值返回。如果函数在最后调用了自身，则称作尾递归。

通常情况下函数在调用时会记录当前调用位置以及内部变量等信息，但是尾调用函数本身就是函数的返回值，所以记录尾调用函数的调用位置和内部变量等信息是没有必要的。针对尾调用，不记录调用位置及内部变量等信息就可以减少栈空间的使用。

单纯的尾调用即使存储了不必要的调用信息也不会浪费太多栈内存，但是如果是尾递归调用的话就有区别了，我们来对比看下记录调用信息和不记录调用信息两种情况下栈的变化。

先看下这段没采用尾调用代码。

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
每次调用需要记录函数调用位置，以及内部变量，然后再依次求值，释放对应空间。

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
再Haskell中尾递归优化是默认支持的，而像Java、Ruby、Python等这些面向对象语言，解释器并没有针对尾递归调用进行优化处理，不过Java8提供函数式支持后有针对尾递归进行优化，ES6标准也规定了JavaScript语言必须实施尾递归优化。也就是说在这些后续支持了尾递归优化的语言，在使用尾递归处理问题时不会像以前一样再出现栈溢出的问题了。

####记忆

记忆的作用是用来存储一个函数的返回值，因为函数式的不变性，输入相同的参数必然返回相同的计算结果，正是为了避免重复计算同一个函数调用而引入了记忆机制。

通常我们会把输入参数作为key，把输出结果作为value存在一个缓存结构中（哈希或者元组，函数式用元组来存储key-value）中，在函数调用时先遍历这个缓存结构中是否存在当前调用的key，如果存在则返回缓存中的value，不存在则计算，并插入这对键值到缓存结构。

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

来看下Java8版本的实现：

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

我们重点来看fibonacci这个函数，它调用了memo的computeIfAbsent方法，这里computeIfAbsent是Java8对Map接口新增的方法，它传入两个参数，一个是键值，另一个是函数接口。当key（这里key就是x）值不在map中时会将key作为键，以key为参数应用到函数运算得到的返回值作为value追加到map中。

最后说明下因为lambda表达式中没有区分当x=0和x=1这两种特殊情况，所以我们干脆直接在map中添加key为0和1的值，这样当x=0或者x=1时就避开了错误的函数调用。

####惰性求值

惰性求值是指表达式不会在它被绑定到变量之后就立刻求值，而是在该值被真正使用的时候才求值，惰性求值有两个优点：

1. 可以提高计算性能
2. 能构造无限列表（因为惰性求值只关心列表中要求算的那个数，并不关心后面是否还有其他数据）。

某些编程语言默认就是惰性求值的，比如Haskell，另外也有些语言提供了惰性求值的函数或语法，比如Java8的stream操作就是惰性求值的。

我们先通过haskell看一个无限列表的例子。

假设我们有这样一个需求：求所有三的倍数的数字之和

```haskell
--Haskell 惰性求值实现无限列表

let mod3 = filter (\x -> mod x 3 == 0) [1..]
take 5 mod3    --[3, 6, 9, 12, 15]
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
和之前不同我们在Stream操作最后调用了collect(Collectors.toList())方法，这是一个及时求值的方法，它会把之前的表达式执行并返回结果，因此这时`System.out.print(n);`就会把numbers的所有元素都打印出来了。


####不相交联合体（disjoint union）

**Either**

在函数式编程中经常会遇到需要返回两种不同类型的情况，为了满足这种需求设计了不相交联合体。不相交联合体可以存储两种不同类型中的某一种类型。一般会用Either类来表述这种结构，Either有一个左值和一个右值，但是左值和右值只能选择一个。

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

Maybe(Java/Scala中称作Optional)类型和Either类型类似，你可以把它看做是一种简单的异常场景，它也存储两个值，一个为空，另一个为有效值。你可以认为Maybe类型只对返回结果做了失败还是成功的判断，如果失败则返回空，成功则返回计算结果的有效值。

```haskell
--Haskell Maybe类型

returnEven:: Int -> Maybe Int
returnEven a
  | a `mod` 2 == 0 = Just a
  | otherwise = Nothing
 
returnEven 2    --Just 2
returnEven 1    --Nothing
```
如上定义了一个返回偶数的函数returnEven，函数返回了一个Maybe类型。如果计算结果为偶数则返回值（Just是Haskell对Maybe类型中有效值的包装，这里不用在意），否则返回Nothing。

Java8也提供了Optional类型用来代替null值。通过Optional代替null表示值不存在，可以避免一些不必要的麻烦。

Optional也可以用来判断一个变量是否存在值。

```java
//Java Optional使用

Optional emptyOptional = Optional.empty();
Optional<String> a = Optional.of("a");

System.out.print(emptyOptional.get());    
//java.util.NoSuchElementException: No value present

System.out.print(a.get());                
//a

```

####纯度

我之前有在介绍函数式特点的时候有提及过纯度（purely）这个概念，纯度的特性有一定的优点但也存在很多问题，比如很多操作（混合运算）并非是顺序无关的，这些操作是有副作用的（输入结果相同，函数执行顺序不同，返回结果不同），因此对于纯函数式的语言而言就要面对一些棘手的问题（但并非所有支持函数式编程的语言都是纯函数式的）。

我们来通过一个过平衡木的例子来说明Haskell是如何处理因函数执行顺序而产生的副作用的。

平衡木游戏的规则如下：

1. 用向左倾斜和向右倾斜的程度表示一个人的平衡程度，向左倾斜用负数表示，向右倾斜用正数表示
2. 向左平衡程度和向右平衡程度的和的绝对值表示平衡值，如果平衡程度值小于3则人是平衡的
3. 人每向前移动一步，则随机追加左右倾斜值，计算平衡值。将平衡值传给下一步

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

我们做如下调用，向左倾斜1 -> 向右倾斜1 -> 向左倾斜2

```haskell
--Haskell 平衡木游戏
toLeft 2 (toRight 1 (toLeft 1 (0,0)))    --(-3, 1)
```
到目前为止是没有问题的，我们在这个基础上继续， 向右倾斜1 -> 向左倾斜3 -> 向右倾斜2

```haskell
toRight 1 (toLeft 3 (toRight 2 (-3,1)))    --(-6, 4)
```
这时返回的结果仍旧是保持平衡的：`|-6+4|<3`。但实际上在向左倾斜3的时候就已经失去平衡而摔倒了，所以是不可能再向右倾斜2恢复到平衡状态的（此时人已经掉下平衡木了）。

那么我们需要在失去平衡的时候返回一个失败信息，并且这个信息会一致传递下去。

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
可以看到这回返回了“摔倒”，同时monad不仅将值应用到了函数包装类型，同时还提供了函数的链式调用，这种写法更加干净易懂。

另外，针对`toRight 1 (toLeft 3 (toRight 2 (-3,1)))`这种嵌套调用，我们也可以定义一个`-:`方法实现函数的链式调用。
```haskell
--Haskell 实现链式调用

x :- f = f x
(0,0) -: toRight 1 -: toLeft 3 -: toRight 2    --(-3,3)
```
通过上面的例子可以看到为了处理产生副作用的调用，Haskell提供了诸如Maybe、Monad这样的概念。而对于允许副作用的语言处理其相同的问题就简单容易的多，这也是纯函数式对比命令式的一个缺点。
