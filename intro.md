
# 介绍

treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data

 It is a declarative programming paradigm, which means programming is done with expressions[1] or declarations[2] instead of statements
 
  In functional code, the output value of a function depends only on the arguments that are input to the function, so calling a function f twice with the same value for an argument x will produce the same result f(x) each time. 
  
  
  函数式编程  程序是求表达式的值
 
Functional programming typically avoids using mutable state.
Functional programming requires that functions are first-class




####递归
在命令式编程中我们通常通过索引来遍历一个数组中的每个元素，但在函数式中我们通常会将列表分成头和尾两部分，然后通过递归尾部（将尾部分为头和尾，尾部越来越短直至为空）来访问列表中的每一个元素。

```haskell
negatedEvery:: Num a=>[a]->[a]
negatedEvery [] = []
negatedEvery (x:xs)  = -x : negatedEvery xs

negatedEvery [1,2,3,4]    --=>[-1,-2,-3,-4]
```
上面代码中`negatedEvery`用于将列表中每一个元素都转换为负数，我们看第三行中`x:xs`是将列表拆分为头部`x`和尾部`xs`，然后分别对`x`和`xs`赋值，因为头部元素是一个数值，所以可以直接取负数，而尾部元素仍旧是一个列表，所以作为一个新的列表重新应用到`negatedEvery`函数。我们看第二行中约定如果元组为空则返回空，结束。所以当我们不断递归`xs`到`negatedEvery`函数时。只有列表不是无限的终究会执行到`negatedEvery [] = []`函数结束。这是得到的列表中每个元素都执行了`x=-x`，通过递归将列表中所有元素取负数。

接下来我们用递归解决斐波那契数列问题，斐波那契数列是满足`F(n) = F(n-2) + F(n-1), F(1) = F(0) = 1`这两条规则的数列。

```java
import java.util.function.IntUnaryOperator;
class Recursive {
  static IntUnaryOperator func = x -> (x == 1 || x == 0) ? 1 : Recursive.func.applyAsInt(x -1) + Recursive.func.applyAsInt(x - 2);

  public static int fibonacci(int n){
      return func.applyAsInt(n);
  }
}

public class Main {

    public static void main(String[] args) {
        System.out.print(Recursive.fibonacci(10));
    }
}
```