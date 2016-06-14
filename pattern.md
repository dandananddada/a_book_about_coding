# 设计模式

设计模式（design pattern）是指对软件设计中常见的各种问题，提出一套通用可行的解决方案，从而避免重复设计方案处理同一类型问题。

设计模式的提出可以提高代码的复用性、易读性、可靠性，使程序开发更加工程化。

####背景

1. 1987年Kent Beck和Ward Cunningham参照建筑设计领域的思想开提出了设计模式并应用在Smalltalk的图形接口。
2. 1989年Erich Gamma在他的博士论文里开始尝试把这种思想用于软件开放。
3. 1989年Erich Gamma拿到博士学位后在美国与Richard Helm, Ralph Johnson ,John Vlissides 合作出版了《Design Patterns》一书，书中提到了23种设计模式。
4. 经过20多年的发展，软件开发中套用模式已经成为一种流行的设计方法。

由于《设计模式》这本书的作者有四个人，这四个人在软件开发领域有了一个称号：四人帮（Gang of Four），简写做GoF，因此GoF也就成了软件设计模式的代称。

####分类

设计模式总体来说分为三大类：

1. 创建型，共五种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。
2. 结构型，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。
3. 行为型，共十一种：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

####原则
1. 开闭原则（Open Close Principle）

  开闭原则就是说对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，从而使程序易于维护和升级。一般通过使用接口和抽象类来达到这个目的。
  
2. 里氏代换原则（Liskov Substitution Principle）
  
  里氏代换原则是对“开-闭”原则的补充。实现“开-闭”原则的关键就是抽象化。而父类与子类的继承关系就是抽象化的一种具体实现。
  里氏代换原则下，任何父类都可以被它的子类替换，只有这样才能保证父类是可复用的。
  
3. 依赖倒转原则（Dependence Inversion Principle）
 
  针对接口编程，不是依赖与具体的实现类，而是依赖于这个实现类的抽象（接口或抽象父类）。

4. 接口隔离原则（Interface Segregation Principle）
  
  将可以拆分的接口拆分为多个，目的在与降低接口之间的耦合性，从大型软件架构角度考虑，降低类与类，类与接口间的耦合性能使程序更易于维护。

5. 迪米特法则（最少知道原则）（Demeter Principle）
  
  同接口隔离类似，不过是针对实体而言，实体间应该尽量少的相互作用，从而保证功能模块的独立，本质还是降低耦合性使程序易于维护。

6. 合成复用原则（Composite Reuse Principle）
  
  尽量使用组合的方式，而不是继承。
  
####几种设计模式

我不准备把23种设计模式一一列举，毕竟这不是一本设计模式的书，我只是想说明面向对象编程下必不可少的一个要素：设计模式。而设计模式中有些是必须掌握的，我就从中列举几个简单说明下，当然设计模式的例子用Java语言最合适不过了。


**工厂模式**

在开发过程中经常会遇到根据不同的条件实例化不同的对象，每次都重复这个逻辑显然是不够理想的，所以我们可以把这样的逻辑封装到一个类中，这个类就是工厂类，通过类和条件参数类返回需要实例化的对象，这个模式就是工厂模式。
```java
interface Language{
  public void sayHello();
}

class Java implements Language{
  public void sayHello(){
    System.out.println("hello Java");
  }
}

class Ruby implements Language{
  public void sayHello(){
    System.out.println("hello Ruby");
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
    Java java = (Java)languageFactory.setLanguage("Java");
    java.sayHello();    //=>hello
  }
}
```
来看下上面的代码
1. 创建一个Language接口，并定义了一个sayHello方法。
2. 分别创建Java类和Ruby类，实现Language的sayHello方法。
3. 创建一个LanguageFactory类和setLanguage方法，这个方法的返回值是Language接口。我们可以根据type参数进行调节判断，返回一个实现了Language接口的类。
4. 调用LanguageFactory的setLanguage得到需要的对象（注意setLanguage的返回类型是Language，所以需要强制转换）。

**单例模式**

对于频繁需要实例化的类，每次调用都事先实例化一个对象会对内存造成很大的开销，因为我们是否可以在实例化这个类的时候判断下是否已经存在对象，如果存在则直接返回不再实例化新的对象。这样就能节省开销。

单例模式就是这样一个作用，它可以避免多次实例化一个类的对象，因此可以节省内存开销。

另外在类似交易系统中，如果多次创建控制类对象会导致系统流程控制混乱，这样的场景下也是离不开单例模式的。

因为单例模式经常与工厂模式配合（通常用来实例化不同对象的工厂只需要一个就足够了），所以我们就把一个工厂类改为单例类。

```java
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
    LanguageFactory.getInstance().demoMethod();    //=>Singleton pattern
  }
}
```

单例模式用到了访问权限的特性，来看下上面的代码：
1. 保证单例类不能被实例化的方式就是让其构造函数变为私有属性，这样就不能在外面调用构造函数实例化当前类了。
2. 因为不能在外面实例化对象，所以我们需要单例类本身提供一个自己的实例（对象）作为属性，并且这个属性是类级别的（否则会变成一个死循环，如果是成员属性就必须有对象，而对象本身是通过这个属性暴漏出来的）。
3. 最后需要暴漏一个类方法来返回这个属性（当前类的一个实例）。
4. 这样就可以直接通过LanguageFactory的getInstance拿到这个类的唯一实例了。

**适配器模式**

适配器的作用主要用于将一个接口适配为另一个期望的接口，从而消除两个接口间不兼容的问题。其实本质上也是一种封装、多态的设计。

适配器可以将代码细节更好的隐藏，在实现和工厂模式对返回对象进行逻辑选择一样，适配器要根据输入参数选择合适的处理类及方法（不同的类实现了不同的接口方法）。

来看下面这个播放器的例子：

```java
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
    audioPlayer.play("mp4");
    audioPlayer.play("flv");
  }
}
```
简单分析下上述代码：

1. 
