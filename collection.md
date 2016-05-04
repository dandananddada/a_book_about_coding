# 集合类型

这里我想围绕两个概念来说明集合类型：

1.异质（是指集合中允许不同数据类型）
2.定长（集合的长度可以拓展）

####数组
在Java中数组是同质定长的，也就是说Java的数组是不允许不同类型数据混合的，且其长度是在实例化的时候就指定的。
```java
int[] a = { 1, 2, "three" };
//=>error incompatible types: String connot be converted to int

int[] b = new int[10];
b[10] = 11;
//=>exception in thread main java.lang.ArrayIndexOutOfBoundsException
```
可以看出声明一个异质的数组会报编译时错误，而对超出数组索引的元素赋值会报运行时异常（数组越界异常）。

于此不同JavaScript和Ruby的数组就是异质且变长的。
```javascript
var a = [1, 2, 3];
console.log(a.length);    //=>3
a[3] = 4;
console.log(a.length);    //=>4
a[6] = 7;
console.log(a);           //=>[1, 2, 3, 4, undefined, undefined, 7]
console.log(a.length);    //=>7
```
在数组方面Ruby和JavaScript表现是一致的。
```ruby
a = [1, 2, "three"]
puts a.length            #=>3
a[3] = 4         
puts a.length            #=>4
a[6] = 7
puts a                   #=>[1, 2, 3, 4, nil, nil, 7]
puts nil
puts a.length            #=>7
```

**数组长度**

上面已经说过Java的数组是定长度，所以对长度为3的数组进行`a[4]=5`这赋值操作会报数组越界的异常。

在JavaScript和Ruby中数组是变长的，也就是你可以对长度为3的数组执行类似`a[4]=5`这样的赋值操作，但是不同的是Ruby会把中间遗漏掉的元素用`nil`填补，而JavaScript则直接跳过，因为JavaScript本身没有数组类型，数组实际上是Key值为数字的Object，你可以给一个Object定义任何键值，所以跳过数组索引顺序赋值本身也是合法的（这样对一个数组的某个元素赋值时就会形成一个稀疏数组）。
```ruby
#ruby
a = [1, 2, 3]
a[5] = 6
puts a           #=>[1, 2, 3, nil, nil, 6]
puts a.length    #=>6
```
```javascript
var a = [1, 2, 3];
a[5] = 6;
console.log(a);           //=>[1, 2, 3, , , 6]
console.log(a.length);    //=>6
```
与Ruby不同，JavaScript甚至支持对Array.length赋值，当设置的length值大于数组长度时，JavaScript也会形成一个等长度的数组，不过缺省的元素也是`undefined`。假如设置的length值小于数组实际长度，那么数组就会截取后面的元素。
```javascript
var a = [1, 2, 3];
consoel.log(a.length);    //=>3
a.length = 5;
consoel.log(a[4]);        //=>undefined 
consoel.log(4 in a);      //=>false

/* 这时a是一个稀疏数组 */
console.log(a);           //=>[1, 2, 3, , ,]

a.length = 2;      
consoel.log(a[3]);        //=>undefined
consoel.log(3 in a);      //=>false
consoel.log(a);           //=>[1, 2]
```

**稀疏数组**

在JavaScript中缺省初始化一个数组时,不设置初始值，JavaScript在遍历数组时会跳过这些元素没有值的元素（这些元素缺省值为undefined，但是不存在对应的key）。在JavaScript这样的数组就是稀疏数组。
```javascript
//javascript
var a = [, , 3];
a.forEach(function(v){
	console.log(v)
});
//=>3
```
如果我们初始化的时候指定缺省值，这时候数组就不是稀疏数组了。
```javascript
//javascript
var a = [undefined, 1]
,   b = [, 1];

console.log(0 in a);    //=>true
console.log(0 in b);    //=>false
Object.keys(a);         //=>["0", "1"]
Object.keys(b);         //=>["1"]
```
因为JavaScript中数组本身是key值为数字的Object，所以可以用in来判断键值是否在对象内，上面的例子可以发现`a[0]`是存在的，而`b[0]`是不存在的。所以a是稠密数组，而b是稀疏数组。

但Java中没有划分稀疏数组的，因为Java数组默认都是有缺省值的。
```java
//java
int a[] = new int[3];
a[2] = 3;

for(int i=0; i<a.length; i++){
  System.out.println(a[i]);
}
//=>003
```
Java中整型数组的缺省值为0，字符型数组缺省值为null。

不过在Android SDK中提供了系数数组类SparseArray，它是用来处理稀疏矩阵问题的，和JavaScript中定义的稀疏数组是两种概念，简单说明下稀疏矩阵就能清楚的分辨出两者的区别了。

假设我们用一个二维数组来表示一个`5*5矩阵`
```
1 0 0 0 0
0 2 0 0 0
0 7 0 0 0
0 0 0 0 9
0 0 0 0 0
```
上面这个矩阵中有效的数值其实只有4个，剩下的21个都是空，这样用一个二维数组来存储这个矩阵会浪费大量的空间。我们可以通过记录有效的数据来压缩这个矩阵，如下。
```
5 5 4    //原矩阵的 行 列 有效数字
1 1 1    //第一个元素所在 行 列 及数值
2 2 2    //第二个元素所在 行 列 及数值
3 2 7    //第三个元素所在 行 列 及数值
4 5 9    //第四个元素所在 行 列 及数值
```
这样就把一个`5*5`的矩阵压缩为`3*4`的矩阵了，这个压缩后的矩阵就是稀疏矩阵，因为采用了数组类型所以笼统的就叫做稀疏数组了（注意Java SE本身没有特别定义稀疏数组的概念）。

**链表**

单纯的数组就先说这么多，之前有说要从数组的异质和定长两方面讨论数组，JavaScript和Ruby所提供的数组本身就是异质变长的，那么先抛开这两门语言，但从Java看，首先看下变长数组。

List是Java的一个接口，它有两个实现类ArrayList和LinkedList，List允许你根据需求动态的增加数组（你也可以叫做链表，实际上就是变长数组的概念）的长度。

大多数情况下使用场景下使用的都是ArrayList，只有在频繁的向数组头部插入元素，或者通过遍历删除数组元素时才会用LinkedList。所以这里我只通过ArrayList给出一个变长的例子。

```java
//java
/* 这里需要引入 List 接口 和 ArrayList 实现类 */
import java.util.List;
import java.util.ArrayList;
class Demo {
  public static void main(String args[]){
      List<Integer> list = new ArrayList<Integer>();
      list.add(1);
      list.add(2);
      list.set(0, 3);
      System.out.println(list.get(0));    //=>3
      System.out.println(list.get(1));    //=>2
      System.out.println(list.size());    //=>2
   }
}
```
`ArryList.set(index, value)`可以修改索引为`index`的元素的值为`value`，但要注意你不能像用Ruby或JavaScript一样对长度为3的ArrayList取第四个元素赋值。这和数组一样会在运行时报数组越界的异常。
```java
//java
List<Integer> list = new ArrayList<Integer>();
list.add(1);
list.set(3, 3);
//=>Exception in thread "main" java.lang.IndexOutOfBoundsException
```
