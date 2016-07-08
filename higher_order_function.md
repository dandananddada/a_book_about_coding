# 高阶函数

在介绍函数时已经说过，高阶函数必须至少满足以下两个条件之一：
1. 传入参数为函数类型
2. 返回值为函数类型

简单的说函数类型可以像其他数据类型一样当做值在函数中传递。

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
-- [4, 5, 6]
```

