# 函数类型

####构造函数
构造函数是类在实例化（构造）一个对象时执行的函数。

在Java中构造函数与类名同名，你可以显示的定义构造函数，并执行一些赋值操作。
```java
//java
/* 显式定义构造函数并执行赋值操作 */
class Demo{
  public int a, b;
  public Test(int a, int b){    //构造函数
    this.a = a;
    this.b = b;
  }
  public static void main(String args[]){
    Test t = new Test(1, 2);
    System.out.println(t.a +","+ t.b);    //=>1,2
}
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



####匿名函数
