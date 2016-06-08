# 面向对象特点

####多态

多态指的是相同的消息传递给不同的对象会产生不同的动作，比如Animal类下有duck对象和dog类型，我们给他们一个run的消息来让他们跑，这时鸭子和狗所表现出的动作就是不同的。
  


函数多态（Ad hoc polymorphism）

函数多态允许传入不同类型的参数，并且根据参数类型的不同，返回值也不同。常见的比如函数重载和操作重载。

```java
//java
/*  函数重载，根据传入的参数类型不同，add函数返回的结果也不同.  */
class Demo{

  public  static int add(int a, int b){
    return a + b;
  }
  public  static String add(String a, String b){
    return a + b;
  }

  public static void main(String args[]){
    int result1 = Demo.add(1, 2);
    String result2 = Demo.add("String ", "combined!");
    System.out.println(result1);    //=>3
    System.out.println(result2);    //=>String combined!
  }
```

Java本身不提供运算符重载，而Ruby的运算本身就是通过定义一个名为操作符的函数实现的，所以Ruby的运算符重载非常方便。
```ruby
#ruby
# 运算符重载，通过定义+方法实现运算符重载.
class Demo
 def initialize a
   @a = a
 end
  def +(b)
    "plus b equal to" + (@a+b).to_s
  end
end

a = Demo.new(2)
puts a + 2
```

变量多态（parametric polymorphism）

参数多态允许函数或数据类型采用同一定义，但是可以处理不同类型的变量。变量多态在保证静态类型安全的情况下使语言的表述更加清晰简明。

在Java语言中通过泛型来实现变量多态，当然动态类型语言在运行时适配参数的数据类型，弱类型语言在处理时转换数据类型也能使同一定义的函数根据参数类型的不同表现为返回不同结果。


