# 原子类型

原子类型是函数式语言中常用的数据类型，它就像是一个常用的、不变的字符，因为有些字符会被多次使用，对于这样的字符多次赋值给不同的字符串有些不必要，同时也存在内存的浪费。所以一些语言中加入了原子类型专门存储这种常用的字符串文本。

Ruby中用:修饰原子类型，比如:red就是文本值为red的原子类型（注意Ruby中原子类型其实叫做符号类型symbol）。
Erlang中用''单引号包裹表示原子类型，比如'red'就是文本值为red的原子类型（Erlang中原子类型用单引号包裹，字符串类型用双引号包裹）。

```ruby
#Ruby 字符串类型与原子类型比较

string_a = "string"
string_b = "string"
symbol_a = :string
symbol_b = :string

puts string_a.object_id    #70329489970500
puts string_b.object_id    #70329489970480
puts symbol_a.object_id    #158248
puts symbol_b.object_id    #158248
```
可以看出虽然都是相同的文本字符，但是用字符串类型会分别存储，而符号类型则会指向同一个存储地址，对于一个不变的文本信息用符号类型可以降低内存的开销，并提高程序性能。

另外symbol也常作为对象的key来使用，因为key是不允许改变的，而且同一模板下的对象key值也是相同的。
```ruby
#Ruby 用符号表示散列的键

prices = {
  :banana => 2,
  :apple  => 3,
  :pear   => 2.5
  #ruby中用=>绑定Hash键值对。
}
```