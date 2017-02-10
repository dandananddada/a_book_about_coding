# 第五章 命令式下的面向对象概念

第四章引入命令式和声明式概念后，在本章将主要说明命令式范式下的面向对象语言。

* 介绍面向对象语言中对象相关概念及对象实现的三种常见模式。
* 介绍面向对象语言的特点，及相关概念。
* 介绍面向对象语言为处理常见问题而引入的设计模式概念。


### 面向对象及模式

在说明面向对象之前先简单说明下什么是对象。

##### 对象

对象本身也是一种数据类型，所以它本质上是一段存储着值的内存地址。它与其他数据类型的区别在于它将数据和方法打包成一个整体，即对象内部包含一定的数据（值类型或引用类型）和对数据操作的方法（函数）。所以对象是涵盖多个数据类型的数据类型。

##### 面向对象

面向对象就是通过对象将程序组织成一个整体，对象之间存在着相互的关系，可以访问修改关联对象的数据，这样做的好处是提高了代码的复用性、灵活性和拓展性，使程序更易于维护和拓展。

接下来我们看看对象在实现上常用的几种模式。

#### 静态类模式

实例和类：类是一系列实例的抽象，实例则是类的一个具现。

实例中通常包括属性（对象的描述）和方法（对象的行为），在静态类模式下实例所拥有的属性和方法是由类的模板定义的，而且模板一旦定义后是不允许再次修改的。

```Java
//伪代码 类与实例

C = class{ int:a, int:b, void:method }
c1 = { a: 10, b: 20 }
c2 = { a: 20, b: 30 }
```
类模式通过类的继承来实现代码复用，A继承自B，那么A就拥有B的属性和方法，B继承自C，那么A、B都有C的属性和方法。如果尝试取得A的一个属性/方法：a，那么首先会在A中寻找a，如果不存在就去B中寻找，如果B中也不存在则去C中寻找，如果C中还不存在说明A不存在这个属性/方法。

另外要说明的是如果A继承自B，那么A会把B的所有属性都拷贝一遍（即使A类可能根本不需要这个属性，但是方法不会拷贝，方法是引用），所以如果A中某个属性与B的同名，则会以A的这个属性为准，只有A中没有的属性才会到B中寻找。

##### 静态模式特点

* 需要定义一个对象的模板：类。
* 根据模板来创建对象，对象中包含属性和方法。
* 类定义后无法修改，对象的属性和方法依赖于类。
* 类的继承使子类包含父类的所有属性和方法（有些语言提供了访问权限解决了这个问题）。
* 实例无法拥有自己特有的属性和方法。


#### 原型模式

动态和可拓展对象：没有类模板概念，通过对象本身定义属性和方法。

```Java
//伪代码 对象拓展

object1 = { a: 10, b: 20 }
object2 = { a: 20, b: 30 }
```

对象可以动态的添加、修改属性和方法。如果我们尝试取得a的一个属性/方法b，a会检查是否存在b，如果存在则更新b，如果不存在则创建b。对象模式通过原型来实现代码的复用，原型也是一个对象（原型是对象的一个属性）。任何一个对象都可以作为其他对象的原型来使用。

```Java
//伪代码 一个对象作为另一个对象的原型实现继承

object1 = { a: 10, b: 20, method1 }
object2 = { a: 20, c: 30, method2 }
object2.[[Prototype]] = object1

show object2    
//{ a: 20, b: 20, c: 30, method1, method2 }

delete object2.a
object2.d = 40

show object2
//{ a: 10, b: 20, c: 30, d: 40, method1, method2 }
```
一个作为原型的对象也可以指定其他对象作为原型，如果一个对象a以b为原型，b以c为原型，则abc就构成了一个原型链，原型链的概念就类模式下的继承一样。不同点在于，因为对象是可拓展的，所以原型链的继承关系和结构是有是会动态改变的。另外除了通过指定原型属性外还有另外一种代码复用的实现，就是完全拷贝，这样做的好处是修改不会对原型对象产生影响，但需要承担更多的内存开销。

在静态类模式下我们是通过类模板来描述对象的，比如说类模板定义了class Car，那么Car的实例很明显都是汽车，而原型模式下则是通过鸭子类型（如果一个对象表现的行为是car，那么它就是Car）来表述这一概念的，简单的说当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。在鸭子类型中关注的不是对象本身是否是鸭子，而是它在使用上是否像鸭子。

```Java
//伪代码 鸭子类型：如果一个对象表现出鸭子的特性，那么它就是个鸭子

object1.active == 'duck' ? 'object1 is duck' : 'object1 is not duck'
object2.active == 'duck' ? 'object2 is duck' : 'object2 is not duck'
```
**原型模式特点**

* 对象本身是动态的、可拓展的。
* 对象本身不需要模板来定义属性和方法。
* 对象可以通过原型实现代码的复用，同样原型也是随时可以改变的。
* 当原型发生改变时，会影响到原型链下层的对象。
* 对象不通过特定的类型或继承关系定义，而是通过当前所拥有的特性（鸭子类型）定义。


#### 动态类模型
上述的两种模式可以对比为类和原型，通常静态语言常使用类模式，而动态语言常使用原型模式。当然也存在像Python或Ruby这样的语言，他们属于动态类型语言，却使用类模式，这种模式就叫作动态类模型。动态类模型通过类模板来规范对象的属性和方法，但又可以在运行时改变类的定义和对象的原型。我们来看下面的例子：

```ruby
#Ruby 对类拓展属性

class Demo
  attr_accessor :a
end

d = Demo.new
d.a = 1
d.b = 2
#undefined method `b=' for #<Demo:0x007fe81a9caa68 @a=1> (NoMethodError)

# 给Demo类拓展属性b
Demo.class_eval do
  attr_accessor :b
end

d.b = 2
puts d.a, d.b
#1
#2
```
可以看出Ruby中可以通过class_eval来拓展类的定义，这种采用对象和类的方式组织数据，但却允许在程序运作过程中动态改变类的定义的模式就是动态类模式。


### 面向对象特点

#### 多态

多态是指同一个消息传递给不同的对象时会产生不同的动作，比如Animal类下有duck和dog两个对象，当他们收到相同的消息run后，表现出跑的动作却是不同的。对多态有了基本的印象后，我们从如下几个方面展开说明：函数多态、变量多态、子类型多态。

##### 函数多态（Ad hoc polymorphism）

函数多态接受不同参数类型，并根据不同的参数类型返回不同的值。函数重载和操作重载是最常见的函数多态。

```java
//Java 函数重载

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

    System.out.println(result1);    //3
    System.out.println(result2);    //String combined!
  }
```

Java本身不提供运算符重载，而Ruby的操作符运算本身就是一个函数，所以Ruby中的运算符重载只需要重载函数即可。

```ruby
#Ruby 运算符重载

class Demo
 def initialize a
   @a = a
 end
  def +(b)
    "plus b equal to" + (@a+b).to_s
  end
end

a = Demo.new(2)
puts a + 2    #4
```

##### 变量多态（parametric polymorphism）

参数多态允许函数或数据类型采用同一定义，但是可以处理不同类型的变量。变量多态在保证静态类型安全的情况下使语言的表述更加清晰简明。在Java语言中通过泛型来实现变量多态，当然动态类型语言在运行时会自动适配参数的数据类型，因此实现上加简洁。

```java
//Java 泛型

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

##### 子类型多态（Subtyping）

一些语言提供了子类型多态的概念，子类型多态允许你在定义函数的时候使用父类型做形参，但实际使用的实参可以是其子类型。

```java
//Java 子类型多态

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
    Main.petRun(new Dog());     //run with four legs
    Main.petRun(new Duck());    //run with two legs
  }
}
```
上述代码中定义了petRun方法，形参为Pet类，实参为Pet类的子类Dog或Duck，因此实际上调用的run方法是子类的一个实现。这里需要额外说明下，子类型多态和继承是两个不同的概念，子类型多态强调的是类型之间的关系，即子类型可以替代父类型；而继承强调的是对象间的关系，即一类对象可以从另一类对象衍生出来。

最后我们通过变量多态和子类型多态组合使用的例子来加深对多态的认识。

```java
//Java 多态

interface Runner{  String run(); };

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

class Pet<T extends Runner>{
  private T pet;
  public Pet(T pet){
    this.pet = pet;
  }
  public String run(){
    return pet.run();
  }
}

public class Main{
  public static void main(String args[]){
    Pet<Dog> dog = new Pet<Dog>(new Dog());
    Pet<Duck> duck = new Pet<Duck>(new Duck());

    System.out.println(dog.run());     //run with four legs
    System.out.println(duck.run());    //run with two legs   
  }
}
```
我们整理下上述代码的逻辑：

1. 首先声明了一个Runner的接口，接口中定义了一个名为run的行为。
2. 然后创建了Dog类和Duck类，他们分别实现了Runner接口的run行为。
3. 接着定义了一个Pet类，这个类需要传递一个实现了Runner接口的泛型（类）。
4. 在Pet类定义了一个run方法，这个方法返回的是实现了Runner接口的类型T的run方法。
5. 执行主函数，声明了两个Pet类，一个传入的类型T是Dog，另一个是Duck，最后分别调用这两个Pet的run方法。

#### 封装

封装是通过类将数据和函数包装起来，封装具有如下作用：

* 将数据、函数的实现细节隐藏，简化使用。
* 防止外界直接访问包装内的数据、函数。

与封装密切相关的一个概念是访问级别，它定义了外部访问包装内数据和函数的规则。

##### 访问级别（Access Level）

访问级别用来控制一个类的属性或方法能否被其他类访问，以Java的访问级别为例来说明包括：private、protected、public和no modifier这四种，其对应的访问权限如下表（Y表示可访问，N表示不可）。

| 修饰符/场景 | 当前类 | 当前包 | 子类 | 全局 |
| -- | -- | -- | -- | -- |
| public| Y | Y | Y | Y |
| protected | Y | Y | Y | N |
| no modifier | Y | Y | N | N |
| private | Y | N | N | N |

假设A是B的父类，如果A中的一个属性p是用private修饰的话，那么B是无法访问这个属性的。相对的如果属性p是用public修饰的，那么B就可以访问这个属性。

```java
//Java 访问权限

class A {
  protected String a = "protected prop can access subclass";
  private int b = "private prop can not access in subclass";
}

class B extends A {}

public class Main{
  public static void main(String args[]){
    B b = new B();
    System.out.println(b.b);    
    //error: b has private access in A
  }
}
```

JavaScript中没有提供私有属性的概念，通常人为的把以`_`开头的量作为私有属性，同时避免在对象外部直接访问以`_`开头的变量。

继续刚才的例子，如果你就想用B来访问A的私有属性p，那么你可以在A中定义一个用public/protected修饰的方法，然后通过这个方法返回p。

```java
//Java 通过公开的方法访问私有属性

class A {
  private int p = 1;
  protected int getP(){
    return this.p;
  }
}

class B extends A {}

public class Main{
  public static void main(String args[]){
    B b = new B();
    System.out.println(b.getP());    //1
  }
}
```
或者通过Java的反射机制获取。
```java
//Java 反射

import java.lang.reflect.*;
class A {
  private int p = 1;
}

public class Main{
  public static void main(String agrs[])throws Throwable {
    A a = new A();
    Field field = a.getClass().getDeclaredField("p");
    field.setAccessible(true);
    Object b = (int)field.get(a);
    System.out.println(b);    //1
  }
}
```
Python和Ruby则提供了直接访问私有属性的语法糖。

```ruby
#Ruby 访问私有成员

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
puts a.send(:private_method, 20)     #30
puts a.instance_variable_get(:@a)    #10
```

#### 继承

类或对象继承自另一个类或对象就能具备其属性和方法，通常继承分为三种：单继承、多重继承和多级继承。B继承自A这种叫做单继承；C继承自A和B叫做多重继承；C继承自B，B继承自A叫做多级继承。单继承和多级继承本身不存在什么问题，但多重继承却存在很多的问题，最典型的问题就是菱形继承问题（diamond problem）。

<img src="/images/diamond_problem.png" alt="菱形继承" style="height: 320px; width: 320px; float: right; margin: 0px 40px">

如图B、C都继承自A，并且重写了A的method的方法，而D继承自B和C，同时D没有重写method方法，那么这时D的method方法究竟该继承自B，还是继承自C，这就是菱形继承问题。

因为多重继承暴露出太多的问题，所以多数语言规定不允许多重继承，但多重继承又是不可或缺的功能，比如铅笔即属于文具，又是木制品，如果它要满足这两点，就需要继承文具类和木制品类，那么该如何处理这种需求呢？

不同的语言提出了不同的方案：

##### 混合（Mixin）

Mixin也是一种代码复用的策略，任何对象都可以通过Mixin拓展方法，与类的多重继承一样，一个对象可以引用多个Mixin。Ruby就是采用Mixin的方式实现多重继承的代表。

```ruby
#Ruby 混合

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
puts cat.jump    #jump with legs
puts cat.say     #say mew
```

上面我们定义了两个Mixin，一个用于描述动作，一个用于描述叫声。我们只需要在类的定义时通过include关键字引入Action和Sound就可以调用相关的方法了。

注意Ruby的类中如果存在和module（Mixin）里定义的同名的方法，则以类中的方法体为准，如果多个module定义了同一方法，就以最后拓展（include）的那个为准。Ruby正是通过这种设计避免了多重继承中的菱形继承问题。

##### 特征（Trait）

Trait和Mixin类似也是用来拓展类的方法的，只是Trait中不允许实例化对象，同Ruby的Mixin不同，如果两个Trait中存在同名函数，那么将不会采取任何策略解决这个冲突，而会直接抛出异常。

我们来看下Scala中Trait的使用。
```scala
//Scala Trait

class Person
trait Nice{
  def greet() = println("Hello")
}

class Character extends Person with Nice

val american = new Character()
american.greet    //Hello
```

上面我们定义了一个父类Person以及一个Trait，然后定义了一个Character类，让他继承自Person，同时使用了Trait拓展。这样Character在没有定义任何方法时也是可以访问Nice的greet方法的。

##### 接口（Interface）

接口和Mixin、Trait类似，也是用来给类拓展方法的，与Mixin、Trait不同的是，接口只定义方法，但不实现。接口的实现由接口的实现类完成。

```java
//Java 接口

interface Move{
  String jump();
}
interface Sound{
  String say();
}
class Cat implements Move, Sound{
  public String jump(){
    return "jump with legs";
  }
  public String say(){
    return "mew";
  }
}

public class Main{
  public static void main(String args[]){
    Cat cat = new Cat();
    System.out.println(cat.jump());    //jump with legs
    System.out.println(cat.say());     //mew
  }
}
```
因为接口规定了实现由类完成，所以多个接口中存在同名函数时也是不会有任何问题，因为类中同一函数的实现肯定是唯一的。

**接口与抽象类对比**

Java中的接口和抽象类不同，抽象类的抽象方法和接口类似。在只需要拓展一个接口的情况下可以用抽象类取代接口，但需要拓展多个接口的时候抽象类是无法取代的，所以接口主要处理的问题还是多重继承。

##### 组合（composition）

组合也是一种实现多重继承的方式，组合是指一个类的属性是另外一个类，此时就可以通过当前类来调用另一个类的方法了，出于理解方便你可以把这个类看做是一个Mixins。一般我们称继承为is a关系，称组合为has a关系，即：

* A是B的一个子类，那么A自然是B，所以A is a B
* A通过组合引用了B，那么A中有B，所以A has a B

```java
//Java 组合

class Move{
  public String jump(){
    return "jump";
  }
}
class Sound{
  public String say(String word){
    return word;
  }
}

class Cat{
  String word;
  Move move;
  Sound sound;
  Cat(){
    this.word = "mew";
    this.move = new Move();
    this.sound = new Sound();
  }

  public String jump(){
    return this.move.jump();
  }
  public String say(){
    return this.sound.say(this.word);
  }
}

public class Main{
  public static void main(String args[])
  {
    Cat cat = new Cat();
    System.out.println(cat.jump());    //jump
    System.out.println(cat.say());     //mew
  }
}
```
我们来看下上面这段代码:

1. 首先定义了一个Move类，实现了jump方法。
2. 然后定义了一个Sound类，实现了say方法。
3. 接着定义Cat类，组合Move类和Sound类作为属性。
4. 重写Cat类的构造函数，实例化move对象和sound对象。
5. 在Cat类中定义jump方法和say方法，方法体内分别调用move的jump方法及sound的say方法。

#### 抽象

抽象用来隐藏复杂的底层细节，将关注点放在呈现的结果，一般而言抽象分两种：

1. 操作抽象，比如编程语言中的`+`运算符，你只需要知道它能够处理加法运算就行，不需要知道底层的实现细节。
2. 数据抽象，比如编程语言中的Number类型，你只需知道它用来存储数字类型就行，不需要知道底层的存储细节。

面向对象中的抽象也是用于隐藏实现细节的。在Java语言下抽象通过抽象（abstract）和接口（interface）两种方式实现。抽象和接口类似，它可以用来修饰类也可以修饰方法。通过abstract修饰的类叫做抽象类，修饰的方法叫做抽象方法。抽象类不能实例化，一般作为父类使用，抽象方法不需要定义方法体，方法体由子类实现。另外因为抽象方法需要子类来实现，所以abstract不能和private一起使用。

```java
//Java 抽象

abstract class Animal{
  public abstract String say();
}

class Cat extends Animal{
  public String say(){
    return "mew";
  }
}

public class Main{
  public static void main(String agrs[]){
    Animal animal = new Animal();    
    //error: Animal is abstract; cannot be instantiated

    Cat cat = new Cat();
    System.out.println(cat.say());     //mew
  }
}
```

抽象作为一种多态的实现机制对于Ruby这种动态类型的语言而言是不需要的，因为在动态类型语言中函数参数和返回值无需强制声明，解释器会根据类型推导机制自动判断。

```java
//Java 函数重载
public String getA(String a){
  return a;
}

public int getA(int a){
  return a;
}
```
```ruby
#Ruby 函数定义
def getA(a)
  return a
end
```

从上面例子可以看出Java这种静态类型语言需要显式的声明函数的参数类型和返回值类型。因为参数类型或返回值类型不同的同名函数被看作两个不同的函数，所以Java中抽象的概念是必要的。而在Ruby这种动态类型语言中，参数类型和返回值类型是程序在运行时动态决定的，函数本身就能够接收不同类型的输入参数，所以抽象类和抽象方法是没有必要的，这也是Ruby不支持抽象类和抽象方法的原因。

虽然Ruby本身没有提供对抽象的支持，但可以通过抛出异常的方式来模拟抽象方法和抽象类。

```ruby
#Ruby 模拟抽象方法

class Animal
  def say
    raise "abstract method"
  end
end

class Cat < Animal
  def say
    return "mew"
  end
end

c = Cat.new
puts c.say    #mew
```

我们在Animal里定义了一个say方法，该方法没有执行任何操作，只是抛出一个异常（防止直接调用这个方法），然后我们声明了个Cat类继承了Animal，并重写了say方法。这样通过抛出异常的方式限制父类的say方法直接调用，来模拟Java的抽象方法。

### 设计模式

设计模式（design pattern）是指对软件设计中常见的各种问题，提出一套通用可行的解决方案，从而避免重复设计解决方案来处理同一类问题。设计模式的提出可以提高代码的复用性、易读性、可靠性，使程序开发更加工程化。

#### 提出背景

* 1987年Kent Beck和Ward Cunningham参照建筑设计领域的思想提出了设计模式并应用在Smalltalk的图形接口上。
* 1989年Erich Gamma在他的博士论文里开始尝试把这种思想用于软件开发。
* 1989年Erich Gamma拿到博士学位后在美国与Richard Helm, Ralph Johnson, John Vlissides 合作出版了《Design Patterns》一书，书中提到了23种设计模式。
*经过20多年的发展，软件开发中套用模式已经成为一种流行的设计方法。

由于《设计模式》这本书的作者有四人，所以在软件开发领域称他们为四人帮（Gang of Four），简写作GoF，因此GoF也就成了软件设计模式的代称。

#### 分类

设计模式总体来说分为三大类：

* 创建型，共五种：工厂模式、抽象工厂模式、单例模式、建造者模式、原型模式。
* 结构型，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。
* 行为型，共十一种：策略模式、模板模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

#### 原则

* 开闭原则（Open Close Principle）

  开闭原则是指对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，从而使程序易于维护和升级。一般通过使用接口和抽象类来达到这个目的。

* 里氏代换原则（Liskov Substitution Principle）

  里氏代换原则是对“开-闭”原则的补充。实现“开-闭”原则的关键就是抽象化。而父类与子类的继承关系就是抽象化的一种具体实现。
  里氏代换原则下，任何父类都可以被它的子类替换，只有这样才能保证父类是可复用的。

* 依赖倒置原则（Dependence Inversion Principle）

  依赖倒置强调针对接口编程，不依赖于具体的实现类，而依赖于这个实现类的抽象（接口或抽象父类）。

* 接口隔离原则（Interface Segregation Principle）

  将可以拆分的接口拆分为多个，目的在与降低接口之间的耦合性，降低类与类，类与接口间的耦合性能使程序更易于维护。

* 迪米特法则（Demeter Principle）

  最少知道原则同接口隔离原则类似，最少知道原则是针对实体而言，强调实体间应该尽量少的相互作用，从而保证功能模块的独立，本质上还是降低耦合性使程序易于维护。

* 合成复用原则（Composite Reuse Principle）

  尽量使用组合的方式，而不是继承。


#### 几种设计模式

我不准备把23种设计模式一一列举，毕竟这不是一本讲设计模式的书，我只是想强调设计模式是面向对象编程下一个必不可少的要素。所以我将从中挑选出几个比较经典的设计模式进行介绍。


##### 工厂模式

在开发过程中经常需要根据不同的条件实例化不同的对象，每次都重复实现这个逻辑显然是不理想的，所以我们可以把这样的逻辑封装到一个类中，这个类就是工厂类，通过类和条件参数返回需要实例化的对象，这个模式就是工厂模式。

```Java
//Java 工厂模式

interface Language{
  public void sayHello();
}

class Java implements Language{
  public void sayHello(){
    System.out.println("say hello by Java");
  }
}

class Ruby implements Language{
  public void sayHello(){
    System.out.println("say hi by Ruby");
  }
}

class LanguageFactory{
  public Language setLanguage(String type){
    if("Java".equals(type))      return new Java();
    else if("Ruby".equals(type)) return new Ruby();
    else{
      System.out.println("type error");
      return null;
    } 
  }
}

public class Main{
  public static void main(String args[]){
    LanguageFactory languageFactory = new LanguageFactory();
    Language java = languageFactory.setLanguage("Java");
    
    java.sayHello();    //say hello by Java
  }
}
```

来看下上面的代码：

1. 创建一个Language接口，并定义了一个sayHello方法。
2. 创建Java类和Ruby类，分别实现Language接口的sayHello方法。
3. 创建一个LanguageFactory类并实现setLanguage方法，这个方法的返回值是一个Language接口。我们可以根据type参数进行逻辑判断，返回一个实现了Language接口的类。
4. 调用LanguageFactory的setLanguage得到需要的对象。

##### 单例模式

对于需要频繁实例化的类，每次调用都实例化一个对象会对内存造成很大的开销。考虑到内存的开销，我们是否可以在实例化这个类的时候判断下是否已经存在该类的对象，如果存在则直接返回这个对象而不是实例化一个新的对象。单例模式就是这样的作用，它可以避免多次实例化一个类的对象从而节省内存开销。另外在交易系统中如果多次创建控制类对象会导致系统流程控制混乱，这样类似的场景下也是离不开单例模式的。

因为单例模式经常与工厂模式配合（通常工厂类只需要实例化一次就可以），所以我们将上面的工厂类改为单例类。

```Java
//Java 单例模式

class LanguageFactory{

  private static LanguageFactory instance = null;
  private LanguageFactory(){}

  public static LanguageFactory getInstance(){
    if(instance == null)  instance = new LanguageFactory();
    return instance;
  }

  public void demoMethod(){
    System.out.println("Singleton pattern");
  }
}

public class Main{
  public static void main(String args[]){
    LanguageFactory.getInstance().demoMethod();    
    //Singleton pattern
  }
}
```

单例模式用到了访问权限的特性，来看下上面的代码：

1. 保证单例类不能被实例化的方式就是让其构造函数变为私有属性，这样就不能通过构造函数构造当前类的对象了。
2. 因为不能在外面实例化对象，所以我们需要单例类本身提供一个自己的实例（对象），并且这个属性是类级别的（否则会变成一个死循环，因为要使用成员属性就必须有对象，而对象本身又是通过这个属性暴露出来的）。
3. 最后还需要暴露一个类方法来返回这个属性。
4. 这样就可以直接通过LanguageFactory的getInstance拿到这个类的唯一实例了。

##### 适配器模式

适配器的作用主要用于将一个接口转换为一个期望的接口，从而消除两个接口间不兼容的问题。其本质上也是一种封装、多态的设计。适配器可以将代码细节更好的隐藏，实现上和工厂模式类似，适配器要根据输入参数选择合适的处理类及方法。

来看下面这个播放器的例子：

```java
//Java 适配器模式

interface MediaPlayer{
  public void play(String audioType);
}

interface AdvancedMediaPlayer{
  public void playFlv();
  public void playMp4();
}

class FlvPlayer implements AdvancedMediaPlayer{
  public void playFlv(){
    System.out.println("play flv");
  }
  public void playMp4(){
    //do nothing
  }
}
class Mp4Player implements AdvancedMediaPlayer{
  public void playFlv(){
    //do nothing
  }
  public void playMp4(){
    System.out.println("play mp4");
  }
}

class AdapterPlayer implements MediaPlayer{
  AdvancedMediaPlayer advancedMediaPlayer;

  public AdapterPlayer(String type){
    if("flv".equals(type))  advancedMediaPlayer = new FlvPlayer();
    else if("mp4".equals(type))  advancedMediaPlayer = new Mp4Player();
    else System.out.println("type error");
  }
  public void play(String audioType){
    if("flv".equals(audioType))  advancedMediaPlayer.playFlv();
    else if("mp4".equals(audioType)) advancedMediaPlayer.playMp4();
    else  System.out.println("audio type error");
  }
}
class AudioPlayer implements MediaPlayer{
  AdapterPlayer adapterPlayer;
  public void play(String audioType){
    adapterPlayer = new AdapterPlayer(audioType);
    adapterPlayer.play(audioType);
  }
}
public class Main{
  public static void main(String args[]){
    AudioPlayer audioPlayer = new AudioPlayer();
    audioPlayer.play("mp4");    //play mp4
    audioPlayer.play("flv");    //play flv
  }
}
```

简单分析下上述代码：

1. 我们有一个要适配的接口AdvancedMediaPlayer和用来包装的接口MediaPlayer，MediaPlayer中定义了一个play方法，而AdvancedMediaPlayer对play进行细化，定义了playFlv和playMp4。
2. 创建FlvPlayer和Mp4Player类，一个实现了AdvancedMediaPlayer接口的playFlv方法，另一个实现了AdvancedMediaPlayer接口的playMp4方法。
3. 定义了适配器AdapterPlayer，适配器实现了包装接口MediaPlayer的play方法，同时适配器还会根据输入参数不同返回对应的advancedMediaPlayer对象。
4. 创建AudioPlayer类，该类通过适配器AdapterPlayer实现了包装接口MediaPlayer的play方法，并根据play方法输入的参数实例化AdapterPlayer对象，然后通过adapterPlayer调用play方法执行播放任务。这样AudioPlayer就通过适配器隐藏了play的实现细节。

##### 装饰模式

装饰模式可以在不修改现有结构的情况下，对当前对象追加新特性，装饰模式会创建一个装饰类来包装这个对象，然后通过装饰类来拓展新特性。当我们想对一个类进行拓展时，可以通过组合把它包装到装饰类里，然后通过在装饰类中定义属性和方法来拓展新特性。

来看下面这段代码：

```java
//Java 装饰模式

interface Animal{
  public void say();
}
class Dog implements Animal{
  public void say(){
    System.out.println("bark");
  }
}
class Cat implements Animal{
  public void say(){
    System.out.println("mew");
  }
}
abstract class AnimalDecorator implements Animal{
  protected Animal animal;
  public AnimalDecorator(Animal animal){
    this.animal = animal;
  }
  public void say(){
    animal.say();
  }
}
class LovelyAnimalDecorator extends AnimalDecorator{
  public LovelyAnimalDecorator(Animal animal){
    super(animal);
  }
  public void say(){
    setCharacter();
    animal.say();
  }
  public void setCharacter(){
    System.out.print("lovely ");
  }
} 
public class Main{
  public static void main(String args[]){
    Animal cat = new Cat();
    Animal kitten = new LovelyAnimalDecorator(cat);
    kitten.say();    //lovely mew
  }
}
```

上面代码如下：

1. 首先创建一个Animal接口并定义了say方法，然后定义Cat类和Dog类分别实现Animal接口的say方法。
2. 然后定义一个装饰类的抽象类，抽象类中定义了一个Animal类型的属性，方便子类包装使用。
3. 接下来定义了一个装饰类LovelyAnimalDecorator，继承了抽象类AnimalDecorator，并添加了新特性，重写了say方法。
4. 最后实例化一个原始类对象cat，把cat作为参数传递给包装类LovelyAnimalDecorator，调用该包装类的say方法。包装类的say方法会得到包装类中定义的新特性，同时还会执行原始类的say方法。

##### 代理模式

代理模式是指当需要执行一个类的某个操作时，不是直接调用这个类的方法，而是通过一个代理类进行包装，调用这个代理类的方法，由代理类来执行真正的需要调用的操作。这个模式有点像中介服务，比如你可以直接从卖家手里购买商品，也可以选择通过电商平台来与卖家交易。在卖家看来与他交易的是电商平台，而实质上购物的真实操作是你完成的，电商平台只是把你的钱给了卖家，那么电商平台就是你的代理类，替你来买执行购物的操作。

来看下面这段代码：

```java
//Java 代理模式

interface Trade{
  public void buy(String good);
}

class Buyer implements Trade{
  public void buy(String good){
    System.out.print("buy "+ good +" ");
  }
}
class Taobao implements Trade{
  private Buyer buyer;
  public void buy(String good){
    if(buyer == null)   buyer = new Buyer();
    buyer.buy(good);
    System.out.println("by taobao");
  }
}
public class Main{
  public static void main(String agrs[]){
    Trade taobao = new Taobao();
    taobao.buy("cd game");    //buy cd game by taobao
  }
}
```

分析以上代码

1. 首先创建Trade接口并定义了购买方法buy。
2. 然后创建Buyer类并实现Trade接口。
3. 最后定义代理类Taobao，拥有一个Buyer属性，实现Trade接口，并在buy方法中调用buyer对象的buy方法。

##### 观察者模式

观察者模式通常在如下场景使用：当一个对象的属性发生改变时，所有依赖它的对象都要收到相应的通知。类似的使用场景比如消息订阅，当消息发生改变时，所有订阅这个消息的用户都应该收到通知。

我们以天气预报为例来说明：

```java
//Java 观察者模式

import java.util.List;
import java.util.ArrayList;

class WeatherForecast{
  private List<Observer> observers = new ArrayList<Observer>();
  private String weather;

  public String getWeather(){
    return this.weather;
  }
  public void setWeather(String weather){
    this.weather = weather;
    this.notifyObservers();
  }
  public void subscribe(Observer observer){
    observers.add(observer);
  }
  public void notifyObservers(){
    for (Observer observer : observers) {
      observer.notified();
    }
 }
}
abstract class Observer{
  protected WeatherForecast weatherForecast;
  public abstract void notified();
}
class You extends Observer{
  public You(WeatherForecast weatherForecast){
    this.weatherForecast = weatherForecast;
    this.weatherForecast.subscribe(this);
  }
  public void notified(){
    System.out.println("notified you, "+ weatherForecast.getWeather());
  }
}
class YourFriend extends Observer{
  public YourFriend(WeatherForecast weatherForecast){
    this.weatherForecast = weatherForecast;
    this.weatherForecast.subscribe(this);
  }
  public void notified(){
    System.out.println("notified your friend, "+ weatherForecast.getWeather());
  }
}
public class Main{
  public static void main(String args[]){
    WeatherForecast weatherForecast = new WeatherForecast();
    new You(weatherForecast);
    new YourFriend(weatherForecast);
    weatherForecast.setWeather("rain");    
    //notified you, rain
    //notified your friend, rain
  }
}

```

简单的说观察者模式就是两个类之间消息的传递，消息类状态发生改变时会通知观察者执行相应操作，而观察者在初始化时会通知消息类把自己加入到观察者注册列表中。具体如下：

1. 首先创建一个消息类，消息类中有一个消息状态及状态的访问器，这里定义了用来描述天气的属性weather及weather的属性访问器setWeather和getWeather。
2. 消息类还定义了一个观察者列表及相关的方法。观察者列表observers用来记录所有订阅了该消息的观察者，subscribe方法用来追加新的观察者对象到观察者列表，notifyObservers方法用来通知观察者消息改变了。
3. 其中notifyObservers方法的工作原理是遍历观察者列表中的每一个观察者，然后并调用它们的notified方法。
4. 接着定义观察者抽象类Observer，这个抽象类拥有消息类WeatherForecast及notified方法。
5. 然后定义了两个观察者You和YourFriend，这两个观察者都在构造函数中调用了消息类weatherForecast的注册方法subscribe，这样新的观察者就会追加到观察者对象，并在消息发生改变时通过notifed方法通知所有观察者消息改变了。
6. 最后让每个观察者类都实现notifed方法来处理接收到的消息。

##### 责任链模式

责任链是将一系列对象像链表一样链接起来，其中每个对象完成一个指定的任务，然后将任务传递给一下个对象。比如生产线中每一个车间完成一个加工任务，然后流水线到下一个车间。

我们来看如下代码：

```java
//Java 责任链模式

abstract class Task{
  protected Task next;

  public void nextTask(Task task){
    this.next = task;
  }
  protected abstract void work();
  public void start(){
    work();
    if(this.next != null)   this.next.start();
  }
}

class Write extends Task{
  public void work(){
    System.out.print("write ");
  }
}

class Compile extends Task{
  public void work(){
    System.out.print("compile ");
  }
}

class Link extends Task{
  public void work(){
    System.out.print("link ");
  }
}

class Run extends Task{
  public void work(){
    System.out.print("run");
  }
}

class TaskChain{
  public static Task getTaskChain(){
    Task write = new Write();
    Task compile = new Compile();
    Task link = new Link();
    Task run = new Run();
    write.nextTask(compile);
    compile.nextTask(link);
    link.nextTask(run);
    return write;
  }
}
public class Main{
  public static void main(String args[]){
    Task taskChain = TaskChain.getTaskChain();
    taskChain.start();    //write compile link run
  }
}
```

这里我们以Java语言从代码到运行这个过程为例进行说明。

>Java从代码到执行要经过编写、编译、链接、运行四个步骤

1. 首先我们创建一个Task抽象类，Task类定义了属性next来指明下一个任务节点，其中next本身也是一个Task类。
2. Task类中包含一个抽象方法work，交由子类实现。同时还定义了一个start方法，start方法调用当前任务的work方法并判断是否存在下一个任务节点，如果存在，则执行这个任务节点的start方法，不存在则结束。这样一来就构成了一个任务链。
3. 然后定义Write、Compile、Link、Run四个任务类，继承自Task类，并实现了work方法。
4. 接下来定义一个责任链类TaskChain，TaskChain中定义了一个静态方法getTaskChain用来获取任务链，getTaskChain依次创建了任务类对象write、compile、link和run，并通过对象的nextTask方法把四个任务对象串联成一个任务链，方法的最后返回任务链的第一个对象节点write。
5. 最后调用taskChain.start方法触发任务链的执行。


#### 设计模式和语言特性

设计模式本身是一种规范与技巧，其存在的目的是完善或者弥补面向对象语言在编程开发上的不足。一些语言无法更好解决的问题，开发中可以通过适当的使用设计模式来妥善处理，但是如果语言本身具备了所需要的特性，那么设计模式就可以抛到一边了。就像Java提供了C++中没有的垃圾回收机制，你就无需在Java中手动释放内存了，Scala提供了Java中没有的线程管理机制，你就无需在Scala中加锁一样。如果语言本身提供了更好的特性，你也就不必为了提高代码的复用性和维护性采用各种复杂设计模式了。我将通过下面两个例子，来对比说明设计模式是如何弥补语言设计上的缺陷的，以及通过部分语言自身的特性来隐藏设计模式的复杂逻辑。

##### Java8接口方法替代工厂模式

在讲匿名函数时提到过Java8新增了lambda特性，基于lambda的特性对应的也提出了接口方法，简单的说接口的函数体（方法实现）可以像变量一样被赋值，而在旧版本的Java中必须通过实现类来实现接口。因为这个特性的增加，我们可以跳出必须通过实现类实现接口方法的约束，直接在接口声明时指定接口方法的实现。

```java
//Java8 接口方法替代工厂模式

interface Language{
  public void sayHello();
}

class NewFeatureReplaceFactory{
  private Language language;
  private String type;

  public NewFeatureReplaceFactory(String type){
    this.type = type;
  }
  public void sayHello(){
    if("Java".equals(this.type))
      language = ()-> System.out.println("hello Java8");
    else if("Ruby".equals(this.type))
      language = ()-> System.out.println("hello Ruby");
    else
      language = ()-> System.out.println("type error");
    language.sayHello();
  }
}

public class Main{
  public static void main(String agrs[]){
    NewFeatureReplaceFactory java = new NewFeatureReplaceFactory("Java");
    java.sayHello();    //hello java8
  }
}
```

从上面代码可以看出，我们没有定义Language接口的实现类，而是在工厂类NewFeatureReplaceFactory的sayHello方法中根据type类型不同，对Language接口指定了不同的函数体实现（注意使用接口函数时，接口只能定义一个函数，解释器会自动把定义的函数体和函数定义绑定），这段逻辑其实和定义不同的类来实现Language的sayHello方法是等效的。

##### Io语言和单例模式

我们再来一个更加明显的例子，这里将引入一门基于原型的嵌入式语言Io，Io是基于原型的语言，支持面向对象编程范式，但是没有模板的概念，而是通过原型链来处理继承层次，原型的方法查找也像其他基于原型的语言一样在原型链中一层一层的向上查找，有就返回，没有就创建。

Io中通过clone方法来链接原型之间的层级关系，`B := A clone`表示原型链中A在B的上级，同时clone方法默认也会返回一个新的Object原型。假设一个原型重写了clone方法，并指定返回值就是当前原型本身，那会如何？没错，你没办法通过clone来创建一个新的Object原型，取代的是你始终会得到这个原型本身，这不正是单例模式要实现的功能么。

```Io
//Io 通过将原型链指向自身实现单例模式

Singleton clone := Singleton
isSingleton := Singleton clone
isSingleton = Singleton    
//true
```

##### 关于设计模式的争论

设计模式是面向对象发展过程中演化出来的编程技巧，适当的使用可以使代码更易于维护和拓展。关于设计模式有两种极端的人持有各自的想法。

* 通常根深蒂固于面向对象思维方式下的人会认为设计模式是一种信仰，设计模式是评断一个人技术水平的基本准则，不使用设计模式的程序一文不值。
* 一些函数式拥护者认为，语言本身存在重大的缺陷才需要设计模式来弥补，足够优秀的语言是不需要设计模式这种繁琐冗杂的技巧来打补丁的。

个人认为这两种想法都是不合理的，目前主流语言都纷纷加入函数式特性，部分模式是可以通过新特性化简的，设计模式确实可以体现出一个编程人员的水平，但没必要过分依赖和看重。另外函数式向工程化发展的过程中也渐渐意识到设计模式的重要性，渐渐的也提出了函数式下常用的设计模式，所以并非完备的语言是不需要模式的。这是语言本身特性和通过模式弥补之间的一个折中。

设计模式是一个复杂而高级的编程技巧，适当的使用可以使代码更健壮、更易维护，但是不要当做是一种信仰过分的依赖，比较所有技术手段和技巧其核心目的无非是提供更健壮、更易拓展的代码。如果你使用的语言本身有更好的特性，那么用语言的特性也是能够达到同样的效果的；同理再优秀的语言，也存在一定的弊端，设计模式是在经验积累下发展出来，弥补语言缺陷的重要手段之一。

和我之前在强弱类型，动静类型中所表述的一样，没必要在一个数轴上过分偏向哪一端，遵循语言的设计初衷一样，设计模式的使用也是如此，如果你对新特性够熟悉当然可以选择跳过设计模式的短板，同理如果语言需要设计模式的补充，也没必要否认。
