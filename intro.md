
# 介绍

treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data

 It is a declarative programming paradigm, which means programming is done with expressions[1] or declarations[2] instead of statements
 
  In functional code, the output value of a function depends only on the arguments that are input to the function, so calling a function f twice with the same value for an argument x will produce the same result f(x) each time. 
  
  
  函数式编程  程序是求表达式的值
 
Functional programming typically avoids using mutable state.
Functional programming requires that functions are first-class

####纯度不变性




####闭包
闭包是函数及其相关引用环境组合而成的一个整体，为了方便解释，你可以理解为闭包也是一个函数，只是这个函数所传入的参数不是直接使用，而是应用到函数内部嵌套的一个函数上。

先看这段代码
```
map' list = map (+3) list
map' [1,2,3]    --=>[4,5,6]
```
这是一个高阶函数，我们把列表中每一项都应用了`(+3)`函数，设想为了更方便使用能不能以再封装一层，让他变成`map (+x) list`。
```
map' x list = map (+x) list
let map'' = map' 3
map'' [1,2,3]    --=>[4,5,6]
```
做了如上改进，我们让`map'`传入两个参数，一个作为加运算的操作数，另一个作为要映射求值的列表。然后我们定义了一个新的函数`map''`并传入了操作数参数`3`，这时`map''`就是组合了一个函数，和这个函数引用环境的一个闭包（当然我们也可以传入其他操作数，组合一个其他的闭包`map''' = map' 3`）。这时在`map''`中`x`就被映射为数值`3`和函数作为一个整体提供调用了。


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
      long startTime = System.currentTimeMillis();
      System.out.print(fibonacci(30));
      long endTime = System.currentTimeMillis();
      long duration = ((endTime - startTime));
      System.out.print("\n" + duration + "milliseconds");

  }
}
```