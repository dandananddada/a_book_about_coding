
# 更多概念

####闭包

闭包（closure）是函数及其相关引用环境组合而成的一个整体，为了方便解释，你可以理解为闭包就是一个容器，这个容器中包含一个函数，和几个参数，函数可以像访问自己内部定义的变量一样访问容器中的参数。

先看这段代码
```
map' list = map (+3) list
map' [1,2,3]    --=>[4,5,6]
```

这是一个高阶函数，我们把列表中每一项都应用了`(+3)`函数，我们对它再封装一层，让他变成`map (+x) list`。
```
map' x list = map (+x) list
map' 3 [1,2,3]    --=>[4,5,6]
```
做了如上改进，我们让`map'`传入两个参数，一个是加运算的操作数另一个是被操作的列表，其中`map`就是一个容器，`(+x)`就是容器中的函数。接下来在`map'`得到参数`x=3`时，就变成了`map (+3) list`，我们再把`(+3)`用匿名函数的方式展开就变成了：`map (\x->x+3) list`，这时我们看到`3`变成了`(\x->x+3)`函数的一部分，而`3`本身不是函数中定义的，是通过容器`map``传入的。

这种函数自由访问容器中外部变量的特性就是闭包，整个容器就是这个函数和它外部变量的`闭包`。



####递归
在命令式编程中我们通常通过索引来遍历一个数组中的每个元素，但在函数式中我们通常会将列表分成头和尾两部分，然后通过递归尾部（将尾部分为头和尾，尾部越来越短直至为空）来访问列表中的每一个元素。

```haskell
negatedEvery:: Num a=>[a]->[a]
negatedEvery [] = []
negatedEvery (x:xs)  = -x : negatedEvery xs

negatedEvery [1,2,3,4]    --=>[-1,-2,-3,-4]
```
上面代码中`negatedEvery`用于将列表中每一个元素都转换为负数，我们看第三行中`x:xs`是将列表拆分为头部`x`和尾部`xs`，然后分别对`x`和`xs`赋值，因为头部元素是一个数值，所以可以直接取负数，而尾部元素仍旧是一个列表，所以作为一个新的列表重新应用到`negatedEvery`函数。我们看第二行中约定如果元组为空则返回空，结束。所以当我们不断递归`xs`到`negatedEvery`函数时。只有列表不是无限的终究会执行到`negatedEvery [] = []`函数结束。这是得到的列表中每个元素都执行了`x=-x`，通过递归将列表中所有元素取负数。

接下来我们用递归解决斐波那契数列问题，斐波那契数列的计算满足`F(n) = F(n-2) + F(n-1), F(0) = 1, F(1) = 1`这三条规则。

斐波那契数列本身就是典型的递归实现，我们根据计算规则可以明确三种情况：

1. n=0时值为0
2. n=1时值为1
3. n为其他值时，递归F(n-1)+F(n-2)

先用Java实现如上逻辑：

```java
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

1. 声明一个LongUnaryOperator函数接口，这个函数接口约定传入参数和返回类型都是long类型。
2. 我们用lambda表达式对输入参数x应用三元运算符，如果x值为0或者1则返回1，否则返回`F(n-1) + F(n-2)`。
3. 接下来声明一个取斐波那契数的方法，将传入参数转换为long类型应用到之前声明的LongUnaryOperator函数接口。

同理Haskell的实现方式类似：

```haskell
fibonacci::Integer->Integer
fibonacci 0 = 0
fibonacci 1 = 1
fibonacci n = fibonacci (n-1) + fibonacci (n-2)
```

####记忆

记忆是用来存储一个函数的返回值的，因为函数式的不变性，输入参数相同返回结果一定相同，这样为了避免重复结算同一个函数调用的返回结果引入了记忆机制。

通常我们会把输入参数作为key，把输出结果作为value存在一个缓存结构中（哈希或者元组，函数式用元组来存储key-value）中，在函数调用时先遍历这个缓存结构中是否存在当前调用的key，如果存在则返回缓存中的value，不存在则计算，并插入这对键值到缓存结构。

我们还是以斐波那契数列为例说明，在不使用缓存机制时斐波那契数列的计算是非常慢的。
```haskell
fibonacci 30
{- 
  832040
  (2.09 secs, 554,439,712 bytes)
-}
```
添加记忆机制后
```haskell
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
这样每次执行memoized_fib的时候都会把结果缓存起来，下次执行的时候就可以直接使用结果，因为斐波那契数列存在大量的重复运算，所以缓存的添加让执行效率有了质的改变。

来看下Java8版本的实现：
```java
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
我们重点来看`fibonacci`这个函数，它调用了`memo`的`computeIfAbsent`方法，`computeIfAbsent`是Java8对`Map`接口新增的方法，它传入两个参数，一个是键值，另一个是函数接口，当`key`（这里`key`就是`x`）值不在map中时会将`x`传入函数并把返回值以`key`为键追加到`map`中。之后我们用`lambda`表达式实现函数接口，因为表达式中没有区分`x=0`和`x=1`两种情况，所以我们直接在`map`中添加`key`为`0`和`1`的值，这样当`x=0`或者`x=1`时就不会执行函数接口了。

####惰性求值

惰性求值是指表达式不在它被绑定到变量之后就立即求值，而是在该值被取用的时候才求值，这样做除了可以提高计算性能外还能构造无限列表（因为惰性求值只关心列表中要求算的那个数，并不关心后面无限个数据的值是多少）。

某些编程语言是默认惰性求值的，比如Haskell，另外也有些语言提供了惰性求值的函数或语法，比如Java8的stream操作就是惰性求值的。

我能先看一个无限列表的例子，假设我们有这样一个需求：求所有三的倍数的数字之和
```haskell
let mod3 = filter (\x -> mod x 3 == 0) [1..]
take 5 mod3    --=>[3,6,9,12,15]
```
我们创建了一个mod3函数，这个函数过滤出所有三的倍数。之后用take先后取了前五个三的倍数，因为惰性求值的缘故，mod3只会计算到第五个元素，这就是一个对无限列表应用函数的例子。

接下来我们看看Java8中stream是如何表现出惰性求值的。
```java
public static void main(String[] args) {
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
  numbers.stream().filter(n-> {
    System.out.print(n);
    return n > 2;
  });
}
```
执行上面这段代码不会输出任何结果，说明filter内的函数并没有执行。这是因为Stream是惰性求值的，如果想得到链操作的返回结果需要在最后执行一个及时求值的方法。
```java
public static void main(String[] args) {
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
  List<Integer> results = numbers.stream().filter(n-> {
      System.out.print(n);    //=>12345
      return n > 2;
  }).collect(Collectors.toList());
}
```
和之前不同我们在Stream操作最后调用了`collect(Collectors.toList())`方法，这是一个及时求值的方法，它会把之前的表达式执行并返回结果，因此这时`System.out.print(n);`就会把`numbers`的所有元素都打印出来了。


####不相交联合体（disjoint union）

**Either**

在函数式编程中经常会遇到需要返回两种不同类型的情况，为了满足这种需求设计了不相交联合体。不相交联合体可以存储两种不同类型的中某一种类型的实例。一般会用Either类来表述这种结构，Either有一个左值和一个右值，但是左值和右值只能选择一个。

```haskell
Left "This is left value"
Right 2
```

Either类型另外一个重要的作用就是处理异常，很多函数式语言是没有异常处理机制的，因为异常的设计和函数式语言无副作用的思想是冲突的，异常会引导程序进入异常处理流程而不是期望的返回值，函数式编程是以值为根本的，所以我们可以把异常信息记录在Either类型的左值中，正确值记录在Either类型的右值中，如果返回了左值则说明函数执行时出现了异常。

```haskell
div3:: Float->Float->Either String Float
div3 x 0 = Left "Divison by zero"
div3 x y = Right (x / y)
div3 3 0    --=>Exception: divide by zero
div3 3 3    --=>1
```
上面定义了一个除以3的方法，当除数为0时，返回左值，给出错误信息`被除数为0`，其他情况下执行除运算并返回右值。

Java8本身没有提供Either类，

**Maybe/Optional**

Maybe(Java/Scala中称作Optional)类型和Either类型类似，你可以把它看做是一种简单的异常场景，它也存储两个值，一个为空，另一个为有效值。你可以认为Maybe类型只对返回结果做了失败还是成功的判断，如果失败则返回空，成功则返回计算结果的有效值。

```haskell
returnEven:: Int -> Maybe Int
returnEven a
  | a `mod` 2 == 0 = Just a
  | otherwise = Nothing
 
returnEven 2    --=>Just 2
returnEven 1    --=>Nothing
```
如上定义了一个返回偶数的函数，`returnEven`的返回类型是Maybe类型，如果为偶数则返回值（Just是Haskell对Maybe类型中有效值的包装，这里不用在意），否则返回Nothing。

Java8也提供了Optional类型用来代替null值，这样就可以用Optional代替null表示值不存在，从而避免一些不必要的麻烦。也可以用Optional来判断一个变量是否存在值。
```java
Optional emptyOptional = Optional.empty();
Optional<String> a = Optional.of("a");
System.out.print(emptyOptional.get());    //=>java.util.NoSuchElementException: No value present
System.out.print(a.get());                //=>a

```

####纯度

我之前有在介绍函数式特点的时候说过纯度（purely）及值不可变，纯度的特性有一定的优点但也存在很多问题，比如很多操作（混合运算）并非与顺序无关的，这些操作是有副作用的（输入结果相同，函数执行顺序不同，返回结果不同），因此对于纯函数式的语言要面对如何处理这样的问题（但并非所有支持函数式编程的语言都是纯函数式的）。

我们来通过一个过平衡木的例子说明Haskell是如何处理因为函数执行顺序而产生的副作用的。

规则如下：
1. 用向左倾斜和向右倾斜程度表示一个人的平衡程度，向左平衡程度用负数表示，向右平衡程度用正数表示。
2. 向左平衡程度和向右平衡程度的和的绝对值表示平衡程度值，如果平衡程度值小于3则人是平衡的。
3. 人每向前移动一步，则随机追加左右平衡程度值，计算平衡程度值。将平衡程度值传给下一步。

```haskell
type Rate = Int
type Balance = (Rate, Rate)

toLeft:: Rate->Balance->Balance
toLeft n (left, right) = (left-n, right) 

toRight:: Rate->Balance->Balance
toRight n (left, right) = (left, right+n) 
```
我们用Rate表示平衡倾斜程度，用Balance表示平衡程度，其中left指向左倾斜程度，right指向右倾斜程度。这样我们定义了向左倾斜toLeft和向右倾斜toRight，两个函数都要求输入当前平衡程度，及平衡倾斜程度，然后返回新的平衡程度。

我们做如下调用，向左倾斜1 -> 向右倾斜1 -> 向左倾斜2
```haskell
toLeft 2 (toRight 1 (toLeft 1 (0,0)))    --=>(-3, 1)
```
这是没有问题的，我们在这个基础上继续， 向右倾斜1 -> 向左倾斜3 -> 向右倾斜2
```
toRight 1 (toLeft 3 (toRight 2 (-3,1)))    --=>(-6, 4)
```
这是返回的结果仍旧是保持平衡的：`|-6+4|<3`。但实际上在向左倾斜3的时候已经失去平衡摔倒了，所以是不可能再向右倾斜2恢复到平衡状态的（此时人已经掉下平衡木了）。

那么我们需要在失去平衡的时候返回一个失败信息，并且这个信息会一致传递下去。

我们做如下修改
```haskell
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
我们对要传递给下一步的平衡状态包装为Maybe类型，如果失去平衡则返回Nothing，否则返回Just。这样当我们下一步接受到Nothing的时候只会传递Nothing。

为了让一个值应用到函数并返回一个包装类型（Maybe在haskell中是一个函子）我们要用到monad（就是下面的`>>=`函数）。

```
return (0,0) >>= toLeft 2 >>= toRight 1 >>= toLeft 1      --=>Just(-3,1)
return (-3,1) >>= toRight 1 >>= toLeft 3 >>= toRight 2    --=>Nothing
```
可以看到这回返回了“摔倒”，同时monad不仅将值应用与函数返回包装类型，同时还提供了函数的链式调用，这种写法更加干净易懂。

另外，针对`toRight 1 (toLeft 3 (toRight 2 (-3,1)))`这种嵌套调用，我们也可以定义一个`-:`方法实现函数的链式调用。
```haskell
x :- f = f x
(0,0) -: toRight 1 -: toLeft 3 -: toRight 2    --=>(-3,3)
```