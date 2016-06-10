# 面向对象特点

####多态

多态指的是相同的消息传递给不同的对象会产生不同的动作，比如Animal类下有duck对象和dog类型，我们给他们一个run的消息来让他们跑，这时鸭子和狗所表现出的动作就是不同的。
  


** 函数多态（Ad hoc polymorphism）**

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

** 变量多态（parametric polymorphism）**

参数多态允许函数或数据类型采用同一定义，但是可以处理不同类型的变量。变量多态在保证静态类型安全的情况下使语言的表述更加清晰简明。

在Java语言中通过泛型来实现变量多态，当然动态类型语言在运行时适配参数的数据类型也能使同一定义的函数根据参数类型的不同表现为返回不同结果。

```java
/*  Java泛型作为参数使用  */
public class Tree<T>{
  private T value;
  private Tree<T> left;
  private Tree<T> right;
  
  public void replaceAll(T value){
    this.value = value;
    if(left != null)   left.replaceAll(value);
    if(right != null)  right.replaceAll(value);
  }
}
```

**子类多态（Subtyping）**

一些语言提供了子类多态的概念，子类多态允许你在定义函数的时候使用父类做形参，但实际使用的实参可以是其子类。

```java
//Main.java
/*  子类多态  */
abstract class Pet {
  abstract String run();
}
class Dog extends Pet {
  String run() {
    return "run with four legs";
  }
}
class Duck extends Pet {
  String run() {
    return "run with two legs";
  }
}
public class Main{
  public static void petRun(Pet p){
    System.out.println(p.run());
  }
  public static void main(String args[]){
    Main.petRun(new Dog());     //=>run with four legs
    Main.petRun(new Duck());    //=>run with two legs
  }
}
```
上面的Java代码中定义了petRun方法，形参为Pet类，但实际调用时传递的为Pet的子类Dog和Duck，这时p.run则分别调用了子类的run方法。

接下来我们看一个泛型和子类多态组合使用的例子。

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


####封装

封装是通过一种结构（这里通常为类）将数据和函数包装起来，通常有以下两点用法：
1. 将数据、函数的实现细节隐藏，简化使用。
2. 防止外界直接访问包装内的数据、函数。

简单的说这两点都是针对数据而提出的设计，一个是方便数据的修改，一个是保证数据修改的安全性。

**访问级别（Access Level）**

访问级别用来控制一个类的属性或方法能否被其他类访问，以Java的访问级别为例来说明包括：private、protected、public和不定义(no modifier)这四种，其对应的访问权限如下表(Y表示可访问，N表示不可)

| 修饰符/场景 | 当前类 | 当前包 | 父类 | 全局 |
| -- | -- | -- | -- | -- |
| public| Y | Y | Y | Y |
| protected | Y | Y | Y | N |
| no modifier | Y | Y | N | N |
| private | Y | N | N | N |

也就是假设A是B的父类，那么如果A中的一个属性a是用private修饰的话那么B实例化的对象是没有b这个属性的。
```java
//java
class A {
  protected String a = "protected prop can access subclass";
  private int b = "private prop can not access in subclass";
}

class B extends A {}

public class Main{
  public static void main(String args[]){
    B b = new B();
    System.out.println(b.b);    //=>error: b has private access in A
  }
}
```
如果你还记得面向对象模式说明中静态类模式下有一个弊端就是：子类只能继承父类全部属性，不能动态选择。当时我提到了访问权限，像Java和Ruby都通过这种机制解决了继承关系属性冗余的问题。因此访问级别是静态模式下用来弥补缺点的一个设计，而在基于原型的模式（比如JavaScript）下没有提出访问权限的概念也是正常的。

当然如果你想在JavaScript里定义私有属性也是可以的，只要认为规定好一个规则，比如通用的用`_`为开头命名的变量。保证在外部访问变量时避免访问这些以`_`为开头的变量就可以了。

继续刚才的例子，如果你就像用B来访问A的私有属性b，那么你可以在A中定义一个public/protected方法，然后返回b。
```java
//java
class A {
  protected int a = 1;
  private int b = 2;
  protected int getB(){
    return this.b;
  }
}

class B extends A {}

public class Main{
  public static void main(String args[]){
    B b = new B();
    System.out.println(b.getB());    //=>2
  }
}
```
或者通过Java的反射机制获取。
```java
//java
import java.lang.reflect.*;
class A {
  private int b = 2;
}

public class Main{
  public static void main(String agrs[])throws Throwable {
    A a = new A();
    Field field = a.getClass().getDeclaredField("b");
	field.setAccessible(true);
	Object b = (int)field.get(a);
	System.out.println(b);    //=>2
  }
}
```


像Python和Ruby提供了直接访问私有属性的语法。
```ruby
#ruby
class A
  def initialize
    @a = 10
  end
 
  private
  def private_method(b)
    return @a + b
  end
end
a = A.new
puts a.send(:private_method, 20)     #=>30
puts a.instance_variable_get(:@a)    #=>10
```

####多重继承

一个类可以继承自一个父类，那么它也可以继承自多个父类，这就是多重继承。但是多重继承存在很多的问题，最典型的问题就是菱形继承问题（diamond problem）。

![菱形继承问题](images/diamond_problem.png)
如图B、C都继承自A，并且重写了A的metho的方法，而D多重继承自B和C，D却没有重写method方法，那么这时D的method方法是以B为准，还是以C为准就形成了菱形继承问题。

但多重继承的特性又是不可或缺的，我们还是以铅笔为例，它即属于文具，又是木制品，那么它要满足这两点，就需要继承文具类和木制品类，但是已经说过多重继承是存在缺陷的。那么该如何处理这种需求呢？

不同的语言提出了不同的方案。

**混合（Mixin）**

Mixin也是一种代码复用的策略，任何对象都可以通过Mixin拓展方法，同类的多重继承一样，一个对象可以引用多个Mixin。

Ruby就是采用Mixin的方式实现多重继承。
```ruby
module Action
  def jump
    "jump with legs"
  end
end

module Sound
  def say
    "say #{@sound}"
  end
end

class Cat
  def initialize
    @sound = "mew"
  end
  include Action
  include Sound
end

cat = Cat.new
puts cat.jump    #=>jump with legs
puts cat.say     #=>say bark
```
上面我们定义了两个Mixin，一个用于描述动作，一个用于描述叫声。我们只需要在定义类是通过`include`关键字引入Mixin就可以使用其中定义的方法了。

注意在Ruby中如果类中有和module（Mixin）中同名的方法，以类中的方法体为准，如果多个module定义了同一方法，以最后拓展（include）的那个为准。这样的设计就避免了多重继承中菱形继承的问题。

**Trait**
Trait和Mixin类似也是用来拓展类的方法的，只是Trait中不允许实例化对象，同时如果引用的两个Trait存在同名函数，那么将不会采取任何策略解决这个问题，而会直接返回冲突异常。

我们来看下Scala中Trait的使用。
```scala
class Person
trait Nice{
  def greet() = println("Hello")
}

class Character extends Person with Nice

val american = new Character()
american.greet    //=>Hello
```
上面我们定义了一个父类Person以及一个Trait，然后定义了一个Character类，继承自Person，同时使用了Trait拓展。这样Character在没有定义任何方法时也是可以访问Nice的greet方法的。