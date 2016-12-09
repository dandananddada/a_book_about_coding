# 集合类型

这里我准备介绍的集合类型主要指命令式语言下常用的数组类型和声明式语言下常用的列表类型。因为语言本身设计上的缘故，通常静态类型语言所使用的集合多是定长的，也就是不能动态拓展，而动态类型语言相对是随时可以拓展的。同样弱类型语言的集合类型是异质的，也就是数组元素可以不同数据类型。这里因为Java语言设计上的缘故，提出了数组、链表、集的概念，这些概念通常为集合类型，所以我也用集合类型来统一称谓了。

接下来我们先看下最基本的数组类型，然后以Java语言为主说明如何通过集合类型完善Java数组对比其他语言所不具备的功能。

####数组

在Java中数组类型是同质定长的，也就是说Java的数组是不支持不同类型数据混合的，且其长度是在实例化时指定的。

```java
//Java 数组类型

int[] a = { 1, 2, "three" };
//error incompatible types: String connot be converted to int

int[] b = new int[10];
b[10] = 11;
//exception in thread main java.lang.ArrayIndexOutOfBoundsException
```
可以看出声明一个异质的数组会报编译时错误，而对超出数组索引的元素赋值会报运行时异常（数组越界异常）。

于此不同JavaScript和Ruby的数组就是异质变长的，不过JavaScript似乎并不提供无序数组的概念，也就是说数组是有且只有顺序的。
```javascript
//JavaScript 数组类型

var a = [1, 2, 3];
console.log(a.length);    //3

a[3] = 4;
console.log(a.length);    //4

a[6] = 7;
console.log(a);           //[1, 2, 3, 4, undefined, undefined, 7]
console.log(a.length);    //7
```
在数组方面Ruby和JavaScript表现是一致的。
```ruby
#Ruby 数组类型

a = [1, 2, "three"]
puts a.length            #3

a[3] = 4         
puts a.length            #4

a[6] = 7
puts a                   #[1, 2, 3, 4, nil, nil, 7]
puts a.length            #7
```
其实Ruby和JavaScript支持异质数组是因为Ruby中一切都是对象，而JavaScript中一切都是原型，采用相同的逻辑，如果Java中定义类型为基类（Object）的数组时，同样也是支持异质结构的。
```java
//Java 异质数组

Object[] o = { "string", 1, false };
System.out.println(o[0]);        //string
System.out.println(o[1]);        //1
System.out.println(o[2]);        //false
System.out.println(o.length);    //3
```

而关于Java如何实现变长数组会在介绍了数组长度相关的几个概念后再详细说明。

**数组长度**

上面已经说过Java的数组是定长的，所以对长度为3的数组进行a[4]=5这赋值操作会报数组越界的异常。

而JavaScript和Ruby中数组是变长的，也就是你可以对长度为3的数组执行类似a[4]=5这样的赋值操作，但是不同的是Ruby会把中间遗漏掉的元素用nil补齐，而JavaScript则直接跳过，因为JavaScript本身没有数组类型，数组实际上是Key值为数字的Object，你可以给一个Object定义任何键值，所以跳过数组索引顺序赋值本身也是合法的（这样的数组叫做稀疏数组）。
```ruby
#Ruby 变长数组

a = [1, 2, 3]
a[5] = 6

puts a           #[1, 2, 3, nil, nil, 6]
puts a.length    #6
```
```javascript
//JavaScript 变长数组

var a = [1, 2, 3];
a[5] = 6;

console.log(a);           //[1, 2, 3, , , 6]
console.log(a.length);    //6
```
与Ruby不同，JavaScript甚至支持对数组长度进行赋值，当设置的length值大于数组长度时，JavaScript用undefined进行填充，形成一个相同长度的数组。假如设定的length值小于数组实际长度，数组就会进行截断处理。
```javascript
//JavaScript 对数组长度赋值

var a = [1, 2, 3];
console.log(a.length);    //3

a.length = 5;
console.log(a[4]);        //undefined 
console.log(4 in a);      //false
console.log(a);           //[1, 2, 3, , ,], 这时a是一个稀疏数组

a.length = 2;      
console.log(a[3]);        //undefined
console.log(3 in a);      //false
console.log(a);           //[1, 2]
```

**稀疏数组**

在JavaScript初始化一个数组时，跳过某些元素不指定具体值，这样的数组就是稀疏数组。当遍历这个数组时会跳过这些没有值的元素（这些元素缺省值为undefined，但是不存在对应的key）。
```javascript
//Javascript 稀疏数组

var a = [, , 3];
a.forEach(function(v){
  console.log(v)
});
//3
```
如果我们初始化的时候指定了缺省值，这时数组就不是稀疏数组了。
```javascript
//Javascript 指定数组元素为undefined

var a = [undefined, 1]
,   b = [, 1];

console.log(0 in a);    //true
console.log(0 in b);    //false
Object.keys(a);         //["0", "1"]
Object.keys(b);         //["1"]
```
因为JavaScript中数组本身是key值为数字的Object，所以可以用in来判断对象是否存在键。上面的例子可以发现a[0]是存在的，而b[0]是不存在的，所以a是稠密数组，而b是稀疏数组。

因为Java数组默认都是有缺省值的，所以Java语言不存在稀疏数组的概念。

```java
//Java 数组元素缺省值
int a[] = new int[3];
a[2] = 3;

for(int i=0; i<a.length; i++){
  System.out.println(a[i]);
}
//003
```
Java中整型数组的缺省值为0，字符型数组缺省值为null。

不过在Android SDK中提供了稀疏数组类SparseArray，它是用来处理稀疏矩阵的，和JavaScript中定义的稀疏数组是两种概念，简单说明下稀疏矩阵就能清楚的分辨出两者的区别了。

假设我们用一个二维数组来表示一个5*5矩阵
```
1 0 0 0 0
0 2 0 0 0
0 7 0 0 0
0 0 0 0 9
0 0 0 0 0
```
上面这个矩阵中有效的数值其实只有4个，剩下的21个都是空，这时用一个二维数组来存储这个矩阵会浪费大量的存储空间，我们考虑通过记录有效数据来压缩这个矩阵，如下：
```
5 5 4    //原矩阵的 行 列 有效数字
1 1 1    //第一个元素所在 行 列 及数值
2 2 2    //第二个元素所在 行 列 及数值
3 2 7    //第三个元素所在 行 列 及数值
4 5 9    //第四个元素所在 行 列 及数值
```
这样就把一个5*5的矩阵压缩为3*4的矩阵了，这个压缩后的矩阵就是稀疏矩阵，因为采用了数组类型所以笼统的就叫做稀疏数组了（注意Java SE本身没有特别定义稀疏数组的概念）。


#### 链表

为了方便理解你可以把链表看做是一个变长的数组，JavaScript和Ruby所提供的数组本身就是变长的，那么抛开这两门语言，单独说下Java的链表。

**链表**

List是Java的一个接口，它有三个实现类ArrayList、LinkedList和Vector，List允许你根据需求动态的增加数组元素。

最常用的List实现类是ArrayList，只有在频繁的向数组头部插入元素，或者通过遍历数组删除元素时才会用LinkedList。而Vectory和ArrayList的不同在于Vector是同步的,在多线程编程时使用Vector更加稳妥。我们来看下通过ArrayList追加元素的例子。

```java
//Java 链表

/* 这里需要引入 List 接口 和 ArrayList 实现类 */
import java.util.List;
import java.util.ArrayList;
class Demo {
  public static void main(String args[]){
      List<Integer> list = new ArrayList<Integer>();
      list.add(1);
      list.add(2);
      list.set(0, 3);
      System.out.println(list.get(0));    //3
      System.out.println(list.get(1));    //2
      System.out.println(list.size());    //2
   }
}
```
`ArryList.set(index, value)`可以将索引为index的元素修改为value，需要注意你不能用这个方法对长度为3的链表取第四个元素赋值，这样做会和数组一样抛出数组越界异常。
```java
//Java 通过ArrayList的add方法拓展数组元素

List<Integer> list = new ArrayList<Integer>();
list.add(1);
list.set(3, 3);
//Exception in thread "main" java.lang.IndexOutOfBoundsException
```
另外和数组不同List的不是存储在一段连续的空间的（注意数组是存储在一段连续空间的），同时数组必须初始化大小，而List可以不初始化大小，List可以删除和插入元素，但是数组不可以。不过相比之下数组效率比List高一些。

**集**

除了List外Java还提供了Set，和List不同的是Set不允许重复元素。Set也是一个接口，实现类有HashSet、TreeSet、LinkedHashSet。HashSet是最常用的，它通过散列（hash）来存储节点，因此是无序的。TreeSet通过红黑树存储节点，所以元素的顺序按照红黑树的遍历顺序计算，效率不如HashSet。LinkedHashSet从名字可以看出，它的节点是由一个链表串联起来的，所以元素的顺序跟插入顺序有关。

同样以最常用的HashSet为例对比链表来说明集更强调元素的唯一。
```java
//Java 集

import java.util.*;
class Demo {
  public static void main(String args[]){
    List<Integer> list = new ArrayList<Integer>();
    list.add(1);
    list.add(1);
    list.add(2);
    System.out.println(list);           //[1, 2, 2]
    System.out.println(list.size());    //3

    Set<Integer> set = new HashSet<Integer>();
    set.add(1);
    set.add(1);
    set.add(2);
    System.out.println(list);          //[1, 2]
    System.out.println(set.size());    //2
  }
}
```

注意集本身是用来存储不重复数据集的，通常的操作是判断元素是否存在于集中，很少直接获取指定元素，所以集本身不提供get方法。关于集不提供get方法你也可以理解为因为集不像数组或链表一样按索引排序，同时也不像散列一样可以通过键来定位元素，所以集无法定位到具体某一个元素，也就不提供get方法了。

如果说链表是一个变长的数组，那么集就可以看做是一个无序的链表。为了方便理解，我将List和Set划分到数组类型下。至于究竟是用Array还是List，是HashSet还是LinkedHashSet就取决于你具体面对的数据格式和业务场景了，如果你不够熟悉这些类，那么ArrayList是最通用的了，就像Ruby和JavaScript并没有对数组进行变长定长、有序无序这样的划分，但是不得不说它们的数组类型本身就足够灵活了，Java的ArrayList也是这样的工具类，所以不妨试试看吧。


####列表

介绍完命令式语言中常用的数组类型后，接下来说下通数组类型类似，在声明式语言中起着重要作用的列表类型。

因为大多数函数式语言都是静态类型的，所以列表基本上也都是非异质的。不同于之前的语言，函数式语言针对列表提供了更丰富的操作，比如你可以通过一些高阶函数直接过滤列表元素并进行运算、操作，也可以使用更加丰富的API对列表进行合并、截断，正因为需要支持灵活的操作，列表本身也是变成的。

我们来看下Haskell针对列表提供的一些操作。

合并列表
```haskell
--Haskell 合并列表

[1, 2, 3] ++ [4, 5]         
--[1, 2, 3, 4, 5]
```
对列表所有元素取绝对值
```haskell
--Haskell 操作列表

map abs [1, -2, 3]          
--[1, 2, 3]
```
过滤出大于三的元素
```haskell
--Haskell 过滤列表

filter (>3) [1, 4, 7, 2]    
--[4, 7]
```
取出列表中元素值小于5，且能被三整除的元素并取反
```haskell
--Haskell 列表推导

[ x | -x <- [1, 3, 5, 6], x `mod` 3 == 0, x < 5]    
---3
```
取得数组第三个元素
```haskell
--Haskell 列表取值

head . tail .tail $ [1,2,3,4,5]    
--3
```

在函数式语言中高阶函数配合列表操作可以以简短的代码完成复杂的任务，具体示例我会在函数式编程中详细说明。
