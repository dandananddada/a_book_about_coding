# 函数类型

####构造函数
构造函数是类在实例化（构造）一个对象时执行的函数。

在Java中构造函数与类名同名，你可以显示的定义构造函数，并执行一些赋值操作。
```java
//java
/* 显式定义构造函数并执行赋值操作 */
class Demo{
  public int a, b;
  public Demo(int a, int b){    //构造函数
    this.a = a;
    this.b = b;
  }
  public static void main(String args[]){
    Demo d = new Demo(1, 2);
    System.out.println(d.a +","+ d.b);    //=>1,2
}
```

在ruby中类的构造函数名都是initialize，同样你也可以显示的定义，并进行赋值操作。
```ruby
class Demo
  attr_accessor :a, :b    #=>配置属性可读写
  def initialize(a, b)	
    @a = a
    @b = b
  end
end
d = Demo.new(1, 2)
puts d.a, d.b    #=>1 2
```

####实例方法和类方法
对于面向对象语言而谈，实例方法和类方法是不得不说的一个概念。

简单的说实例方法是属于对象的，而类方法是属于类的，也就是说前者通过
对象调用，后者通过类可以直接调用。

在Java中类级别的方法或者属性通过`static`修饰符标明。通常类方法用于获取类属性。

```java
//java
/* 通过getInstanceNumbers得到Car类实例化对象的个数 */
class Car{ 
  public static int instanceNumbers;
  public static int getInstanceNumbers(){
    return instanceNumbers;
  }
  public Car(){
    instanceNumbers++;
  }
  public static void main(String args[]){
    Car c  = new Car();
    System.out.println(Car.getInstanceNumbers());    //=>1
  }
}
```
Ruby通过self（指向当前运行时的上下文，在方法定义时self指向的其实就是类本身）来定义类方法。

```ruby
#ruby
class Car
  @@instance_numbers = 0    #ruby中类属性用@@声明
  def initialize()
      @@instance_numbers += 1
  end
  def self.get_instance_numbers
      @@instance_numbers
  end
end
c = Car.new
puts Car.get_instance_numbers    #=>1
```
####私有方法

public protected private



####匿名函数
匿名函数是无需显示声明名称的函数，一般作为高阶函数（参数或返回值类型为函数的函数）的参数或者返回值使用。
一般对于基本不会复用或使用次数有限的函数可以用匿名函数。匿名函数可以简化代码调用起来更便捷。

```javascript
//JavaScript匿名自执行函数
console.log(function(){ return 2 }());    //=>1
```
我们在输出语句里传入了一个匿名函数的定义，紧接着用调用运算`()`符执行了这个匿名函数。

Java本身是不支持匿名函数的，但是可以通过匿名内部类实现匿名函数的功能。
```java
//java
class Demo{
  public static void main(String args[]){
    new Object(){
      void add(int a, int b){  System.out.println(a + b); }
    }.add(1, 2);
  }
}
```
上面我们在输出方法中实例化了一个匿名内部类，并在类中定义了add方法，紧接着用这个实例化后的对象调用了add方法。

在Java8中加入了lambda，所以如果使用Java8以后版本可以通过lambda表达式替代匿名内部类。

 ```java
 //java
 
 ```
