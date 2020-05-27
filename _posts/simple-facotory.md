---
layout: post
title: 设计模式读书笔记-简单工厂模式
description: "针对接口编程，可以隔离掉以后系统可能发生的一大堆改变。入股代码是针对接口而写，那么可以通过多态，它可以与任何新类实现该接口。但是，当代码使用一大堆的具体类时，等于是自找麻烦，因为一旦加入新的具体类，就必须要改变代码。在这里我们希望能够调用一个简单的方法，我传递一个参数过去，就可以返回给我一个相应的具体对象，这个时候我们就可以使用简单工厂模式。"
modified: 2020-05-27
tags: [设计模式]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
---

在设计原则中有这样一句话“我们应该针对接口编程，而不是正对实现编程”。但是我们还是在一直使用new关键字来创建一个对象，这不就是在针对实现编程么？

​针对接口编程，可以隔离掉以后系统可能发生的一大堆改变。入股代码是针对接口而写，那么可以通过多态，它可以与任何新类实现该接口。但是，当代码使用一大堆的具体类时，等于是自找麻烦，因为一旦加入新的具体类，就必须要改变代码。在这里我们希望能够调用一个简单的方法，我传递一个参数过去，就可以返回给我一个相应的具体对象，这个时候我们就可以使用简单工厂模式。
# 简单工厂模式
* 基本定义
  - 简单工厂模式又称之为静态工厂方法，属于创建型模式。在简单工厂模式中，可以根据传递的参数不同，返回不同类的实例。简单工厂模式定义了一个类，这个类专门用于创建其他类的实例，这些被创建的类都有一个共同的父类。
* 模式结构
  - 模式结构图如下：
  - ![simple factory image]({{ site.url }}/assets/img/简单工厂模式.jpg){: .image-right}
  - 模式分析:
    + Factory：工厂角色。专门用于创建实例类的工厂，提供一个方法，该方法根据传递的参数不同返回不同类的具体实例。
    + Product：抽象产品角色。为所有产品的父类。
    + ConcreteProduct：具体的产品角色。
  - 简单工厂模式将对象的创建和对象本身业务处理分离了，可以降低系统的耦合度，使得两者修改起来都相对容易些。当以后实现改变时，只需要修改工厂类即可。
* 简单工厂模式实现
  - 模式场景：在一个披萨店中，要根据不同客户的口味，生产不同的披萨，如素食披萨、希腊披萨等披萨。
  - 该例的UML结构图如下:
  - ![simple factory uml image]({{ site.url }}/assets/img/简单工厂模式UML.jpg){: .image-right}
  - 代码实现
  - Pizza制造工厂：SimplyPizzaFactory.java
```java
/**
 * 专门用于创建披萨的工厂类
 */
public class SimplePizzaFactory {
    public Pizza createPizza(String type) {
        Pizza pizza = null;

        if (type.equals("cheese")) {
            pizza = new CheesePizza();
        } else if (type.equals("clam")) {
            pizza = new ClamPizza();
        } else if (type.equals("pepperoni")) {
            pizza = new PepperoniPizza();
        } else if (type.equals("veggie")) {
            pizza = new VeggiePizze();
        }

        return pizza;
    }
}
```
    - 抽象披萨：Pizza.java
```java
/**
 * 抽象pizza类
 */
public abstract class Pizza {
    public abstract void prepare();

    public abstract void bake();

    public abstract void cut();

    public abstract void box();
}
```
    - 具体披萨：CheesePizza.java
```java
public class CheesePizza extends Pizza {

    @Override
    public void bake() {
        System.out.println("bake CheesePizza ...");
    }

    @Override
    public void box() {
        System.out.println("box CheesePizza ...");
    }

    @Override
    public void cut() {
        System.out.println("cut CheesePizza ...");
    }

    @Override
    public void prepare() {
        System.out.println("prepare CheesePizza ...");
    }

}
```
    - PizzaStore.java
```java
public class PizzaStore {
    SimplePizzaFactory factory;      //SimplePizzaFactory的引用

    public PizzaStore(SimplePizzaFactory factory) {
        this.factory = factory;
    }

    public Pizza orderPizza(String type) {
        Pizza pizza;
        pizza = factory.createPizza(type);       //使用工厂对象的创建方法，而不是直接new。这里不再使用具体实例化
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
}
```

* 简单工厂模式的优缺点
  - 优点
    + 1. 简单工厂模式实现了对责任的分割，提供了专门的工厂类用于创建对象。
    + 2. 客户端无须知道所创建的具体产品类的类名，只需要知道具体产品类所对应的参数即可，对于一些复杂的类名，通过简单工厂模式可以减少使用者的记忆量。
    + 3. 通过引入配置文件，可以在不修改任何客户端代码的情况下更换和增加新的具体产品类，在一定程度上提高了系统的灵活性。
  - 缺点
    + 1. 由于工厂类集中了所有产品创建逻辑，一旦不能正常工作，整个系统都要受到影响。
    + 2. 使用简单工厂模式将会增加系统中类的个数，在一定程序上增加了系统的复杂度和理解难度。
    + 3. 系统扩展困难，一旦添加新产品就不得不修改工厂逻辑，在产品类型较多时，有可能造成工厂逻辑过于复杂，不利于系统的扩展和维护。
    + 4. 简单工厂模式由于使用了静态工厂方法，造成工厂角色无法形成基于继承的等级结构。