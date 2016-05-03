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

**稀疏数组**
假设我们想用一个二维数组来表示一个`5*5矩阵`
```
1 0 0 0 0
0 2 0 0 0
0 7 0 0 0
0 0 0 0 9
0 0 0 0 0
```
上面这个矩阵中有效的数值其实只有4个，剩下的21个都是空，这是用一个二维数组来存储会浪费大量的空间。我们可以通过记录有效的数据来压缩这个矩阵，如下。
```
5 5 4    //原矩阵的 行 列 有效数字
1 1 1    //第一个元素所在 行 列 及数值
2 2 2    //第二个元素所在 行 列 及数值
3 2 7    //第三个元素所在 行 列 及数值
4 5 9    //第四个元素所在 行 列 及数值
```
这样就把一个`5*5`的矩阵压缩为`3*4`的矩阵了，只要在解析的时候按照上述逻辑处理就可以了。这样就节省了很大一部分的内存开销。把这样的一个数组叫做稀疏数组（注意Java SE本身不提供稀疏数组API，Android API提供 SparseArray）。

而在JavaScript中缺省初始化一个数组时,不设置初始值，JavaScript在遍历数组时会跳过这些元素没有值的元素（这些元素缺省值为undefined，但是不存在对应的key）。在JavaScript中这样的数组定义为稀疏数组。
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