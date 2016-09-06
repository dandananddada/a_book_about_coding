# 函数类型

####函数

函数是程序中的一段子程序，它可以执行一定的操作或运算，通常函数包括一个入口和一个出口。入口即参数输入（输入可以为空，即不输入参数），出口即函数返回值（输出可以为空，即不返还值），简单的说在你使用一个函数时，需要输入一些参数，函数执行操作和运算后会返回一个值。

比如如下一个除运算函数，需要输入除数`b`与被除数`a`，函数进行除法运算后会返回一个商`a/b`。

```java
public static double devide(double a, double b){
  return a/b;
}
public static void main(String args[]){
  double r = devide(3, 2);
  System.out.println(r);      //>1.5
}
```

**分类**

函数分为标准函数和自定义函数，所谓标准函数就是语言本身提供的函数，通常这是一些通用的函数，视具体语言不同，标准函数库也稍有差别。

比如针对数组排序的函数`sort`，在java中该函数会破坏原数组：
```java
public static void main(String args[]){
  int[] a = {4, 7, 5, 1, 2};
  int[] b = Arrays.sort(a);
  for(int i in a) {
    System.out.print(a[i] + " ");    //>1 2 4 5 7
  }
} 
```
可以看出数组`a`的顺序发生了改变。对比Java，Ruby提供了两种排序方法`sort`和`sort!`，其中sort方法排序数组会保留原数组，返回一个新的排序后的数组，而`sort!`方法和上述中Java的`Arrays.sort()`类似，会破坏原数组。值得一提的是Ruby对大多数会产生破坏性的方法都提供了一组这样的函数，其中`!`结尾的表示会破坏原结构，之所以用`!`大概是想提示你这个函数要慎重使用。
```ruby
a = [4, 7, 5, 1, 2]
a.sort
puts a    #>[4, 7, 5, 1, 2]

a = [4, 7, 5, 1, 2]
a.sort!
puts a    #>[1, 2, 5, 5, 7]
```

但是对于大多数语言而言最基本的函数都是有提供的，比如字符串处理函数：
**substring：**字符串截取、**concat：**字符串合并、**replace：**字符串替换、**toUpperCase/toLowerCase：**大/小写转换、**charAt：**截取字符、**startsWith/endsWith：**开始/结束字符等；以及数组处理函数：**join：**数组转字符串、**reverse：**数组翻转、**sort：**数组排序、**concat：**数组合并、**slice：**数组截取、**splice：**插入删除等。

而自定义函数就是用户自己定义的函数。不管是标准函数还是自定义函数都是按照接下来我要说明的函数类型来定义的。

我想按照两种通用的编程范式来介绍函数，一种是面向对象，另一种是函数式。面向对象的核心就是以对象的方式处理问题，所以这部分函数通常都和类/对象相关。而函数式则以函数的方式处理问题，所以这部分的函数通常是一些函数的组合。

另外因为有些函数与编程范式关系不大，更多的是跟语言类型及语言的设计有关的，或者是所有范式下都会用到的，所以我想先介绍下这些函数，然后在按照面向对象相关的函数和函数式相关的函数来说明其他的函数。

####无关编程范式的函数

####函数重载

函数重载是针对于多个函数而言的，对于像`C++`、`Java`、`C#`这些静态类型语言而言，函数参数是需要声明具体类型的，相同名称不同参数类型的函数可以看做是不同的函数。重载就是针对这一的函数而言的，函数重载允许创建输入参数或输出参数不同，但函数名相同的函数。

来看下Java的函数重载：
```java
public static double add(double a, double b){
  return a+b;
}
public static int add(int a, int b){
  return a+b;
}
```
上面两个函数都叫做`add`，但是输入参数和输出结果不同，这就是函数重载。因为语言特性不同，对于动态语言而言，就不存在函数重载，因为动态语言本身是支持类型推导的，所以一个函数足以应对变化了。
```ruby
def add(a, b)
  a+b
end

puts add(1 ,2)        #>3
puts add(1.3, 1.2)    #>2.5
```
Ruby会根据输入参数进行类型推导，并根据实际传入类型进行运算，输出对应类型的返回值。像这种使用了

####运算符重载

如何函数重载是对一个已存在函数重新定义，那么运算符重载就是对运算符对应的运算规则重新定义。出于语言设计者的考虑，运算符重载也不是所有语言都支持的。比如Java语言就不提供运算符重载，而C++和Ruby则提供了运算符重载。

因为Ruby的运算符本身就是像函数一样定义的，只是函数名是符号而已，所以重新定义对应符号名的函数就相当于运算符重载了。

比如如下这个例子，假设我们有一个班级类`Class`，`Class`下有两个属性：`boy_numbers`和`girl_numbers`分别记录了班级里男生和女生的人数，现在我们想不区分性别，通过一个简单的加法运算来求出两个班级的总人数。这里我们用Ruby来重写`+`运算。

```ruby
class Class
  attr_accessor :boy_numbers, :girl_numbers
  def initialize(boys, girls)
    @boy_numbers = boys
    @girl_numbers = girls
  end
  def +(c)
    @boy_numbers + c.boy_numbers + @girl_numbers + c.girl_numbers
  end
end

puts Class.new(27, 24) + Class.new(26, 23)    //>100
```
可以看出我们重写了`+`运算符，使他对传入的对象`c`取得`boy_numbers`和`girl_numbers`的值与自身的`boy_numbers`和`girl_numbers`的值求和，返回总人数。


#### 面向对象下函数

面向对象和函数式下函数，分类说明。


####构造函数（）

构造函数是类在实例化（构造）一个对象时执行的函数。一般用于初始化一个对象的属性。

**Java构造函数**

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

**Ruby构造函数**

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

####析构函数(destructor)

与构造函数相反，析构函数在对象结束调用后执行，用来释放对象所占用的内存空间，回收资源。现在的高级语言都加入了垃圾回收机制帮我们处理无用对象，回收内存空间。所以通常也不会去定义析构函数了。

在C++中析构函数采用在构造函数前加`~`符号的方式来命名。

```c-plus
class Demo{
  public:
    Demo(){};     //构造函数
    ~Demo(){};    //析构函数
}
```

####实例方法和类方法

对于面向对象语言而谈，实例方法（Java中也有叫做成员方法的）和类方法是不得不说的一个概念。

简单的说实例方法是属于对象的，而类方法是属于类的，也就是说前者通过
对象调用，而后者可以通过类直接调用。

**Java实例方法和类方法**

在Java中类级别的方法或者属性通过`static`修饰符标明。通常类方法用于获取类属性。

```java
//java
/* 对比类方法和实例方法 */
class Animal{ 
  public static int numbers;
  public int index;

  public static int getNumbers(){
    return numbers;
  }
  public int getIndex(){
    return index;
  }
  public Animal(){
    numbers++;
    index++;
  }
  public static void main(String args[]){
    Animal cat = new Animal();
    Animal dog = new Animal();
    System.out.println(Animal.getNumbers());    //=>2
    System.out.println(dog.getIndex());        //=>1
  }
}
```
上述例子中声明了类属性number以及类方法getNumber，实例属性（也有叫成员变量的）index和实例方法getIndex。输出结果中可以看出，每次实例对象都会有一个新的index，但是number却是共享的，因为它是类级别的。

**Ruby实例方法和类方法**

Ruby则通过`@@`来定义类属性，通过`self`（指向当前运行时的上下文，在方法定义时self指向的其实就是类本身）来定义类方法。

```ruby
#ruby
class Animal
  @@numbers = 0    
  def initialize()
      @@numbers += 1
      @index = 1
  end
  def self.get_numbers
      @@numbers
  end
  def get_index
      @index
  end
end
cat = Animal.new
dog = Animal.new

puts Animal.get_numbers     #=>2
puts dog.get_index          #=>1
```
上面的例子和Java一样，只是语法上稍有不同，Ruby通过`@`定义成员属性,`@@`定义类属性。

####私有方法

私有方法可以看做是一种特殊的实例方法和类方法，私有方法是只有当前类才能调用的方法，通常用`private`关键词修饰，如果这个类存在子类，那么子类也是无法访问该方法的（注意子类会继承该方法，只是无法直接访问）。

```java
class A{
  private void privateMethod(){
    System.out.println("This is private method of class A");
  }
  public void callPrivateMethod(){
    this.privateMethod();
  }
}
class B extends A{}

public class Demo{

  public static void main(String args[]){
    A a = new A();
    B b = new B();
    a.privateMethod();        //=>error: privateMethod() has private access in A       
    a.callPrivateMethod();    //=>This is private method of class A
    b.privateMethod();        //=>error: cannot find symbol b.privateMethod();
    b.callPrivateMethod();    //=>This is private method of class A
  }
}
```
可以看出`privateMethod`只有在A类定义中可以访问，在外部通过a对象调用会报权限错误，而子类调用则找不到该方法。

```ruby
class A
  private
    def privateMethod
      puts "this is private method of class A"
    end
  public
    def callPrivateMethod
      privateMethod()
    end
end
class B < A
end

a = A.new
b = B.new

a.privateMethod        #>private method `privateMethod' called for #<A:0x007fde9309e038> (NoMethodError) 
a.callPrivateMethod    #>this is private method of class A
b.privateMethod        #>private method `privateMethod' called for #<A:0x007fde9309e038> (NoMethodError) 
b.callPrivateMethod    #>this is private method of class A 
```
可以看出Ruby中的私有方法调用结果和Java一样，对于类A外部调用及子类调用都会抛出方法不存在的错误。


#### 友元函数

友元函数是针对一个类而言的，当一个函数定义为一个类的友元函数，那么这个函数就可以无视访问权限关键词（private、protected）直接访问这个类的属性。因为友元函数的设计降低了封装性，和面向对象设计的根本原则有所违背，所以后续的语言中并没有继续支持这种概念。


#### 属性访问器

同私有方法类似，对于用`private`修饰的属性来说也是无法在外部直接访问的。因此需要对属性的读写提供一个入口，这就是属性访问器。通常属性访问器用set表示写，get表示读。

```java
class A{
  private String privateVariable;

  public void setVariable(String a){
    this.privateVariable = a;
  }
  public String getVariable(){
    return this.privateVariable;
  }
}
public class Demo{
  public static void main(String args[]){
    A a = new A();
    a.privateVariable = "private variable";    //>error: privateVariable has private access in A
    System.out.println(a.privateVariable);     //>error: privateVariable has private access in A
    a.setVariable("private variable");         
    System.out.println(a.getVariable());       //>private variable

  }
}
```
可以看出对于私有属性无法在类外部直接访问，需要通过`set`和`get`方法进行读写操作，这样做的目的是封装数据并保证其读写的安全。

接下来看看Ruby中是如何实现属性读写的配置的。

```ruby
class A
  attr_accessor :privateVariable
end

a = A.new
a.privateVariable = "private variable"
puts a.privateVariable                    #>private variable
```
在Ruby下，类中所有属性都是`private`的，Ruby提供了对属性读写的配置：`attr_reader`表示可读，相当于`get`，`attr_writer`表示可配置（写）相当于`set`，这里用到的`attr_accessor`表示可读可配置，相当于同时设置了`set`和`get`。对属性进行了相关配置后就可以通过`.`运算直接对属性进行读写操作了。


####虚函数（virtual function）/函数重写（override）

虚函数是C++中提出的一个概念，意在使子类继承父类时，如果父类中某个函数定义为虚函数，那么子类可以重写这个函数的实现。虚函数需要用`virtual`关键字修饰。

```c-plus
class A{
  public:
    virtual void VirtualFunction(){
      cout << "This is virtual function of class A";
    }
};
class B : public A{
  public:
    void VirtualFunction(){
     cout << "Rewrite virtual function in class B";
  }
};
```
虚函数是面向对象语言中实现多态的一种重要手段。在后续的面向对象语言中所有函数默认都是虚函数，其实这个概念在Java中被叫做函数重写，无论什么命名，本质上都是为了实现多态。

```java
class A{
  public void overrideFunction(){
    System.out.println("function of class A");
  }
}

class B extends A{
  public void overrideFunction(){
    System.out.println("function override by class B");
  }
}
class C extends A{
  public void overrideFunction(){
    System.out.println("function override by class C");
  }
}
public class Main{
  public static void main(String args[]){
    A a = new A();
    B b = new B();
    C c = new C();
    a.overrideFunction();    //>function of class A
    b.overrideFunction();    //>function override by class B
    c.overrideFunction();    //>function override by class C
  }
}
```

###函数式语言中的函数

####匿名函数
匿名函数是无需显示声明名称的函数，一般作为高阶函数（参数或返回值类型为函数的函数）的参数或者返回值使用。
一般对于基本不会复用或使用次数有限的函数可以用匿名函数。匿名函数可以简化代码调用起来更便捷。

综合如上两点说明，匿名函数一般就如做如下用途：
1. 作为参数传递。
2. 匿名自执行，简化代码。

**JavaScript中匿名函数**
```javascript
/*  JavaScript匿名自执行函数  */
console.log(function(){ return 2 }());    //=>1
```
我们在输出语句里传入了一个匿名函数的定义，紧接着用调用运算`()`符执行了这个匿名函数。

相比自执行外更加常用的场景就是作为参数进行传递，一般在高阶函数中会经常用到，高阶函数所涉及到的一些概念我会在后续的章节（函数式编程）中详细说明，这里只简单看一下作为参数使用时的匿名函数。
```javascript
/*  JavaScript匿名函数作为参数传递  */
function add (a){
  return function(b, c){ a+b+c };
}
add(1)(2,3);    //=>6
```
上面的例子把函数作为返回值进行使用，调用`add(1)`后返回一个函数，因此可以通过`()`运算符并传入参数来执行这个返回的函数。

**Java匿名内部类和lambda**

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
 /*  匿名内部类实现  */
new Thread(new Runnable() {
  @Override
  public void run() {
    System.out.println("anonymous inner class");
  }
}).start();

 /*  lambda表达式实现  */
new Thread(() -> System.out.println("lambda express")).start();
 ```
在这里还要强调一点，虽然Java8引入了lambda的特性，但这不只是单纯的匿名函数，和内部类一样只是一种匿名函数的实现手段，lambda表达式和匿名函数最大的区别就在于lambda并不是一个可以通过`()`直接调用的函数，它只是一段带有作用域的代码，其主要目的其实是用于数据的传递。

**Ruby中的proc和lambda**

在Ruby中匿名函数也是通过lambda实现的，而lambda实质上是一个proc对象，Ruby用proc和block两个概念来包装代码段，区别在于proc是Proc类的对象，而block只是单纯的代码段，所以block只能作为代码段使用，而proc可以像变量一样传递。

同时lambda和proc也存在区别，lambda会检查参数是否合法（比如实参个数是否与形参匹配），而proc则不会。

```ruby
#ruby
#匿名自执行函数
lambda{ |x| puts x + 1 }.call(2)        #=>3
Proc.new { |x| puts x + 1 }.call(2)     #=>3

```
上面代码定义了一个匿名的lambda，并且通过call（等同于上面JavaScript代码的()）执行这个lambda。第二个例子是创建了一个匿名的Proc对象并执行。

因为proc和lambda都是对象，所以是可以赋值给其他变量的。
```ruby
#ruby
#作为参数传递
a = lambda { |x| puts x*2 }
[1, 2, 3].each(&a)    #=>2 4 6
```
上面这段代码`&`运算符是将proc对象转为block，相当于把代码段带入到`each`中，当然这样的写法就不能算作纯粹的匿名函数了，因为我们给lambda了名称a。
```ruby
#ruby
#匿名函数作为参数传递
[1, 2, 3].each(&lambda { |x| puts x*2 })
#=>2 4 6 
```



####纯函数

纯函数是指没有副作用的函数，所谓副作用是指会对外界变量、状态产生影响的语句。简单的说这样的函数是不会修改函数外的变量的，因此纯函数输入相同的参数返回结果始终都会是一样的。纯函数的设计是为了保证状态不变，也是函数式语言的一种策略，纯函数式语言中所有的函数都是纯函数，比如Haskell。

####谓词函数

谓词函数是值返回值为布尔类型的函数，一般在函数式语言中用来配合构成高阶函数。

**ruby中用？结束**

####高阶函数

高阶函数是一种参数或返回值也是函数类型的函数。高级函数作为函数式编程中最为基础和根本的内容，我会在后续章节中用一节的内容详细介绍，现在你只需要记得这个函数和其他函数有些不同的是，它可以把其他函数作为参数传入，也可以返回一个函数作为调用的返回值。


