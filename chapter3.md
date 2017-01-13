# 第三章 与值类型相对的引用类型

继第一章第五节和第二章，这一章主要说明数据类型中的引用类型，这里汇总大多数语言中常用的引用类型，进行了归类划分，主要区分为集合类型，其中集合类型分为按顺序索引的数组及按键索引的散列，函数类型和自定义类型，这些介绍中混合了命令式语言和函数式语言两种范式下相关的引用类型。

1. 介绍命令式下的数组、链表及声明式下的列表。
2. 介绍命令式下的散列、枚举及声明式下的元组。
3. 介绍命令式和声明式两个范式下和函数相关的概念。
4. 介绍了命令式下的结构体、类和声明式下的类型。



### 集合类型

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

** 集 **

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

### 散列类型

散列是一种由键值对构成的数据类型，拿他与数组比较的话，可以理解为数组是以升序数字为键的散列，散列是索引为字符串的数组。

通数组类似，我会先说明命令式语言下的散列类型，然后再介绍声明式语言下同等功能的元组。

#### 散列

在Ruby中散列是作为基本数据类型定义的，在JavaScript中对象类型就是散列结构，因此也可以认为散列是JavaScript的基本数据类型，最后Java中散列同List、Set类似通过Map接口实现。

```ruby
#Ruby 散列

language = { "name"=> "ruby", "type"=> "dynamic" }
puts language["name"]    #ruby
```

```javascript
//JavaScript 对象

var language = { name: "javascript", type: "dynamic" }
console.log(language.name);    //javascript
```

```java
//Java 散列

Map<String, String> language = new HashMap<String, String>(){
  {
    put("name", "java");
    put("type", "static");
  }
};
System.out.println(language.get("name"));    //java
```

注意散列和数组一样，是引用类型，在使用时要特别小心，如果不想破坏原引用最好进行深度拷贝。

```ruby
#Ruby 散列属于引用类型 

a = language
a["name"] = "Ruby"

puts language["name"]    #Ruby
```

```javascript
//JavaScript 散列属于引用类型

var a = language;
a.name = "JavaScript";

console.log(language.name);    //JavaScript
```

```java
//Java 散列属于引用类型

Map<String, String> a = new HashMap<String, String>();
a = language;
a.put("name", "Java");

System.out.println(language.get("name"));    //Java
```

**键值改变**

在使用散列时保证键值不可修改是很重要的，如果我们通过变量定义散列的键，就会存在一种额外的风险。

我们以JavaScript为例说明。

```javascript
//Javascript 变量作为属性名

var name = "name"
,   language = {};

language[name] = "javascript";
console.log(language, language[name]);
//{ name: 'javascript' } 'javascript'

name = "rename"
console.log(language, language[name]);
//{ name: 'javascript' } undefined
/* 这里因为不存在键rename，所以language[name]返回undefined */
```

从上面的例子可以看出如果用变量作散列的键，当变量发生改变时，再用这个变量获取散列的值时会出现问题。

在Ruby中可以使用符号来作为散列的键，符号是不可修改的，这样就避免了如上问题。

```ruby
#Ruby 符号作为散列的键
language = { :name=> "ruby", :type=> "dynamic" }
```

Ruby中用符号作为键还有另外一个优势，因为符号是同一个引用，所以这样的散列相比字符串索引运算性能要高。

```ruby
#Ruby 符号做键与字符串做键查询性能对比

require 'benchmark/ips'
STRING_HASH = { "foo" => "bar" }
SYMBOL_HASH = { :foo => "bar"  }
Benchmark.ips do |x|
  x.report("string") { STRING_HASH["foo"] }
  x.report("symbol") { SYMBOL_HASH[:foo]  }
end

# Warming up --------------------------------------
# string   102.788k i/100ms
# symbol   116.332k i/100ms
# Calculating -------------------------------------
# string      3.799M (± 6.5%) i/s -     18.913M
# symbol      7.810M (± 6.8%) i/s -     38.855M
# 可以看出符号的运算速度是字符串的两倍多。
```

同理Java下键值通过变量来存储，发生改变后也会返回null值。

```java
//Java 变量作为散列的键

public static void main(String args[]){
  Map<String, String> language = new HashMap<String, String>();
  String name = "name";
  language.put(name, "Java");
  System.out.println(language +" "+ language.get(name));  
  //{name=Java} Java

  name = "rename";
  System.out.println(language +" "+ language.get(name));  
  //{name=Java} null
}
```

可以将变量修饰为final, 或者用匿名内部类传入形参不可变的特性来保证键值不可修改。

```java
//Java 匿名内部类保证键值不可修改

public static void main(String args[]){
  String name = "name";
  Map<String, String> language = new HashMap<String, String>(){
    {
      put(name, "java");     
      put("type", "static");
    }
  };

  name = "NAME";
  //error: local variables referenced from a inner class must be final or effectively final
}
```

上面例子可以看出如果你试图修改匿名内部类中引用的变量name，程序在编译时就会抛出一个编译错误。

#### 枚举

枚举类型可以看做是一个键为常量，值为数字类型（默认值从0开始自增长）的散列，其主要用来存储一些列性质相同的常量元素。

目前主流语言都加入了枚举类型：

```c
//C 枚举

enum season {
  Spring = 0,
  Summer = 1,
  Autumn = 2,
  Winter = 3;
};
```

```java
//Java v1.5后引入枚举

public enum Season {  
  SPRING, SUMMER, AUTUMN, WINTER;  
}

```
在分支语句中使用枚举能提高代码的可读性：

```java
//Java 分支语句中使用枚举

public static void main(String args[]){
  Color color = Color.RED;

  switch(color){
    case RED:
      System.out.println("red");
      break;
    case GREEN:
      System.out.println("green");
      break;
    case YELLOW:
      System.out.println("yellow");
      break;
  }
}
```
枚举的另外一个主要用途是充当静态常量，对比静态常量枚举类型更加安全。

```java
//Java 常量在使用时需要合法性验证

interface Color{
  static final int RED = 1;
  static final int GREEN = 2;
  static final int YELLOW = 3;
}

public class Main{
  public Boolean isRed(int i){
    //为保证参数有效，需要合法性验证
    if(i < 1 || i > 3)  System.out.println("error code");

    if(i == 1)  return true;
    else        return false;
  }
  public static void main(String args[]){
    Main main = new Main();
    if(main.isRed(Color.RED))  System.out.println("red");    //red
  }
}
```
```java
//Java 枚举类型比常量更加安全

enum Color{
  RED,GREEN,YELLOW;
}
public class Main{
  public Boolean isRed(Color color){
    //枚举类型限定了输入参数，保证了合法性输入
    if(color.ordinal() == 0)  return true;
    else                      return false;
  }
  public static void main(String args[]){
    Main main = new Main();
    Color red = Color.RED;
    if(main.isRed(red))  System.out.println("red");    //red
  }
}
```
合理的通过枚举替换常量的使用是一种良好的编程习惯，当然抛去枚举的概念不用也不会产生什么严重的问题，枚举的使用与否也是个人习惯的一种选择，这里只是讲到散列类型联系到枚举，简单进行说明。


#### 元组

除了列表外函数式语言中另外一种常用的数据结构就是元组了，元组和列表的区别在于元组允许异质类型，不过元组多数都是定长的。

鉴于元组如上的性质，一般元组用来存储数据，而不处理数据，当你在一个函数中需要返回多种类型的数值时，元组就是一个很好的包装。

我们来看下如何定义元组，并从中取值。

定义一个元组
```haskell
--Haskell 元组
(1, "two", "3", "three")
```

元组是定长的，不支持列表一样的合并运算

```haskell
--Haskell 元组无法合并

(1, "two", 3) ++ ("three", 4)    
--Couldn't match expected type
```

元组不支持列表一样的函数应用，因此无法处理数据
```haskell
--Haskell 元组无法处理数据

map (+3) (1, 2, 3)    
--Couldn't match expected type
```

定义一个函数third，取得元组的第三个值
```haskell
--Haskell 从元组中获取元素

third:: (a, b, c, d) -> c
third (_, _, c, _) = c

third (1, "two", "3", "three")    
--3
```

大多数函数式语言都针对二元组（有两个元素的元组）提供了常用的函数来简化了元组的访问。

```haskell
--Haskell 二元组

fst ("name", "haskell")    --name
snd ("name", "haskell")    --haskell
```

这里二元组就相当于命令式下的散列类型，第一个元素作用等同于散列的键，第二个元素相当于散列的值。当然也可以采用多元组（奇数位存键，偶数位存值）或者元组列表来模拟多键值对的存储。

最后我们看下如何配合列表来处理稍微复杂一点的数据类型：

```haskell
--Haskell 取列表中第三个元组第二个元素的值

snd . last . take 2  $ [("language", "ruby"), ("language", "java"), ("language", "haskell"), ("language", "javascript")]
--haskell
```


### 函数类型

####函数

函数是程序中的一段子程序，它用来执行一定的操作或运算，通常函数包括一个入口和一个出口。入口即参数输入（输入可以为空，即不输入参数），出口即函数返回值（输出可以为空，即不返还值），简单的说在你使用一个函数时，需要输入一些参数，函数会根据输入参数进行运算，然后会返回给你一个运算结果。

比如如下一个除运算函数，需要输入除数b与被除数a，函数进行除法运算后会返回一个商a/b。

```java
//Java 函数

public static double devide(double a, double b){
  return a/b;
}

public static void main(String args[]){
  double r = devide(3, 2);
  System.out.println(r);    //1.5
}
```

####函数分类

函数分为标准函数和自定义函数，所谓标准函数就是语言本身提供的函数，通常这是一些通用的函数，视具体语言不同标准函数库也稍有差别。

比如针对数组排序的函数sort，在java中该函数会破坏原数组：

```java
//Java sort方法会破坏原数组

public static void main(String args[]){
  int[] a = {4, 7, 5, 1, 2};
  int[] b = Arrays.sort(a);
  for(int i in a) {
    System.out.print(a[i] + " ");    
    //1 2 4 5 7
  }
} 
```
可以看出数组a的排序也发生了改变。对比Java，Ruby提供了两种排序方法sort和sort!，其中sort方法排序数组会保留原数组，返回一个新的排序后的数组，而sort!方法和上述中Java的Arrays.sort()方法类似，会破坏原数组。值得一提的是Ruby对大多数会产生破坏性的方法都提供了一组这样的函数，其中`!`结尾的方法表示会破坏原结构，之所以用`!`大概是想提示你这个函数要慎重使用。

```ruby
#Ruby sort和sort!方法对比

a = [4, 7, 5, 1, 2]
a.sort
puts a    #[4, 7, 5, 1, 2]

a = [4, 7, 5, 1, 2]
a.sort!
puts a    #[1, 2, 5, 5, 7]
```

对于大多数语言而言最基本的函数都是有提供的，比如字符串处理函数：
substring：字符串截取、concat：字符串合并、replace：字符串替换、toUpperCase/toLowerCase：大/小写转换、charAt：截取字符、startsWith/endsWith：开始/结束字符等；以及数组处理函数：join：数组转字符串、reverse：数组翻转、sort：数组排序、concat：数组合并、slice：数组截取、splice：插入删除等。

而自定义函数就是用户自己定义的函数。

抛开这种分类方式，可以从面向对象编程和函数式编程所属范式的角度说明函数相关的概念。面向对象的核心就是以对象的方式处理问题，所以这部分函数通常都和类/对象相关，而函数式则以函数的方式处理问题，所以这部分的函数通常是一些函数的组合。

#### 对象相关函数

##### 函数重载

函数重载是针对于多个函数而言的，对于像C++、Java、C#这些静态类型语言而言，函数参数是需要声明具体类型的，相同名称不同参数类型的函数可以看做是不同的函数。重载就是针对这一特点而言的，函数重载允许创建输入参数或输出参数不同，但函数名相同的函数。

来看下Java的函数重载：

```java
//Java 函数重载

public static double add(double a, double b){
  return a+b;
}

public static int add(int a, int b){
  return a+b;
}
```
上面两个函数都叫做add，但是输入参数类型和返回结果类型不同，这就是函数重载。因为语言特性不同，对于动态语言而言，函数重载是没有意义的，因为动态语言本身是支持类型推导，所以没必要重复声明同一个函数应付不同的参数类型。

```ruby
#Ruby 动态类型语言不必重复接受不同参数的同一函数

def add(a, b)
  a+b
end

puts add(1 ,2)        #3
puts add(1.3, 1.2)    #2.5
```
Ruby会根据输入参数进行类型推导，并根据实际传入类型进行运算，输出对应类型的返回值。

##### 运算符重载

如果函数重载是对一个函数重新定义，那么运算符重载就是对运算符对应的运算规则重新定义。出于语言设计者的考虑，运算符重载也不是所有语言都支持的。比如Java语言就不提供运算符重载，而C++和Ruby则提供了运算符重载。

因为Ruby的运算符本身就是一个函数，只是函数名称是运算符号而已，所以重新定义对应符号名的函数就相当于运算符重载了。

比如这个例子：假设我们有一个班级类Class，Class下有两个属性：boy\_numbers和girl\_numbers,分别记录了班级里男生人数和女生人数，现在我们想不区分性别，通过一个简单的加法运算来求出两个班级的总人数。这里我们用Ruby来重写`+`运算。

```ruby
#Ruby (+)运算符重载

class Class
  attr_accessor :boy_numbers, :girl_numbers
  def initialize(boys, girls)
    @boy_numbers = boys
    @girl_numbers = girls
  end

  def +(c)
    @boy_numbers + c.boy_numbers + @girl_numbers + c.girl_numbers
  end
end

puts Class.new(27, 24) + Class.new(26, 23)    #100
```
可以看出我们重写了+运算符，使他对传入的对象c取得boy\_numbers和girl\_numbers的值并与自身的boy\_numbers和girl\_numbers值求和，最后返回总人数。

##### 构造函数

构造函数是类在实例化一个对象时执行的函数，一般用于初始化对象的属性。

Java构造函数

在Java中构造函数名与类名一致，你可以显示的定义构造函数，并执行一些赋值操作。
```java
//Java 显式定义构造函数并执行赋值操作

class Demo{
  public int a, b;
  public Demo(int a, int b){    
    //构造函数
    this.a = a;
    this.b = b;
  }
  public static void main(String args[]){
    Demo d = new Demo(1, 2);
    
    System.out.println(d.a +","+ d.b);    //1,2
}
```

Ruby构造函数

在Ruby中类的构造函数都用initialize命名，同样你也可以显示的定义，并进行赋值操作。

```ruby
#Ruby 构造函数

class Demo
  attr_accessor :a, :b    #配置属性可读可写
  def initialize(a, b)
    #构造函数 
    @a = a
    @b = b
  end
end
d = Demo.new(1, 2)

puts d.a, d.b    #1 2
```

##### 析构函数(destructor)

与构造函数相反，析构函数在对象结束调用后执行，用来释放对象所占用的内存空间，回收资源。现在的高级语言都加入了垃圾回收机制帮我们处理废弃的对象并回收内存空间。所以通常你应该也不会去定义析构函数了。

在C++中析构函数采用在构造函数名前加`~`的方式来命名。

```c-plus
//C++ 构造函数和析构函数

class Demo{
  public:
    Demo(){};     //构造函数
    ~Demo(){};    //析构函数
}
```

##### 实例方法和类方法

对于面向对象语言而言，实例方法（也可以叫作成员方法）和类方法是不得不说的一个概念。

简单的说实例方法是属于对象的函数，而类方法是属于类的函数，也就是说前者通过对象调用，而后者则通过类调用。


Java中的实例方法和类方法

在Java中类级别的方法或者属性通过static关键字修饰。

```java
//Java 类方法和实例方法

class Animal{ 
  public static int numbers;
  public int index;

  public static int getNumbers(){
    return numbers;
  }
  public int getIndex(){
    return index;
  }
  public Animal(){
    numbers++;
    index++;
  }
  public static void main(String args[]){
    Animal cat = new Animal();
    Animal dog = new Animal();

    System.out.println(Animal.getNumbers());    //2
    System.out.println(dog.getIndex());         //1
  }
}
```
上述例子中声明了类属性number以及类方法getNumber，实例属性（也有叫成员变量的）index和实例方法getIndex。输出结果中可以看出，每次实例对象都会有一个新的index，但是number却是共享的，因为它是类级别的。

Ruby中的实例方法和类方法

Ruby则通过`@@`来定义类属性，通过self关键字来定义类方法，self代表当前运行时的上下文，这里所指上下文就是类本身。

```ruby
#Ruby 类方法和实例方法

class Animal
  @@numbers = 0    
  def initialize()
      @@numbers += 1
      @index = 1
  end
  def self.get_numbers
      @@numbers
  end
  def get_index
      @index
  end
end
cat = Animal.new
dog = Animal.new

puts Animal.get_numbers     #2
puts dog.get_index          #1
```
上面的例子和Java一样，只是语法上稍有不同，Ruby通过`@`定义成员属性,通过`@@`定义类属性。

##### 私有方法

私有方法可以看做是一种特殊的实例方法和类方法，私有方法是只有在当前类（上下文）下才能调用的方法，通常用private关键字修饰，如果这个类存在子类，那么子类是无法访问该方法的（注意子类会继承该方法，只是无法访问）。

```java
//Java 类的私有方法

class A{
  private void privateMethod(){
    System.out.println("This is a private method of class A");
  }
  public void callPrivateMethod(){
    this.privateMethod();
  }
}

class B extends A{}

public class Demo{

  public static void main(String args[]){
    A a = new A();
    B b = new B();

    a.privateMethod();        
    //error: privateMethod() has private access in A       

    a.callPrivateMethod();    
    //This is a private method of class A

    b.privateMethod();        
    //error: cannot find symbol b.privateMethod();

    b.callPrivateMethod();    
    //This is a private method of class A
  }
}
```
可以看出privateMethod只有在类A的上下文下才可以访问，在外部通过a对象调用会报权限错误，而子类调用则找不到该方法。

```ruby
#Ruby 类的私有方法

class A
  private
    def privateMethod
      puts "this is private method of class A"
    end
  public
    def callPrivateMethod
      privateMethod()
    end
end
class B < A
end

a = A.new
b = B.new

a.privateMethod        
#private method `privateMethod' called for #<A:0x007fde9309e038> (NoMethodError) 

a.callPrivateMethod    
#this is private method of class A

b.privateMethod        
#private method `privateMethod' called for #<A:0x007fde9309e038> (NoMethodError) 

b.callPrivateMethod    
#this is private method of class A 
```

可以看出Ruby中的私有方法调用结果和Java一样，对于类A所在的上下文外是无法访问的。


##### 友元函数

友元函数是针对一个类而言的，当一个函数定义为一个类的友元函数，那么这个函数就可以无视访问权限关键字（private、protected）直接访问这个类的属性。因为友元函数的设计降低了封装性，和面向对象设计的根本原则有所违背，所以在后续的语言中不再提供这个概念，这里也不过多赘述了。


##### 属性访问器

同私有方法类似，通过private修饰的属性来是无法在外部直接访问的。因此需要提供一个属性读与写的入口，这就是属性访问器。通常属性访问器用set/get表示，set表示写，get表示读。

```java
//Java 属性构造器

class A{
  private String privateVariable;

  public void setVariable(String a){
    this.privateVariable = a;
  }
  public String getVariable(){
    return this.privateVariable;
  }
}
public class Demo{
  public static void main(String args[]){
    A a = new A();
    
    a.privateVariable = "private variable";    
    //error: privateVariable has private access in A

    System.out.println(a.privateVariable);     
    //error: privateVariable has private access in A

    a.setVariable("private variable");         
    System.out.println(a.getVariable());       
    //private variable

  }
}
```
可以看出无法在类外直接访问类的私有属性，需要通过set和get方法进行读写操作，这样做可以保证数据读写的安全。

接下来我们看看Ruby中是如何实现属性构造器的。

```ruby
#Ruby 属性构造器

class A
  attr_accessor :privateVariable
end

a = A.new
a.privateVariable = "private variable"

puts a.privateVariable    #private variable
```
在Ruby中，类的所有属性都是private的。Ruby提供了对属性读写的配置：attr\_reader、attr\_writer和attr\_accessor，其中attr\_reader表示属性可读，相当于Java的get方法，attr\_writer表示属性可写（配置）相当于set方法，最后这里用到的attr_accessor表示属性既可读又可配置，相当于同时定义了set方法和get方法。属性具备了读写特性后就可以通过`.`运算进行读写操作了。


##### 虚函数（virtual function）和函数重写（override）

虚函数是C++中提出的一个概念，当子类继承父类时，如果父类中某个函数定义为虚函数，那么子类可以重写这个函数的实现。虚函数需要用virtual关键字修饰。

```c-plus
//C++ 虚函数

class A{
  public:
    virtual void VirtualFunction(){
      cout << "This is virtual function of class A";
    }
};
class B : public A{
  public:
    void VirtualFunction(){
      cout << "Rewrite virtual function in class B";
  }
};
```

虚函数是面向对象语言中实现多态的一种重要手段。在Java语言中所有函数默认都是虚函数，也就是子类可以重写所有父类的函数，因此Java中称作函数重写。

```java
//Java 函数重写

class A{
  public void overrideFunction(){
    System.out.println("function of class A");
  }
}

class B extends A{
  public void overrideFunction(){
    System.out.println("function override by class B");
  }
}
class C extends A{
  public void overrideFunction(){
    System.out.println("function override by class C");
  }
}

public class Main{
  public static void main(String args[]){
    A a = new A();
    B b = new B();
    C c = new C();

    a.overrideFunction();    //function of class A
    b.overrideFunction();    //function override by class B
    c.overrideFunction();    //function override by class C
  }
}
```

#### 函数相关的函数

##### 匿名函数

匿名函数是无需显示声明的函数，一般用于基本不会复用或使用次数有限的函数。匿名函数可以使代码简化，方便调用。

除此之外匿名函数也常用作高阶函数的参数或返回值，这一点会在介绍函数式的时候详细说明。


JavaScript中匿名函数

```javascript
//JavaScript 匿名自执行函数
console.log(function(){ return 2 }());    //1
```

我们在log函数里传入了一个匿名函数`function(){ return 2 }`，紧接着通过`()`运算符执行了这个匿名函数。

接下来再看下匿名函数作为返回值的例子：

```javascript
//JavaScript 匿名函数作为返回值

function add (a){
  return function(b, c){ a+b+c };
}

add(1)(2,3);    //6
```
上面的例子中add函数返回了一个匿名函数`function(b, c){ a+b+c }`，当调用add(1)时会返回一个函数`function(b, c){ 1+b+c }`，所以我们可以继续用`()`运算符传入参数b与c执行返回的匿名函数。

Java匿名内部类和lambda表达式

Java本身是不支持匿名函数的，但是可以通过匿名内部类实现匿名函数的功能。

```java
//Java 匿名内部类模拟匿名函数

class Demo{
  public static void main(String args[]){
    new Object(){
      void add(int a, int b){  System.out.println(a + b); }
    }.add(1, 2);
  }
}
```

上面我们在输出方法中实例化了一个匿名内部类，并在类中定义了add方法，紧接着用这个实例化的对象调用了add方法，早期版本的Java不支持匿名函数与其初衷的设计也是有关的。在后续版本Java8中引入了lambda，通过lambda表达式也可以避开丑陋的匿名内部类直接实现匿名函数。

 ```java
//Java 对比匿名内部类与lambda表达式

/*  匿名内部类实现  */
new Thread(new Runnable() {
  @Override
  public void run() {
    System.out.println("anonymous inner class");
  }
}).start();

/*  lambda表达式实现  */
new Thread(() -> System.out.println("lambda express")).start();
 ```

在这里还要强调一点，虽然Java8引入了lambda的特性，但其实并非是单纯的匿名函数，和内部类一样只是一种匿名函数的实现手段，lambda表达式和匿名函数最大的区别就在于lambda并不是一个可以通过`()`运算符直接调用的函数，它只是一段带有作用域的代码，其主要目是数据及代码的传递，如果从这方面看匿名函数的话，也可以理解为匿名函数是一种以函数的形式来传递数据和代码的手段。

Ruby中的proc和lambda

在Ruby中匿名函数也是通过lambda实现的，不同的是Ruby中的lambda是一个proc对象，Ruby用proc和block两个概念来包装代码段，区别在于proc是Proc类的对象，而block只是单纯的代码段，所以block只能作为代码段使用，而proc可以像变量一样传递。

虽然lambda是一个proc对象，但两者也存在区别：lambda会检查参数的合法性（比如实参个数与形参是否匹配），而proc则不会如此。

```ruby
#Ruby 匿名自执行函数

lambda{ |x| puts x + 1 }.call(2)        #3
Proc.new { |x| puts x + 1 }.call(2)     #3

```
上面代码定义了一个匿名的lambda，并且通过call（等同于上面JavaScript代码中的()）执行了这个lambda。第二个例子则创建了一个匿名的Proc对象并执行。

因为proc和lambda都是对象，所以是可以赋值给其他变量的。

```ruby
#Ruby lambda作为参数传递

a = lambda { |x| puts x*2 }
[1, 2, 3].each(&a)    #2 4 6
```

上面代码中`&`运算符是将proc对象转为block，相当于把代码段`puts x*2`带入到each中，当然这样的写法就不能算作纯粹的匿名函数了，因为我们给lambda指定了变量名a。

```ruby
#Ruby 匿名函数作为参数传递

[1, 2, 3].each(&lambda { |x| puts x*2 })    #2 4 6 
```

##### 纯函数

纯函数是指没有副作用的函数，所谓副作用是指会对外界变量、状态产生影响的语句，也就是说纯函数不会改变函数外变量的值，因此纯函数对相同的参数输入会返回相同的计算结果。

这种禁止修改外部状态的策略，也是函数式语言的一种策略，对于纯函数式语言，所有的函数必须都是纯函数（比如Haskell），正因为纯函数语言不存在可变状态，使其在并发编程下具备绝对的优势。

我们来通过Ruby的reverse和reverse!对比下纯函数和非纯函数的区别。

```ruby
#Ruby reverse函数

a = [1, 2, 3]

puts a.reverse    #[3,2,1]
puts a            #[1,2,3]
```
```ruby
#Ruby reverse!函数
a = [1, 2, 3]

puts a.reverse!    #[3,2,1]
puts a             #[3,2,1]
```

上面的例子中reverse就是一个纯函数，reverse翻转一个数组后不会改变原数组，而reverse!则相反，它会修改原数组，所以reverse!不是一个纯函数，包括上面函数分类用的sort和sort!也是如此的，Ruby本身提供的函数是纯函数，根据需求你也可以用相应的`!`函数改变源数据。对比Java（Java的reverse是破坏性的）和Haskell，Ruby提供了更多的可选条件。

##### 谓词函数

谓词函数是值返回值为布尔类型的函数，一般常在函数式语言中使用来配合构成高阶函数。

```javascript
//JavaScript 谓词函数

var isString = function(s){ return typeof s === "string" };

isString("it's a string");    //true
isString(1);                  //false
```
上述代码定义一个字符串判断函数，如果输入参数为字符串则返回true，否则返回false。

同样还是特别介绍下Ruby的谓词函数

```ruby
#Ruby 谓词函数

"it's a string".is_a? String    #true
1.is_a? String                  #false
```
函数is_a?用来判断参数类型是否匹配，匹配则返回true，否则返回false，注意观察会发现这个谓词函数后面跟着一个`?`，和破坏性函数类型，Ruby用`?`来特别标识一个函数为谓词函数。

因此如果我们用Ruby来定义一个谓词函数时按照规范应该在函数名后标识一个`?`。

```ruby
#Ruby 定义谓词函数

def is_string? s
  s.is_a? String
end

is_string? "it's a string"    #true
is_string? 1                  #false
```

##### 高阶函数

高阶函数是一种参数、返回值为函数的函数。高级函数作为函数式编程中最为基础和根本的内容，我会在后续章节中用一节的内容详细介绍，现在你只需要记得高阶函数可以把其他函数作为参数传入，也可以指定一个函数作为计算结果返回。



### 类、类型类

#### 结构体

在说明类之前我想先聊一聊结构体。

结构体在C语言中是用来打包存储多种数据类型的数据类型，它可以在一块内存区存储多种变量，并通过一个名字来访问这些变量。

```c
//C 定义一个结构体 

struct account {
   int account_number;
   char *first_name;
   char *last_name;
   float balance;
};
struct account s;
```

简单的说，结构体就是一种可以包含多种数据类型的数据类型。

类和结构体类似，也是打包不同数据类型的数据类型，类通常由属性和方法构成，其中属性用来存储变量，方法则是函数操作。

#### 对象及其实例化

**实例和对象**

如上所述类其实就是一系列数据类型的封装，其本身也是一种数据类型。通过类声明的一个变量就叫做对象或者实例。

```java
//Java

int a;      //声明了一个Int类型的变量a，a是一个int类型的变量
Pet dog;    //声明了一个Pet类的对象dog，dog是Pet类的一个实例
```

**new运算符**

new操作符会为对象分配一块内存并返回这个内存的引用，同时它也会调用构造函数，我们把这个过程叫做实例化。接下来构造函数会对对象的属性进行初始化赋值，我们把这个过程叫做初始化。

```java
//Java 实例化类并初始化对象

Pet cat = new Pet();    //实例化cat对象
//注意Java中构造函数与类名相同，因此前一个Pet是对象的类型，后一个Pet是用来初始化对象的构造函数的名称。
```

**创建一个对象**

综上所述创建一个对象包括三个步骤

1. 声明一个类的对象
2. 通过new操作符实例化对象
3. 通过这个类的构造函数初始化对象

**类与对象的关系**

其实对象就是一系列状态和行为的打包，比如说铅笔有笔芯和笔杆（对象的状态），铅笔可以写字、画画（对象的行为）。而类是对象的模板，比如说铅笔有笔芯笔杆，可以写作绘画，油性笔也有笔芯笔杆，也可以写作绘画，像这样的有笔芯有笔杆可以写作绘画的东西就叫做笔。那么笔就是模板，铅笔、油性笔就是对象，而这个模板所打包的状态就是笔芯、笔杆，打包的动作就是写作、绘画。

```java
//Java 作为笔的类

Class Pen {
  public Refill r;
  public Shaft s;
  public void write(){};
  public void draw(){};
}
```

有了类与变量之间关系的认识，就可以更书面的说明类的定义了：

类是一个模板，它描述一类对象的行为和状态。


#### 类和变量

通常在类中有三种变量：局部变量、实例变量和类变量。

实例变量和类变量在实例函数和类函数中已经有过说明，可以通过类直接访问的变量就是类变量，一般用static关键词修饰。通过对象访问的变量是实例变量。

局部变量是指那些在函数中定义的变量，是无法通过类、对象直接访问的。

```java
//Java 类变量、实例变量和局部变量

Class Demo{  
  public static String classVariable = "类变量";
  public String instanceVariable = "实例变量";

  public static void(String args[]){
     String localVariable = "局部变量";
  }
}
```

更多关于类的概念我会在后续章节面向对象特点下详细说明。

#### 鸭子类型

在说明类型类之前，我想先介绍下Ruby语言中提出的鸭子类型概念，这样方便我们区分联系类及类型类之间的关系。

>当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。

鸭子类型中关注的不是对象本身是什么，而是其表现出的行为，也就是它的使用场景。还是以之前的笔为例子说明，我们用木枝在沙滩上写字绘画，很明显木枝并不是笔的一种，但是它能作为笔来使用，实现写的功能，那么这是我们就可以把木枝称作为“笔”。这就是鸭子类型的特点，不注重对象本身，而是对象其行为。

有了鸭子类型的概念我们继续下面的类型类。

#### 类型类

如果说类是Java中实现复用的重要手段，那么类型类就是Haskell中实现复用的重要手段。

类型类和鸭子类型的思维方式类似，它也强调行为功能。通常类型类描述一些列的行为，属于这个类型类的类型就实现了其定义的行为（你也可以把类型类理解为Java的接口，它是描述行为的一种抽象）。

我们来看下Haskell中函数(>)的定义：

```Haskell
--Haskell (>) 定义

(>):: (Ord a)=> a -> a -> Bool  
```
这里Ord就是一个类型类，属于Ord的类型a就具有排序的行为，这个定义说明传入两个a类型，a类型具有比较行为，进行比较运算后返回一个布尔类型。

在Haskell中除了Ord还有诸如Enum、Num、Bounded等类型类，它们分别具备成员是否连续、是否为数字、成员的上下限等行为。类型具备了类型类的行为才能实现出相等、比较、字符串转换这些运算。


#### 类、类型类、类型及类类型

这些概念放在一起确实有些容易混淆，所以我想在最后简单区分下。

类和类型类已经说明过了，类是面向对象程序中非常重要的一个概念，而类型类是Haskell语言中抽象行为的一种方式。类型就是我们这两章中在说明的概念，那么类类型是什么，很简单，就是“类 类型”。如果把类看做是声明的变量，那么类类型就是对应的类型，就像`int a`，如果a是一个类，那么int就像类类型。
