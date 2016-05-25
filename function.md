# 函数类型

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

