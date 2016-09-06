# 散列类型

散列是由键值对组成的集合类型，拿他与数组比较的话，可以理解为数组是以顺序数字为键的散列，散列是索引为字符串的数组。

在Ruby中散列是作为基本数据类型定义的，在JavaScript中对象类型可以看做数散列结构（如果不是特别细化的定义，也可以说JavaScript散列是数据类型），在Java中同List、Set类似，散列是实现Map接口的类。

```ruby
#ruby
language = { "name"=> "ruby", "type"=> "dynamic" }
puts language["name"]    #=>ruby
```

```javascript
//javascript
var language = { name: "javascript", type: "dynamic" }
console.log(language.name);    //=>javascript
```

```java
//java
Map<String, String> language = new HashMap<String, String>(){
  {
    put("name", "java");
    put("type", "static");
  }
};
System.out.println(language.get("name"));    //=>java
```

注意散列和数组一样，是引用类型，在使用时要特别小心，如果不想破坏原引用最好进行深度拷贝。

```ruby
#ruby
a = language
a["name"] = "Ruby"
puts language["name"]    #=>Ruby
```

```javascript
var a = language;
a.name = "JavaScript";
console.log(language.name);    //=>JavaScript
```

```java
//java
Map<String, String> a = new HashMap<String, String>();
a = language;
a.put("name", "Java");
System.out.println(language.get("name"));    //=>Java
```

**键值改变**

这不打脸么，下面写错了。。。。

在使用散列时保证键值的不变是很重要的，散列赋值后键值是不会改变的，但是如果我们通过变量定义散列的键，就会存在一种额外的风险。

我们以JavaScript为例说明。

```javascript
//javascript
var name = "name"
,   language = {};
language[name] = "javascript";
console.log(language, language[name]);
//=>{ name: 'javascript' } 'javascript'
name = "rename"
console.log(language, language[name]);
//=>{ name: 'javascript' } undefined
```

这个例子可以看出我们用一个变量作为一个对象（散列）的键，当我们修改了作为键的变量后，仍旧用这个变量获取对象（散列）的值：`language[name]`，这是因为不存在键`rename`的值，所以会返回`undefined`。

在Ruby中可以使用符号来作为散列的键，符号是不可修改类型，这样就可以避免如上问题了。

```ruby
#ruby
language = { :name=> "ruby", :type=> "dynamic" }
# 使用散列作为键就很好的避免了使用不存在的键取值的问题
```

另外Ruby中用符号作为键还有另外一个优势，因为符号是同一个引用，所以这样的散列相比字符串索引运算性能要高。

```ruby
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

同理Java下键值通过变量来存储，也会像之前的JavaScript一样返回`null`值。

```java
public static void main(String args[]){
  Map<String, String> language = new HashMap<String, String>();
  String name = "name";
  language.put(name, "Java");
  System.out.println(language +" "+ language.get(name));  //>{name=Java} Java
  name = "rename";
  System.out.println(language +" "+ language.get(name));  //>{name=Java} null
}
```
同Ruby类似，我们可以利用匿名内部类传入形参不可变的特性来保证键值不可修改。

```java
public static void main(String args[]){
  String name = "name";
  Map<String, String> language = new HashMap<String, String>(){
    {
      put(name, "java");     
      //>error: local variables referenced from a inner class must be final or effectively final
      put("type", "static");
    }
  };
  name = "NAME";  
}
```
上面可以看出如果你试图修改匿名内部类中引用的变量`name`，程序在编译时就会抛出一个编译错误。