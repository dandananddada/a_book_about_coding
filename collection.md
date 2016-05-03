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