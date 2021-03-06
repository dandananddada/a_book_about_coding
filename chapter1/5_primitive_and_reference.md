# 基础类型和引用类型

抛开在1.1中列举的数据类型分类方式，这里我想从另外一种分类方式来说明：基础类型（有些语言喜欢称作值类型，比如C#）和引用类型。


#### 基础类型与引用类型对比

**基础类型**
1. 基础类型存储的是直接值
2. 基础类型的值存储在栈中
3. 基础类型是不可修改的
4. 基础类型进行比较时，直接值相等就相等
5. 基础类型在作为参数传递时，传递的是值
6. 一般数字类型、字符串类型、布尔类型都是基础类型

**引用类型**
1. 引用类型存储的是地址
2. 引用类型的值存储在堆中
3. 引用类型是可修改的（可变的）
4. 引用类型进行比较时，值相等，且地址相等才相等
5. 引用类型作为参数传递时，传递的是引用
6. 一般数组类型、对象类型、函数类型都是引用类型

接下来对上面的五点分别举例说明。

#### 值的存储

基础类型在栈中存储直接值，而引用类型在栈中存储引用的地址，在堆中存储实际值。

<img src = "../images/primitive_and_reference_type.png" alt = "" style = "width: 380px; float: right;">

对于编译型语言而言栈是在编译时处理内存的分配的，在运行时处理堆的内存分配。所以栈的内存分配是静态的，堆的内存分配是动态的；栈中存储的数据是固定大小的，而堆中的数据是可变大小的。因为编译型语言在程序编译时处理了基础类型的内存分配，所以相比解释型语言在运行时的处理速度效率自然要高。

对于基础类型和引用类型的存储情况可以参考右图。


#### 是否可变

基础类型是不可变的，这里的不可变不是指变量的值不可变，而是存储在栈中指定地址的值是不允许改变的。我们通过Ruby代码来说明值不可变的意思（因为获取一个值的栈地址对于现在的编程语言来说并没有实际作用，所以Java一类的语言本身并没有提供类似的功能，多亏Ruby是支持的）。
```ruby
#Ruby 获取基础类型栈地址

a = 5
a.object_id    #11

a = 3
a.object_id    #7

b = 5
b.object_id    #11
```

这里我们先对变量a赋值为5，然后输出5对应在栈中的地址11（转换为二进制是0b1011），然后我们修改a的值，再次输出a的地址，会发现地址改变了，这说明a的值改变了，是因为a的地址变了。然后我们对b赋值为5并输出地址，会发现其地址也是11，这说明地址为11的内存中存储的值就是5，是不会改变的，变量值的改变是因为指向的地址发生了改变。

引用类型是可变的，根据上述描述，可以猜测到所谓可变是指存储引用类型的地址所存储的值是可以改变的。

数组是引用类型的，我们修改数组中元素的值，然后输出数组的地址信息进行对比。
```ruby
Ruby 获取引用类型栈地址

a = [1, 2, 3]
a.object_id       #70324083600480
a[0].object_id    #3

a[0] = 0
a.object_id       #70324083600480
a[0].object_id    #1

```
可以发现数组的值改变了，但是其地址信息没有改变，说明引用类型的值是可变的。当输出a[0].object_id发现这个地址改变了，因为a[0]是个数字类型，数字类型是基本类型，他的值是不可变的，所以a[0]指向了改变后的值的地址。但是数组a的地址不会因为元素值的修改而修改，除非你对a重新赋值（Java是不允许这样做的，Java只允许数组初始化时整体赋值或者循环每一个元素分别赋值）。

```ruby
#Ruby 重新赋值引用类型，引用地址会改变

a = [1, 2, 3]
a.object_id    #70324083600480

a = [0, 1, 2]
a.object_id    #70324083654580
```

#### 相等比较

判断基础类型是否相等，只要值相等就是相等的。而引用类型相等的条件是保证引用的对象是同一个，也就是说栈中存储的地址是指向堆中同一个位置的。

```javascript
//JavaScript 基础类型、引用类型比较
 
a = 1;
b = 1;
a === b;    //true

a = [1, 2, 3];
b = [1, 2, 3];
a === b;    //false
```

```java
//Java 基础类型、引用类型比较

int a = 1;
int b = 1;
System.out.print(a == b);    //true

int a[] = { 1, 2, 3 };
int b[] = { 1, 2, 3 };
System.out.print(a == b);    //false
```

```ruby 
#Ruby 基础类型、引用类型比较

a = 1
b = 1
a.equal? b    #true

a = [1, 2, 3]
b = [1, 2, 3]
a.equal? b    #false
```

这里单独说明下ruby用`==`判断两个对象的值是否相等，用`equal?`判断是否为同一引用。

```ruby
#Ruby ==和equal?的区别

a = {:type=>"Object"}
b = {:type=>"Object"}
c = a

a == b        #true
a == c        #true
a.equal? b    #false 
a.equal? c    #true
```
#### 参数传递

基础类型传递的是值，而引用类型传递的是引用。基础类型发生修改是无副作用的（不会修改原变量），引用类型修改是副作用的。
```javascript
//JavaScript 基础类型、引用类型参数传递

var a = 1;
var o = { b: 1 };
var fa = function(a){  a = 2;  }
var fo = function(o){  o.b = 2;  }
f(a);
f(o);

console.log(a);      //1
console.log(o.b);    //2
```
```java
//Java 基础类型、引用类型参数传递

Class Demo{
  public static void mi(int a){  a = 2;  }
  public static void mr(int[] a){  a[0] = 2;  }
  public static void main(String args[]){
    int a = 1;
    int[] aa = { 1, 2, 3 };
    Demo.mi(a);
    Demo.mr(aa);

    System.out.print(a);        //1
    System.out.print(aa[0]);    //2
  }
}
```
对于变量拷贝也是如此，基础类型拷贝的是值，修改是无副作用的；引用类型拷贝的是引用，修改是有副作用的。

```javascript
//JavaScript 基础类型、引用类型拷贝

var a = 1
,   b = a
,   oa = { a: 1 }
,   ob = oa;
b    = 2;
ob.a = 2;

console.log(a);       //1
console.log(oa.a);    //2
```
```java
//Java 基础类型、引用类型参数传递

int a = 1, b = a;
int aa[] = { 1, 2, 3 }, ab[] = aa;
b = 2;
ab[0] = 2;

System.out.print(a);        //1
System.out.print(aa[0]);    //2
```

#### 基础类型和栈内存分配

字符类型是基础类型，所以在做字符串修改、合并、截取等操作时，字符串的每次改变都会在栈中分配一块新的内存记录新的值。

```ruby
#Ruby 字符串修改会从栈分配新内存

a = [ "A", "B", "C"]
s = "Array a contains: "
a.each do |i|
	s += i	
	puts "address of s is #{s.object_id}"
end
puts s
# output
# address of s is 70123772400880
# address of s is 70123772400780
# address of s is 70123772400700
# Array a contains: ABC
```

可以看出，如果循环的集合对象中存在大量的元素，这时对栈内存而言是一种极大的开销，对于这种需要多次修改的字符串，Java提供了StringBuffer类，Ruby提供了StringIO类，我们仍旧以Ruby为例来演示StringIO的效果。

```ruby
#Ruby StringIO类降低字符串修改的内存开销
require 'stringio'
a = ["a", "b", "c"]
s = StringIO.new
s << "Array a contains: "
a.each do |i|
	s << i
	puts "address of s is #{s.object_id}"
end
puts s.string
# output
# address of s is 70280643727180
# address of s is 70280643727180
# address of s is 70280643727180
# Array a contains: abc
```
可以看出这时s指向的是同一块地址，这样就可以避免频繁修改同一字符串变量造成栈内存大量开销的问题了。

#### 引用类型深度拷贝

有时候我们在使用一个引用类型时希望操作后能保证原引用是不可破坏的（值不会被修改），这个时候我们就要用到拷贝，拷贝分为浅拷贝和深度拷贝，简单的说浅拷贝是只拷贝引用类型的第一层（假设一个对象的一个属性b也是对象，那么浅拷贝的对象产生修改时是无法保证b对象的属性不被破坏的），而深度拷贝会逐层拷贝这个引用类型的所有引用类型的属性。

可惜的是无论是Java还是JavaScript都没有提供深度拷贝的API，所有只能自己编写或者使用第三方库来实现深度拷贝。

当然最常用的实现思路是遍历这个对象的每一个属性，如果是引用类型则继续遍历这个引用，如果是基础类型就拷贝。另外一种相对简单但不稳定的思路是将对象序列化为JSON串，然后拷贝这个字符串，再反序列化为对象。我们采用第二个思路来看如下例子。

```javascript
//JavaScript 序列化方式实现深度拷贝

function deepClone(obj) {
  return JSON.parse(JSON.stringify(obj));
}
var o = {
  a: { b: 1, c: 2 },
  d: 3
}
var b = deepClone(o);

console.log(b);    
//{ a: { b: 1, c: 2 }, d: 3 }
```