# 散列类型

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
