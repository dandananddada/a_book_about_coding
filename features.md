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

在Java语言中通过泛型来实现变量多态，当然动态类型语言在运行时适配参数的数据类型也能使同一定义的函数根据参数类型的不同表现为返回不同结果。
```java
//Main.java

interface Runner{  String run(); };

class Pet<T extends Runner>{
  private T pet;
  public Pet(T pet){
    this.pet = pet;
  }
  public String run(){
    return pet.run();
  }
}
class Dog implements Runner{
  public String run(){
    return "run with four legs";
  }
}
class Duck implements Runner{
  public String run(){
    return "run with two legs";
  }
}

public class Main{
  public static void main(String args[]){
    Pet<Dog> dog = new Pet<Dog>(new Dog());
    Pet<Duck> duck = new Pet<Duck>(new Duck());
    System.out.println(dog.run());     //=>run with four legs
    System.out.println(duck.run());    //=>run with two legs   
  }
}
```
我们先过下这段代码的逻辑。
1. 我们声明了一个Runner的接口，接口中定义了一个跑的行为。
2. 然后我们创建了Dog类和Duck类，他们都实现了Runner接口的run行为。
3. 接着我们定义了一个Pet类，这个类需要传递一个实现了Runner接口的泛型（类）。
4. 在Pet类定义了一个跑方法，这个跑方法实际上返回的是实现了Runner接口的T的run方法。
5. 执行主函数，声明了两个Pet类，一个传入的T是Dog，另一个是Duck，分别调用这两个Pet的run方法。

**子类多态**

```java
//Main.java
class Pet {
  String run(){
    return "run with legs";
 };
}
class Dog extends Animal {
  String run() {
    return "run with four legs";
  }
}
class Duck extends Animal {
  String run() {
    return "run with two legs";
  }
}
public class Main2{
  public static void main(String args[]){
    Dog dog = new Dog();
    Duck duck = new Duck();
    System.out.println(dog.run());     //=>run with four legs
    System.out.println(duck.run());    //=>run with two legs
  }
}
```


