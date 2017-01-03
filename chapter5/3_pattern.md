# 设计模式

设计模式（design pattern）是指对软件设计中常见的各种问题，提出一套通用可行的解决方案，从而避免重复设计方案来处理同一类问题。

设计模式的提出可以提高代码的复用性、易读性、可靠性，使程序开发更加工程化。

#### 背景

1. 1987年Kent Beck和Ward Cunningham参照建筑设计领域的思想提出了设计模式并应用在Smalltalk的图形接口上。
2. 1989年Erich Gamma在他的博士论文里开始尝试把这种思想用于软件开发。
3. 1989年Erich Gamma拿到博士学位后在美国与Richard Helm, Ralph Johnson, John Vlissides 合作出版了《Design Patterns》一书，书中提到了23种设计模式。
4. 经过20多年的发展，软件开发中套用模式已经成为一种流行的设计方法。

由于《设计模式》这本书的作者有四人，所以在软件开发领域称他们为四人帮（Gang of Four），简写作GoF，因此GoF也就成了软件设计模式的代称。

#### 分类

设计模式总体来说分为三大类：

1. 创建型，共五种：工厂模式、抽象工厂模式、单例模式、建造者模式、原型模式。
2. 结构型，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。
3. 行为型，共十一种：策略模式、模板模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

#### 原则

1. 开闭原则（Open Close Principle）

  开闭原则是指对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，从而使程序易于维护和升级。一般通过使用接口和抽象类来达到这个目的。

2. 里氏代换原则（Liskov Substitution Principle）

  里氏代换原则是对“开-闭”原则的补充。实现“开-闭”原则的关键就是抽象化。而父类与子类的继承关系就是抽象化的一种具体实现。
  里氏代换原则下，任何父类都可以被它的子类替换，只有这样才能保证父类是可复用的。

3. 依赖倒置原则（Dependence Inversion Principle）

  依赖倒置强调针对接口编程，不是依赖与具体的实现类，而是依赖于这个实现类的抽象（接口或抽象父类）。

4. 接口隔离原则（Interface Segregation Principle）

  将可以拆分的接口拆分为多个，目的在与降低接口之间的耦合性，降低类与类，类与接口间的耦合性能使程序更易于维护。

5. 迪米特法则（Demeter Principle）

  最少知道原则同接口隔离原则类似，最少知道原则是针对实体而言，强调实体间应该尽量少的相互作用，从而保证功能模块的独立，本质上还是降低耦合性使程序易于维护。

6. 合成复用原则（Composite Reuse Principle）

  尽量使用组合的方式，而不是继承。


#### 几种设计模式

我不准备把23种设计模式一一列举，毕竟这不是一本讲设计模式的书，我只是想说明设计模式是面向对象编程下一个必不可少的要素。所以我将从中挑选出几个比较经典的设计模式进行介绍，当然介绍设计模式没有比Java语言更合适的了。

**工厂模式**

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
2. 分别创建Java类和Ruby类，实现Language接口的sayHello方法。
3. 创建一个LanguageFactory类并实现setLanguage方法，这个方法的返回值是一个Language接口。我们可以根据type参数进行逻辑判断，返回一个实现了Language接口的类。
4. 调用LanguageFactory的setLanguage得到需要的对象。

**单例模式**

对于需要频繁实例化的类，每次调用都实例化一个对象会对内存造成很大的开销。考虑到内存的开销，我们是否可以在实例化这个类的时候判断下是否已经存在该类的对象，如果存在则直接返回这个对象而不是实例化一个新的对象。

单例模式就是这样的作用，它可以避免多次实例化一个类的对象从而节省内存开销。另外在类似交易系统中，如果多次创建控制类对象会导致系统流程控制混乱，这样的场景下也是离不开单例模式的。

因为单例模式经常与工厂模式配合（通常工厂类只需要实例化一次就可以），所以我们上面的工厂类改为单例类。

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
2. 因为不能在外面实例化对象，所以我们需要单例类本身提供一个自己的实例（对象）作为属性，并且这个属性是类级别的（否则会变成一个死循环，如果是成员属性就必须有对象，而对象本身是通过这个属性暴漏出来的）。
3. 最后需要暴漏一个类方法来返回这个属性（当前类的一个实例）。
4. 这样就可以直接通过LanguageFactory的getInstance拿到这个类的唯一实例了。

**适配器模式**

适配器的作用主要用于将一个接口转换为一个期望的接口，从而消除两个接口间不兼容的问题。其本质上也是一种封装、多态的设计。

适配器可以将代码细节更好的隐藏，实现上和工厂模式类似，适配器要根据输入参数选择合适的处理类及方法（不同的类实现了不同的接口方法）。

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

1. 我们有一个要适配的接口AdvancedMediaPlayer和用来包装的接口MediaPlayer，MediaPlayer定义了一个play方法，而AdvancedMediaPlayer对play细化定义了playFlv和playMp4。
2. 创建FlvPlayer和Mp4Player类，一个实现了AdvancedMediaPlayer接口的playFlv方法，另一个实现了AdvancedMediaPlayer接口的playMp4方法。
3. 定义了适配器AdapterPlayer，适配器实现了包装接口MediaPlayer的play方法，同时适配器同工厂类似根据输入参数返回具体的advancedMediaPlayer对象。
4. 创建AudioPlayer类，该类通过适配器AdapterPlayer实现了包装接口MediaPlayer的play方法，并根据play方法输入的参数实例化AdapterPlayer对象，然后调用adapterPlayer的play方法。这样AudioPlayer就通过适配器就隐藏了play的细节。

**装饰模式**

装饰模式可以在不修改现有结构的情况下，对当前对象追加新特性，装饰模式会创建一个装饰类来包装这个对象，然后通过装饰类来拓展新特性。

相当于我们要对一个类进行拓展，可以通过组合的方式把它包装到装饰类里，通过对装饰类定义属性和方法来拓展新特性。

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

1. 首先创建了一个Animal接口并定义了say方法，然后定义了实现这个接口的两个类Cat和Dog。
2. 定义一个装饰类的抽象类，抽象类中定义了一个Animal类型的属性，方便子类包装使用。
3. 定义了一个装饰类LovelyAnimalDecorator，继承了抽象类AnimalDecorator，并添加了新特性，重写了say方法。
4. 实例化一个原始类对象cat，把cat作为参数传递给包装类LovelyAnimalDecorator，调用包装类的say方法。
5. 包装类的say方法执行包装类定义的新特性，并执行原始类的say方法。

**代理模式**

代理模式是指当需要执行一个类的某个操作时，不是直接调用这个类的方法，而是通过一个代理类进行包装，调用这个代理类的方法，由代理类来执行真正的需要调用的操作。

这个模式有点像中介服务，比如你可以直接从卖家手里购买商品，也可以选择通过电商平台来与卖家交易。在卖家看来与他交易的是电商平台，而实质上购物的真实操作是你完成的，电商平台只是把你的钱给了卖家，那么电商平台就是你的代理类，替你来买执行购物的操作。

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

1.创建了Trade接口并定义了购买方法。
2.创建Buyer类并实现Trade接口。
3.定义了代理类Taobao，拥有一个Buyer属性，实现Trade接口，并在buy方法中调用buyer对象的buy方法。

**观察者模式**

观察者模式通常在如下场景使用：当一个对象的属性发生改变时，所有依赖它的对象都要收到相应的通知。

类似的使用场景比如消息订阅，当消息发生改变时，所有订阅这个消息的用户都应该收到通知。

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
2. 消息类还定义了一个观察者列表及相关的方法。观察者列表observers用来存储所有订阅该消息的观察者，subscribe方法用来添加新的观察者对象到观察者列表，notifyObservers方法用来通知观察者消息改变了。
3. 其中notifyObservers方法遍历观察者列表中每一个观察者，并调用他们的notified方法。
4. 接着定义了观察者抽象类Observer，这个抽象类拥有消息类WeatherForecast及notified方法。
5. 然后定义了两个观察者You和YourFriend，分别在构造函数中调用了消息类weatherForecast的观察者注册方法subscribe，这样在消息发生改变时所有在观察者列表中的观察者都会收到相应的通知并调用notifed方法。
6. 最后让每个观察者类都实现notifed方法，当收到消息类发送的通知时分别执行各自的处理方法。

**责任链模式**

责任链是将一系列对象像链表一样链接起来，其中每个对象完成一个指定的任务，然后将任务传递给一下个对象。

比如生产线中每一个车间完成一个加工任务，然后流水线到下一个车间。

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

这里我们以Java语言从代码到运行这个过程为例进行说明（Java从代码到执行要经过编写、编译、链接、运行四个步骤）。

1. 首先我们创建一个Task抽象类，Task类定义了一个Task类型的属性next来指明下一个任务节点。
2. Task类中包含一个抽象方法work，交由子类实现。同时还定义了一个start方法，用来执行当前Task的work方法并判断是否存在下一个任务节点，如果存在，则执行这个任务节点的start方法。这样就可以构成一个任务链了。
3. 然后定义Write、Compile、Link、Run四个任务，都继承自Task类，并实现了work方法。
4. 定义一个责任链类TaskChain，类中定义了一个静态方法getTaskChain用来获取任务链，方法中创建了任务类对象，并通过nextTask方法组成一个任务类链表，然后将任务链的第一个任务返回。
5. 最后调用taskChain.start方法触发任务链的执行，start方法会执行当前任务的work方法并判断是否存在下一个任务，如果存在，则执行下一个任务的start方法，否则结束任务，以此类推。

#### 设计模式和语言特性

设计模式本身是一种规范与技巧，其存在的目的是完善或者弥补面向对象语言在编程开发上的不足。一些语言无法更好解决的问题，开发中可以通过适当的使用设计模式来妥善处理，但是如果语言本身具备了所需要的特性，那么设计模式就可以抛到一边了。就像Java提供了C++中没有的垃圾回收机制，你就无需在Java中手动释放内存了，Scala提供了Java中没有的线程管理机制，你就无需在Scala中加锁一样。如果语言本身提供了更好的特性，你也就不必为了提高代码的复用性和维护性采用各种复杂设计模式了。

我们可以通过以下两个例子来对比说明设计模式是如何弥补语言设计上的缺陷的，以及某些语言是如何隐藏掉设计模式的。

**Java8接口方法替代工厂模式**

在讲匿名函数时提到过Java8新增了lambda特性，基于lambda的特性对应的也提出了接口方法，简单的说接口的函数体（方法实现）可以像变量被赋值，而旧版本的Java必须通过实现类来实现接口。因为这个特性的增加，我们可以跳出必须通过实现类实现接口方法的约束，直接在接口声明时指定接口方法的实现。

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

**Io语言和单例模式**

我们再来一个更加明显的例子，这里将引入一门基于原型的嵌入式语言Io，Io是基于原型的语言，支持面向对象编程范式，但是没有模板的概念，而是通过原型链来处理继承层次，原型的方法查找也像其他基于原型的语言一样在原型链中一层一层的向上查找，有就返回，没有就创建。

Io中通过clone方法来链接原型之间的层级关系，`B := A clone`表示原型链中A在B的上级，同时clone方法默认也会返回一个新的Object原型。假设一个原型重写了clone方法，并指定返回值就是当前原型本身，那会如何？没错，你没办法通过clone来创建一个新的Object原型，取代的是你始终会得到这个原型本身，这不正是单例模式要实现的功能么。

```Io
Io 通过将原型链指向自身实现单例模式

Singleton clone := Singleton
isSingleton := Singleton clone
isSingleton = Singleton    
//true
```

**关于设计模式的争论**

设计模式是面向对象发展过程中演化出来的编程技巧，适当的使用可以使代码更易于维护和拓展。关于设计模式有两种极端的人持有各自的想法。

1. 通常根深蒂固于面向对象思维方式下的人会认为设计模式是一种信仰，设计模式是评断一个人技术水平的基本准则，不使用设计模式的程序一文不值。
2. 一些函数式拥护者认为，语言本身存在重大的缺陷才需要设计模式来弥补，足够优秀的语言是不需要设计模式这种繁琐冗杂的技巧来打补丁的。

个人认为这两种想法都是不合理的，目前主流语言都纷纷加入函数式特性，部分模式是可以通过新特性化简的，设计模式确实可以体现出一个编程人员的水平，但没必要过分依赖和看重。另外函数式向工程化发展的过程中也渐渐意识到设计模式的重要性，渐渐的也提出了函数式下常用的设计模式，所以并非完备的语言是不需要模式的。这是语言本身特性和通过模式弥补之间的一个折中。

设计模式是一个复杂而高级的编程技巧，适当的使用可以使代码更健壮、更易维护，但是不要当做是一种信仰过分的依赖，比较所有技术手段和技巧其核心目的无非是提供更健壮、更易拓展的代码。如果你使用的语言本身有更好的特性，那么用语言的特性也是能够达到同样的效果的；同理再优秀的语言，也存在一定的弊端，设计模式是在经验积累下发展出来，弥补语言缺陷的重要手段之一。

和我之前在强弱类型，动静类型中所表述的一样，没必要在一个数轴上过分偏向哪一端，遵循语言的设计初衷一样，设计模式的使用也是如此，如果你对新特性够熟悉当然可以选择跳过设计模式的短板，同理如果语言需要设计模式的补充，也没必要否认。

