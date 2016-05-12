# 散列

散列是由键值对组成的集合类型，拿他与数组比较的话，可以理解为数组是以顺序数字为键的散列，散列是索引为字符串的数组。

在Ruby中散列是作为数据类型，在JavaScript中对象类型可以看做数散列结构（如果不是特别细化的定义，也可以说JavaScript散列是数据类型），Java中同List、Set类似，散列是实现Map接口的类。

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
在使用散列时保证键值的不变是很重要的，散列赋值后键值是不会改变的，但是如果我们通过变量定义散列的键，就会存在一种额外的风险（Ruby和JavaScript表现一致，Java不会存在这种情况）。我们以JavaScript为例说明。
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
这个例子可以看出我们用一个变量作为一个对象（散列）的键，当我们修改变量值时对象的键不会改变，但是当我们用通过这个修改后的变量取得散列中的值时会发生意外。可能这段代码看起来很荒唐，但实际开发中你无法保证一个散列的键不是变量，也无法保证自己不会修改这个变量并仍旧以它作为键来取值。

在Ruby中最好使用符号来作为散列的键值，因为符号是不可修改类型，这样就可以人为的避免这样的问题了。Java中可以用Static类型修饰变量，但事实上Java的HashMap本身就不会存在这种问题（稍后会说明），所以不必担心。至于JavaScript语言本身没有好一些的防范机制，只能通过工开发人员定制规范来避免了（注意在ES6中提出了不可变量）。
```ruby
#ruby
language = { :name=> "ruby", :type=> "dynamic" }
# 使用散列作为键就很好的避免了使用不存在的键取值的问题
```
另外Ruby中用符号作为键还有另外一个优势，因为符号是同一个引用，所以这样的散列相比字符串索引运算性能要高（至于Java用同一个字符串引用作为键会不会有性能提高我没有测试，就不做对比了）。
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
```
