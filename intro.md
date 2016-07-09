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
negatedEvery[] = []
negatedEvery (x:xs)  = -x : test xs
```