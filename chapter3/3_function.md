# 函数类型

###函数

函数是程序中的一段子程序，它用来执行一定的操作或运算，通常函数包括一个入口和一个出口。入口即参数输入（输入可以为空，即不输入参数），出口即函数返回值（输出可以为空，即不返还值），简单的说在你使用一个函数时，需要输入一些参数，函数会根据输入参数进行运算，然后会返回给你一个运算结果。

比如如下一个除运算函数，需要输入除数b与被除数a，函数进行除法运算后会返回一个商a/b。

```java
//Java 函数

public static double devide(double a, double b){
  return a/b;
}

public static void main(String args[]){
  double r = devide(3, 2);
  System.out.println(r);    //1.5
}
```

####分类

函数分为标准函数和自定义函数，所谓标准函数就是语言本身提供的函数，通常这是一些通用的函数，视具体语言不同标准函数库也稍有差别。

比如针对数组排序的函数sort，在java中该函数会破坏原数组：

```java
//Java sort方法会破坏原数组

public static void main(String args[]){
  int[] a = {4, 7, 5, 1, 2};
  int[] b = Arrays.sort(a);
  for(int i in a) {
    System.out.print(a[i] + " ");    
    //1 2 4 5 7
  }
} 
```
可以看出数组a的排序也发生了改变。对比Java，Ruby提供了两种排序方法sort和sort!，其中sort方法排序数组会保留原数组，返回一个新的排序后的数组，而sort!方法和上述中Java的Arrays.sort()方法类似，会破坏原数组。值得一提的是Ruby对大多数会产生破坏性的方法都提供了一组这样的函数，其中`!`结尾的方法表示会破坏原结构，之所以用`!`大概是想提示你这个函数要慎重使用。

```ruby
#Ruby sort和sort!方法对比

a = [4, 7, 5, 1, 2]
a.sort
puts a    #[4, 7, 5, 1, 2]

a = [4, 7, 5, 1, 2]
a.sort!
puts a    #[1, 2, 5, 5, 7]
```

对于大多数语言而言最基本的函数都是有提供的，比如字符串处理函数：
substring：字符串截取、concat：字符串合并、replace：字符串替换、toUpperCase/toLowerCase：大/小写转换、charAt：截取字符、startsWith/endsWith：开始/结束字符等；以及数组处理函数：join：数组转字符串、reverse：数组翻转、sort：数组排序、concat：数组合并、slice：数组截取、splice：插入删除等。

而自定义函数就是用户自己定义的函数。

抛开这种分类方式，我准备按照两种通用的编程范式来区分介绍函数，即面向对象下的函数和函数式下的函数。面向对象的核心就是以对象的方式处理问题，所以这部分函数通常都和类/对象相关，而函数式则以函数的方式处理问题，所以这部分的函数通常是一些函数的组合。

### 面向对象下函数

####函数重载

函数重载是针对于多个函数而言的，对于像C++、Java、C#这些静态类型语言而言，函数参数是需要声明具体类型的，相同名称不同参数类型的函数可以看做是不同的函数。重载就是针对这一特点而言的，函数重载允许创建输入参数或输出参数不同，但函数名相同的函数。

来看下Java的函数重载：

```java
//Java 函数重载

public static double add(double a, double b){
  return a+b;
}

public static int add(int a, int b){
  return a+b;
}
```
上面两个函数都叫做add，但是输入参数类型和返回结果类型不同，这就是函数重载。因为语言特性不同，对于动态语言而言，函数重载是没有意义的，因为动态语言本身是支持类型推导，所以没必要重复声明同一个函数应付不同的参数类型。

```ruby
#Ruby 动态类型语言不必重复接受不同参数的同一函数

def add(a, b)
  a+b
end

puts add(1 ,2)        #3
puts add(1.3, 1.2)    #2.5
```
Ruby会根据输入参数进行类型推导，并根据实际传入类型进行运算，输出对应类型的返回值。

####运算符重载

如果函数重载是对一个函数重新定义，那么运算符重载就是对运算符对应的运算规则重新定义。出于语言设计者的考虑，运算符重载也不是所有语言都支持的。比如Java语言就不提供运算符重载，而C++和Ruby则提供了运算符重载。

因为Ruby的运算符本身就是一个函数，只是函数名称是运算符号而已，所以重新定义对应符号名的函数就相当于运算符重载了。

比如这个例子：假设我们有一个班级类Class，Class下有两个属性：boy\_numbers和girl\_numbers,分别记录了班级里男生人数和女生人数，现在我们想不区分性别，通过一个简单的加法运算来求出两个班级的总人数。这里我们用Ruby来重写`+`运算。

```ruby
#Ruby (+)运算符重载

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

puts Class.new(27, 24) + Class.new(26, 23)    #100
```
可以看出我们重写了+运算符，使他对传入的对象c取得boy\_numbers和girl\_numbers的值并与自身的boy\_numbers和girl\_numbers值求和，最后返回总人数。

####构造函数

构造函数是类在实例化一个对象时执行的函数，一般用于初始化对象的属性。

**Java构造函数**

在Java中构造函数名与类名一致，你可以显示的定义构造函数，并执行一些赋值操作。
```java
//Java 显式定义构造函数并执行赋值操作

class Demo{
  public int a, b;
  public Demo(int a, int b){    
    //构造函数
    this.a = a;
    this.b = b;
  }
  public static void main(String args[]){
    Demo d = new Demo(1, 2);
    
    System.out.println(d.a +","+ d.b);    //1,2
}
```

**Ruby构造函数**

在Ruby中类的构造函数都用initialize命名，同样你也可以显示的定义，并进行赋值操作。

```ruby
#Ruby 构造函数

class Demo
  attr_accessor :a, :b    #配置属性可读可写
  def initialize(a, b)
    #构造函数	
    @a = a
    @b = b
  end
end
d = Demo.new(1, 2)

puts d.a, d.b    #1 2
```

####析构函数(destructor)

与构造函数相反，析构函数在对象结束调用后执行，用来释放对象所占用的内存空间，回收资源。现在的高级语言都加入了垃圾回收机制帮我们处理废弃的对象并回收内存空间。所以通常你应该也不会去定义析构函数了。

在C++中析构函数采用在构造函数名前加`~`的方式来命名。

```c-plus
//C++ 构造函数和析构函数

class Demo{
  public:
    Demo(){};     //构造函数
    ~Demo(){};    //析构函数
}
```

####实例方法和类方法

对于面向对象语言而言，实例方法（也可以叫作成员方法）和类方法是不得不说的一个概念。

简单的说实例方法是属于对象的函数，而类方法是属于类的函数，也就是说前者通过对象调用，而后者则通过类调用。


**Java中的实例方法和类方法**

在Java中类级别的方法或者属性通过static关键字修饰。

```java
//Java 类方法和实例方法

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

    System.out.println(Animal.getNumbers());    //2
    System.out.println(dog.getIndex());         //1
  }
}
```
上述例子中声明了类属性number以及类方法getNumber，实例属性（也有叫成员变量的）index和实例方法getIndex。输出结果中可以看出，每次实例对象都会有一个新的index，但是number却是共享的，因为它是类级别的。

**Ruby中的实例方法和类方法**

Ruby则通过`@@`来定义类属性，通过self关键字来定义类方法，self代表当前运行时的上下文，这里所指上下文就是类本身。

```ruby
#Ruby 类方法和实例方法

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

puts Animal.get_numbers     #2
puts dog.get_index          #1
```
上面的例子和Java一样，只是语法上稍有不同，Ruby通过`@`定义成员属性,通过`@@`定义类属性。

####私有方法

私有方法可以看做是一种特殊的实例方法和类方法，私有方法是只有在当前类（上下文）下才能调用的方法，通常用private关键字修饰，如果这个类存在子类，那么子类是无法访问该方法的（注意子类会继承该方法，只是无法访问）。

```java
//Java 类的私有方法

class A{
  private void privateMethod(){
    System.out.println("This is a private method of class A");
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

    a.privateMethod();        
    //error: privateMethod() has private access in A       

    a.callPrivateMethod();    
    //This is a private method of class A

    b.privateMethod();        
    //error: cannot find symbol b.privateMethod();

    b.callPrivateMethod();    
    //This is a private method of class A
  }
}
```
可以看出privateMethod只有在类A的上下文下才可以访问，在外部通过a对象调用会报权限错误，而子类调用则找不到该方法。

```ruby
#Ruby 类的私有方法

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

a.privateMethod        
#private method `privateMethod' called for #<A:0x007fde9309e038> (NoMethodError) 

a.callPrivateMethod    
#this is private method of class A

b.privateMethod        
#private method `privateMethod' called for #<A:0x007fde9309e038> (NoMethodError) 

b.callPrivateMethod    
#this is private method of class A 
```

可以看出Ruby中的私有方法调用结果和Java一样，对于类A所在的上下文外是无法访问的。


#### 友元函数

友元函数是针对一个类而言的，当一个函数定义为一个类的友元函数，那么这个函数就可以无视访问权限关键字（private、protected）直接访问这个类的属性。因为友元函数的设计降低了封装性，和面向对象设计的根本原则有所违背，所以在后续的语言中不再提供这个概念，这里也不过多赘述了。


#### 属性访问器

同私有方法类似，通过private修饰的属性来是无法在外部直接访问的。因此需要提供一个属性读与写的入口，这就是属性访问器。通常属性访问器用set/get表示，set表示写，get表示读。

```java
//Java 属性构造器

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
    
    a.privateVariable = "private variable";    
    //error: privateVariable has private access in A

    System.out.println(a.privateVariable);     
    //error: privateVariable has private access in A

    a.setVariable("private variable");         
    System.out.println(a.getVariable());       
    //private variable

  }
}
```
可以看出无法在类外直接访问类的私有属性，需要通过set和get方法进行读写操作，这样做可以保证数据读写的安全。

接下来我们看看Ruby中是如何实现属性构造器的。

```ruby
#Ruby 属性构造器

class A
  attr_accessor :privateVariable
end

a = A.new
a.privateVariable = "private variable"

puts a.privateVariable    #private variable
```
在Ruby中，类的所有属性都是private的。Ruby提供了对属性读写的配置：attr\_reader、attr\_writer和attr\_accessor，其中attr\_reader表示属性可读，相当于Java的get方法，attr\_writer表示属性可写（配置）相当于set方法，最后这里用到的attr_accessor表示属性既可读又可配置，相当于同时定义了set方法和get方法。属性具备了读写特性后就可以通过`.`运算进行读写操作了。


####虚函数（virtual function）和函数重写（override）

虚函数是C++中提出的一个概念，当子类继承父类时，如果父类中某个函数定义为虚函数，那么子类可以重写这个函数的实现。虚函数需要用virtual关键字修饰。

```c-plus
//C++ 虚函数

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

虚函数是面向对象语言中实现多态的一种重要手段。在Java语言中所有函数默认都是虚函数，也就是子类可以重写所有父类的函数，因此Java中称作函数重写。

```java
//Java 函数重写

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

    a.overrideFunction();    //function of class A
    b.overrideFunction();    //function override by class B
    c.overrideFunction();    //function override by class C
  }
}
```

###函数式语言中的函数

####匿名函数

匿名函数是无需显示声明的函数，一般用于基本不会复用或使用次数有限的函数。匿名函数可以使代码简化，方便调用。

除此之外匿名函数也常用作高阶函数的参数或返回值，这一点会在介绍函数式的时候详细说明。


**JavaScript中匿名函数**

```javascript
//JavaScript 匿名自执行函数
console.log(function(){ return 2 }());    //1
```

我们在log函数里传入了一个匿名函数`function(){ return 2 }`，紧接着通过`()`运算符执行了这个匿名函数。

接下来再看下匿名函数作为返回值的例子：

```javascript
//JavaScript 匿名函数作为返回值

function add (a){
  return function(b, c){ a+b+c };
}

add(1)(2,3);    //6
```
上面的例子中add函数返回了一个匿名函数`function(b, c){ a+b+c }`，当调用add(1)时会返回一个函数`function(b, c){ 1+b+c }`，所以我们可以继续用`()`运算符传入参数b与c执行返回的匿名函数。

**Java匿名内部类和lambda表达式**

Java本身是不支持匿名函数的，但是可以通过匿名内部类实现匿名函数的功能。

```java
//Java 匿名内部类模拟匿名函数

class Demo{
  public static void main(String args[]){
    new Object(){
      void add(int a, int b){  System.out.println(a + b); }
    }.add(1, 2);
  }
}
```

上面我们在输出方法中实例化了一个匿名内部类，并在类中定义了add方法，紧接着用这个实例化的对象调用了add方法，早期版本的Java不支持匿名函数与其初衷的设计也是有关的。在后续版本Java8中引入了lambda，通过lambda表达式也可以避开丑陋的匿名内部类直接实现匿名函数。

 ```java
//Java 对比匿名内部类与lambda表达式

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

在这里还要强调一点，虽然Java8引入了lambda的特性，但其实并非是单纯的匿名函数，和内部类一样只是一种匿名函数的实现手段，lambda表达式和匿名函数最大的区别就在于lambda并不是一个可以通过`()`运算符直接调用的函数，它只是一段带有作用域的代码，其主要目是数据及代码的传递，如果从这方面看匿名函数的话，也可以理解为匿名函数是一种以函数的形式来传递数据和代码的手段。

**Ruby中的proc和lambda**

在Ruby中匿名函数也是通过lambda实现的，不同的是Ruby中的lambda是一个proc对象，Ruby用proc和block两个概念来包装代码段，区别在于proc是Proc类的对象，而block只是单纯的代码段，所以block只能作为代码段使用，而proc可以像变量一样传递。

虽然lambda是一个proc对象，但两者也存在区别：lambda会检查参数的合法性（比如实参个数与形参是否匹配），而proc则不会如此。

```ruby
#Ruby 匿名自执行函数

lambda{ |x| puts x + 1 }.call(2)        #3
Proc.new { |x| puts x + 1 }.call(2)     #3

```
上面代码定义了一个匿名的lambda，并且通过call（等同于上面JavaScript代码中的()）执行了这个lambda。第二个例子则创建了一个匿名的Proc对象并执行。

因为proc和lambda都是对象，所以是可以赋值给其他变量的。

```ruby
#Ruby lambda作为参数传递

a = lambda { |x| puts x*2 }
[1, 2, 3].each(&a)    #2 4 6
```

上面代码中`&`运算符是将proc对象转为block，相当于把代码段`puts x*2`带入到each中，当然这样的写法就不能算作纯粹的匿名函数了，因为我们给lambda指定了变量名a。

```ruby
#Ruby 匿名函数作为参数传递

[1, 2, 3].each(&lambda { |x| puts x*2 })    #2 4 6 
```

####纯函数

纯函数是指没有副作用的函数，所谓副作用是指会对外界变量、状态产生影响的语句，也就是说纯函数不会改变函数外变量的值，因此纯函数对相同的参数输入会返回相同的计算结果。

这种禁止修改外部状态的策略，也是函数式语言的一种策略，对于纯函数式语言，所有的函数必须都是纯函数（比如Haskell），正因为纯函数语言不存在可变状态，使其在并发编程下具备绝对的优势。

我们来通过Ruby的reverse和reverse!对比下纯函数和非纯函数的区别。

```ruby
#Ruby reverse函数

a = [1, 2, 3]

puts a.reverse    #[3,2,1]
puts a            #[1,2,3]
```
```ruby
#Ruby reverse!函数
a = [1, 2, 3]

puts a.reverse!    #[3,2,1]
puts a             #[3,2,1]
```

上面的例子中reverse就是一个纯函数，reverse翻转一个数组后不会改变原数组，而reverse!则相反，它会修改原数组，所以reverse!不是一个纯函数，包括上面函数分类用的sort和sort!也是如此的，Ruby本身提供的函数是纯函数，根据需求你也可以用相应的`!`函数改变源数据。对比Java（Java的reverse是破坏性的）和Haskell，Ruby提供了更多的可选条件。

####谓词函数

谓词函数是值返回值为布尔类型的函数，一般常在函数式语言中使用来配合构成高阶函数。

```javascript
//JavaScript 谓词函数

var isString = function(s){ return typeof s === "string" };

isString("it's a string");    //true
isString(1);                  //false
```
上述代码定义一个字符串判断函数，如果输入参数为字符串则返回true，否则返回false。

同样还是特别介绍下Ruby的谓词函数

```ruby
#Ruby 谓词函数

"it's a string".is_a? String    #true
1.is_a? String                  #false
```
函数is_a?用来判断参数类型是否匹配，匹配则返回true，否则返回false，注意观察会发现这个谓词函数后面跟着一个`?`，和破坏性函数类型，Ruby用`?`来特别标识一个函数为谓词函数。

因此如果我们用Ruby来定义一个谓词函数时按照规范应该在函数名后标识一个`?`。

```ruby
#Ruby 定义谓词函数

def is_string? s
  s.is_a? String
end

is_string? "it's a string"    #true
is_string? 1                  #false
```

####高阶函数

高阶函数是一种参数、返回值为函数的函数。高级函数作为函数式编程中最为基础和根本的内容，我会在后续章节中用一节的内容详细介绍，现在你只需要记得高阶函数可以把其他函数作为参数传入，也可以指定一个函数作为计算结果返回。


