---
title: Java常见关键字static、final、abstract、interface
date: 2021/5/5
tags: [Java关键字]
categories: Java
---

## Java 常见关键字 static、final、abstract、interface

### static 关键字

- 当编写一个类时，其实就是在描述其对象的属性和行为，而没有产生实质上的对象，只有通过 new 关键字才会产生的对象，这时系统才会分配内存空间给对象，器方法才可以供外部调用；有时我们希望无论是否产生了对象或无论产生了多少对象的情况下，<span style="color:red">某些特定的数据在内存空间里只有一份</span>
- 例如所有中国人都有（共享）中国国籍这个属性，即不需要实例每个中国人时都在内存空间单独分配一个变量表示国籍属性，避免浪费空间。

### static 使用范围

- static 可修饰：属性、方法、代码块、内部类
- 被修饰后的成员具有的特点：
  - 随着类的加载而加载
  - 优先于对象存在
  - 修饰的成员，被所有对象所共享
  - 访问权限允许时，可不创建对象，直接被类调用

### static 修饰属性

- 使用 static 关键字 修饰的属性称为静态变量
  - 属性分为静态属性和非静态属性（实例变量）
  - 实例变量：当创建了类的多个对象，每个对象都独立拥有一套类中的非静态属性
  - 静态变量：当创建了类的多个对象，多个对象都共享同一个静态变量
  - 静态变量随着类加载而加载，早于对象的创建
    - 所以可以通过 `类.静态变量` 的方式来调用
    - 类只加载一次，则静态变量在内存中也只会存在一份，存在方法区中的静态域中
- 静态属性举例：`System.out`、`Math.xxx`

### static 修饰方法

- 使用 static 关键字修饰的方法称为静态方法
  - 静态方法随着类加载而加载，故可以使用`类.静态方法`来调用静态方法
  - 静态方法只能调用静态方法和静态属性，非静态方法中，既可以调用静态方法和属性，也能调用非静态方法和属性
  - 在静态方法中，由于生命周期的不同，不能使用 this、super 关键字

### static 修饰属性、方法的使用思路

- 类属性作为该类各个对象之间共享的变量。在设计类时，分析哪些<span style="color:red">属性不因对象的不同而改变</span>，将这些属性设置为类属性，相应的方法设置为类方法
- 如果方法与调用者无关，则这样的方法通常被声明为类方法，由于<span style="color:red">不需要创建对象就可以调用类方法</span>，从而简化了方法的调用

### static 应用——单例设计模式

- 某个类<span style="color:red">只能存在一个对象实例</span>，并且该类只提供一个取得其对象实例的方法。
  - 首先该类的构造器的访问权限设置为 private，这样就不能用 new 操作符在类的外部产生类的对象了，但在类内部仍可以产生该类的对象
  - 由于静态方法只能调用静态属性，所以的用 static 来修饰变量
- 单例的实现 1（饿汉式）

```java
class Bank{
  //1.私有化类的构造器，避免在Bank类外部new多个实例
  private Bank(){

  }
  //2.内部创建类的对象(静态)
  private static Bank instance = new Bank();
  //3.提供公共的静态方法，返回类的对象
  public static Bank getInstance(){
    return instance;
  }
}
```

- 单例的实现 2（懒汉式）

```java
class Order{
  //1.私有化类的构造器
  private Order(){

  }
  //2.声明当前类的对象(静态)，未初始化
  private static Order instance = null;
  //3.声明public、静态的返回当前类对象的方法
  public static Order getInstance(){
    if(instance == null){
      instance = new Order();
    }
    return instance;
  }
}
```

- 区分前面两者的实现方式

  - `饿汉式`：在一开始就把对象造好了，对象加载时间更长；线程安全
  - `懒汉式`：延迟对象的创建；多线程时需要修改

- 单例模式的作用：单例模式只生成一个实例，减少了系统的性能开销

- 单例模式应用场景：网站的计数器、日志应用、数据库连接池

### 代码块

- 作用：初始化类、对象（初始化块），内部可以有输出语句
- 执行顺序：按照声明的先后顺序执行，静态先于非静态
- 静态代码块：随着类的加载而执行，区分于静态方法

```java
static{
  ...
}
```

- 只执行一次，初始化当前类的静态属性
- 非静态代码块：随着对象实例化而执行，每创建一个对象就会执行一次

```java
{
  ...
}
```

- 可在创建对象时，对对象的属性进行初始化

### 对属性赋值先后顺序

- 对属性赋值一共有默认初始化（1）、显式初始化（2）、构造器初始化（3）、对象赋值（4）、代码块初始化（5）
- 顺序：1 -> 2/5 -> 3 -> 4
  - 其中 2/5 按照代码先后顺序执行

### final 关键字

- 可以用来修饰类、方法、变量（表最终的）

1. 使用 final 修饰的类不能被继承了

- 例如 String 类、System 类

2. 使用 final 修饰方法：方法不可被子类重写了，不需要再扩充功能了

- 例如 Object 类中的 getClass 方法

3. final 修饰的变量不可被修改了（常量）

- final 修饰属性：不能再默认赋值、不能通过对象来赋值
- final 修饰局部变量：方法体里的局部变量，形参局部变量（表名此形参为常量，当调用此方法时，给常量形参赋一个实参，一旦赋值，不能重新赋值）

- `static final`用来修饰属性（全局常量）和方法

### 抽象类和抽象方法（abstract 关键字）

- 有时将一个父类设计得非常抽象，他的子类种类非常丰富，以至于它没有具体的实例（不需要 new 对象），这样的类叫做抽象类
- 抽象类中仍然提供构造器，可提供给子类实例化时调用
- <span style="color:red">abstract 关键字</span>
  - abstract 关键字修饰一个类：该类叫做修饰类
  - abstract 关键字修饰一个方法：该方法叫做抽象方法
    - 抽象方法只有方法的声明，没有方法的实现（方法体），以分号结束
    - `public abstract void talk();`
  - 含有抽象方法的类必须被声明为抽象类
  - 抽象类不能被实例化，抽象类是用来继承的，都会提供抽象类的子类必须重写父类的方法，并提供方法体，若子类没有重写父类的全部的抽象方法，则该子类仍为抽象类
- abstract 使用的注意点：
  - 不能用 abstract 修饰变量、代码块、构造器
  - 不能用 abstract 修饰私有方法、静态方法、final 的方法、final 的类
- 抽象类的应用：抽象类是用来模拟化那些无法确定自身功能具体实现方法的父类，需要由子类来提供具体功能实现的方法
- 创建抽象类的匿名子类对象
  - 子类名匿名，结合多态

```java
//创建抽象类的匿名子类对象
Employee employee = new Employee() {
  @Override
  public void work() {
    System.out.println("匿名子类的对象");
  }
};
//抽象类
public abstract class Employee{
   public abstract void work();
}
```

### 接口 interface

- 一方面：Java 不支持多继承，但是通过接口可以解决类的单继承的局限性
- 另一方面：从几个类中抽取了一些共同的行为特征，且他们之间没有 is-a 的关系，仅仅是具有相同的行为特征而已；此时即可抽取这些特性形成接口

- 接口的使用
  - 接口（能不能）和类（是不是）是并列的两个结构
- 接口：`javainterface 接口{ ...}`
- 定义接口中的成员
  - JDK7 及以前：只能定义全局常量和抽象方法
  - 全局常量：`public static final XXX` 书写时可省略前三个修饰符
  - 抽象方法：`public abstract xxx();` 书写时可省略前两个修饰符
  - JDK8 后：还可以定义静态方法、默认方法
  - 静态方法：`public static xxx(){...};`，只能通过接口来调用
  - 默认方法：`default xxx(){...};`，可通过实现类来操作
- 接口中不能定义构造器，接口不可以实例化
- Java 开发中，让类实现（implements）接口的方式来使用，如果实现类覆盖（实现）了接口的所有抽象方法，此实现类才可以进行实例化，否则为抽象类，得加 abstract 修饰符
- 接口得多实现与继承性：Java 类可实现多个接口 ---> 弥补了 Java 得单继承得局限性
  - 格式：`class A extends B implements C,D{...}`
  - 类 A 继承类 B，实现 C，D 接口功能
- 接口与接口之间可以继承，而且可以多继承
  - `interface C extends A,B{...}`
- 接口的使用

```java
Computer com = new Computer();
  //1.创建了接口的非匿名实现类的非匿名对象
  Flash flash = new Flash();
  com.transferDate(flash);
  //2.创建了接口的非匿名实现类的匿名对象
  com.transferDate(new Printer());
  //3.创建了接口的匿名实现类的非匿名对象
  USB phone = new USB() {
    @Override
    public void start() {
      System.out.println("手机开始工作");
    }
    @Override
    public void stop() {
      System.out.println("手机结束工作");
    }
  };
  com.transferDate(phone);
  //4.创建了接口的匿名实现类的匿名对象
  com.transferDate(new USB() {
    @Override
    public void start() {
      System.out.println("mp3开始工作");
    }
    @Override
    public void stop() {
      System.out.println("mp3结束工作");
    }
  });
```
