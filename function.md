# 函数类型

####构造函数

##加初始化顺序

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
上面的例子和Java一样，只是语法上稍有不同，Ruby通过`@`定义成员属性,`@@`定义类属性，不同的时Ruby在构造函数里对实例属性进行
####私有方法

public protected private



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