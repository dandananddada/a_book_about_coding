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