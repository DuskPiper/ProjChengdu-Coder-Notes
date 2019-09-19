# 创建型设计模式

创建型模式(Creational Pattern)对类的实例化过程进行了抽象，能够将软件模块中对象的创建和对象的使用分离。为了使软件的结构更加清晰，外界对于这些对象只需要知道它们共同的接口，而不清楚其具体的实现细节，使整个系统的设计更加符合单一职责原则。

创建型设计模式具体包含以下模式，按重要程度排序：

1. Factory Method 工厂方法模式

2. Abstract Factory 抽象工厂模式

3. Simple Factory Pattern 简单工厂模式

4. Singleton 单例模式

5. Builder 建造者模式

   

## Simple Factory Pattern 简单工厂模式

### 简述

在简单工厂模式中，可以根据参数的不同返回不同类的实例。简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类。

简单工厂模式包含如下角色：

- Factory：工厂角色，负责实现创建所有实例的内部逻辑。
- Product：抽象产品角色，所创建的所有对象的父类，负责描述所有实例所共有的公共接口。
- ConcreteProduct：具体产品角色，创建目标，所有创建的对象都充当这个角色的某个具体类的实例。

简单工厂模式的要点在于：当你需要什么，只需要传入一个恰当的参数，就可以获取你所需要的对象，而无须重复地去思考其创建细节。

![simple factory demo](https://design-patterns.readthedocs.io/zh_CN/latest/_images/SimpleFactory.jpg)

### 实现

- 定义：

```java
public interface Product { // 定义抽象产品
    void use(); // 声明公共方法
}

public class ProductOne implements Product { // 实例化抽象产品，叫做One
    public ProductOne() {
        // 构造函数
    }
    
    @Override
    public void use() {
        // 实际调用use方法的逻辑
    }
}

public class ProductTwo implements Product { // 实例化抽象产品，叫做Two
    // 同上
    public ProductTwo(){/*do something*/}
    public void use() {/*do something*/}
}

public class ProductFactory {
    public static final String TAG = "Demo Product Factory";
    Product product = null;
    public static Product produce(String name) { // 生产方法，用户实际使用工厂会用的方法
        if (name.equalsIgnoreCase("One")) {
            product = new ProductOne();
        } else if (name.equalsIgnoreCase("Two")) {
            product = new ProductTwo();
        }
        return product;
    }
}
```

- 使用

```java
Product myProduct = ProductFactory.produce("One"); // 得到一个ProductOne的实例
myProduct.use();
```



## Factory Method Pattern 工厂方法模式

### 简述

又简称工厂模式。工厂父类负责定义创建产品对象的公共接口，而工厂子类则负责生成具体的产品对象，目的是将产品类的实例化操作延迟到工厂子类中完成，即通过工厂子类来确定究竟应该实例化哪一个具体产品类。因此不同于简单工厂模式，不再提供统一的工厂类来创建所有的对象，而是针对不同的对象提供不同的工厂。也就是说每个对象都有一个与之对应的工厂。

工厂方法模式包含如下角色：

- Product：抽象产品
- ConcreteProduct：具体产品
- Factory：抽象工厂
- ConcreteFactory：具体工厂

![工厂方法模式图解](https://design-patterns.readthedocs.io/zh_CN/latest/_images/FactoryMethod.jpg)

### 实现

- 定义

  ```java
  public interface Product { // 定义抽象产品
      void use(); // 声明公共方法
  }
  
  public class ProductOne implements Product { // 实例化抽象产品，叫做One
      public ProductOne() {
          // 构造函数
      }
      
      @Override
      public void use() {
          // 实际调用use方法的逻辑
      }
  }
  
  public class ProductTwo implements Product { // 实例化抽象产品，叫做Two
      // 同上
      public ProductTwo(){/*do something*/}
      public void use() {/*do something*/}
  }
  // 目前为止，尚且和Simple Factory相同
  public interface ProductFactory { // 定义抽象工厂
      Product produce(); // 声明公共方法：生产
  }
  
  public class ProductOneFactory implements ProductFactory { // 实例化生产一号产品的工厂
      @Override
      public Product produce() {
          return new ProductOne();
      }
  }
  
  public class ProductTwoFactory implements ProductFactory { // 实例化生产二号产品的工厂
      @Override
      public Product produce() {
          return new ProductTwo();
      }
  }
  ```

- 使用

  ```java
  ProductFactory factory = new ProductOneFactory();
  Product product = factory.produce();
  product.use();
  ```



## Abstract Factory 抽象工厂模式

### 简述

提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类。抽象工厂模式是所有形式的工厂模式中最为抽象和最具一般性的一种形态。抽象工厂模式与工厂方法模式最大的区别在于，工厂方法模式针对的是一个产品等级结构，而抽象工厂模式则需要面对多个产品等级结构，一个工厂等级结构可以负责多个不同产品等级结构中的产品对象的创建 。因此，当系统所提供的工厂所需生产的具体产品并不是一个简单的对象，而是多个位于不同产品等级结构中属于不同类型的具体产品时使用抽象工厂模式。

抽象工厂模式包含如下角色：

- AbstractFactory：抽象工厂
- ConcreteFactory：具体工厂
- AbstractProduct：抽象产品
- Product：具体产品

![抽象工厂图解](https://design-patterns.readthedocs.io/zh_CN/latest/_images/AbatractFactory.jpg)

### 实现

- 定义

  ```java
  public interface Computer { // 定义抽象产品
      void boot(); // 声明公共方法
  }
  
  public interface Phone { // 定义抽象产品
      void call(); // 声明公共方法
  }
  
  public class Macbook implements Computer { // 实例化抽象电脑
      @Override
      public void boot(){/*do something*/}
  }
  
  public class HuaweiLaptop implements Computer { // 实例化抽象电脑
      @Override
      public void boot(){/*do something*/}
  }
  
  public class IPhone implements Phone { // 实例化抽象手机
      @Override
      public void call(){/*do something*/}
  }
  
  public class HuaweiP40 implements Phone { // 实例化抽象手机
      @Override
      public void call(){/*do something*/}
  }
  
  public interface ElectronicalFactory { // 定义抽象工厂
      public Phone producePhone();
      public Computer produceComputer();
  }
  
  public class AppleFactory implements ElectronicalFactory {
      @Override
      public IPhone producePhone() {
          return new IPhone();
      }
      public Computer produceComputer() {
          return new Macbook();
      }
  }
  
  /*etc...*/
  ```

- 使用

  ```java
  ElectronicalFactory factory = new AppleFactory();
  Phone myIPhone = factory.producePhone();
  Computer myMacbook = factory.produceComputer();
  ```



## Builder Pattern 建造者模式/生成器模式

### 简述

将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

建造者模式是一步一步创建一个复杂的对象，它允许用户只通过指定复杂对象的类型和内容就可以构建它们，用户不需要知道内部的具体构建细节。建造者模式属于对象创建型模式。根据中文翻译的不同，建造者模式又可以称为生成器模式。

建造者模式包含如下角色：

- Builder：抽象建造者
- ConcreteBuilder：具体建造者
- Director：指挥者
- Product：产品角色

![builder demo](https://design-patterns.readthedocs.io/zh_CN/latest/_images/Builder.jpg)

### 实现

- 定义

  ```java
  public class Computer() {
      String CPU;
      String RAM;
      String SSD;
      public String getCPU() {return this.CPU;}
      public String getRAM() {return this.RAM;}
      public String getSSD() {return this.SSD;}
      public void setCPU(String CPU) {this.CPU = CPU;}
      public void setRAM(String RAM) {this.RAM = RAM;}
      public void setSSD(String SSD) {this.SSD = SSD;}
      public void info() {
          System.out.println(CPU + RAM + SSD);
      }
  }
  
  public interface ComputerBuilder { // 抽象的工人
      void buildCPU(); // 组装CPU的具体逻辑过程
      void buildRAM();
      void buildSSD();
      Computer getComputer();
  }
  
  public class ConcreteComputerBuilder implements ComputerBuilder { // 实际的工人
      Computer computer = new Computer(); 
      
      @Override
      public void buildCPU(String CPU) {computer.setCPU(CPU);}
      @Override
      public void buildRAM(String RAM) {computer.setRAM(RAM);}
      @Override
      public void buildSSD(String SSD) {computer.setSSD(SSD);}
      @Override
      public Computer getComputer() {return computer;}
  }
  
  public class Director { // 工头
      public void buildComputer(ComputerBuilder builder) {
          builder.buildCPU("i9-9980XE"); // director只在乎装电脑的调度，细节builder来完成
          builder.buildRAM("128GB"); // 所以逻辑上更像是工头，而builder是工人
          builder.buildSSD("4TB");
      }
  }
  ```

- 使用

  ```java
  Director director = new Director();
  Builder builder = new ConcreteComputerBuilder();
  
  director.buildComputer(builder);
  Computer myComputer = builder.getComputer();
  myComputer.info();
  ```



## Singleton 单例

### 简述

确保一个类只有一个实例，并提供一个全局访问点。

单例有多种不同难度的实现形式。单例模式的要点有三个：某个类只能有一个实例；二是它必须自行创建这个实例；三是它必须自行向整个系统提供这个实例。单例模式是一种对象创建型模式。单例模式又名单件模式或单态模式。

简单的单例不是线程安全的，需要利用线程锁来实现线程安全的单例。

### 实现

- 线程不安全，延迟实例化。好处是如果没用到就不会实例化，从而节约资源。

```java
// 线程不安全，延迟实例化
public class SingletonThreadUnsafe {
    
    private static SingletonThreadUnsafe instance; // 私有静态变量，记录本类唯一实例
    
    private SingletonThreadUnsafe() {
        // 初始化实例，是私有函数所以只有本类内才能调用构造
    }
    
    public static Singleton getInstance() { // 程序中任何地方都能调用此函数来获取唯一实例
        // 调用方法 SingletonThreadUnsafe.getInstance();
        if (instance == null) { // 延迟实例化，即在需要使用时才实例化
            instance = new SingletonThreadUnsafe();
        }
        return instance;
    }
}
```

- 线程安全，急切实例化。如想实现单例的类在创建和运行时的**负担并不繁重**可考虑。

```java
// 线程安全，急切实例化
public class SingletonThreadSafe {
    
    private static SingletonThreadSafe instance = new SingletonThreadSafe();
    // 在且仅在静态初始化中就创建单例，也就避免多线程的问题
    
    private SingletonThreadSafe(){}
    
    public static SingletonThreadSafe getInstance() {
        return instance;
    }
}
```

- 线程安全，线程锁。会让线程阻塞时间过长，因此该方法有性能问题，不推荐使用。

```java
// 线程安全，延迟实例化，线程锁
......
public static synchronized SingletonThreadSafe getInstance() {
    if (instance == null) {
        instance = new SingletonThreadSafe();
    }
    return uniqueInstance;
}
......
```

- 线程安全，双重校验锁。

```java
// 线程安全，延迟实例化，双重校验锁
public class SingletonThreadSafe {
    
    private volatile static SingletonThreadSafe instance;
    /* volatile确保当instance被初始化成实例时，多个线程正确的处理instance变量。
    创建instance不具备原子性，JVM指令重排会潜在导致某线程创建instance尚未完成时，另一个线程未检测到instance于是也进行创建。
    当共享变量被volatile修饰时，会保证修改的值立即被更新到主存，其他线程需读取时会去内存中读取新值。
    另外，通过synchronized和Lock也能够保证可见性。
    */
    // more about volatile: https://www.cnblogs.com/dolphin0520/p/3920373.html
    
    private SingletonThreadSafe() {}
    
    public static Singleton getInstance() {
        if (instance == null) { // 可能同时被两个线程执行并判true
            synchronized (Singleton.class) {
                if (instance == null) { // 双重校验锁
                    instance = new SingletonThreadSafe;
                }
            }
        return instance;
        }
    }
}
```



## 参考资料

- [图说设计模式：创建型模式](https://design-patterns.readthedocs.io/zh_CN/latest/creational_patterns/creational.html)
- [CyC2018/CS-Notes/设计模式](https://github.com/CyC2018/CS-Notes/blob/master/docs/notes/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F.md)
- [工厂模式 看这一篇就够了](https://www.jianshu.com/p/83ef48ce635b)
- [建造者模式（Builder Pattern）- 最易懂的设计模式解析](https://blog.csdn.net/carson_ho/article/details/54910597)
- [单例模式](https://www.jianshu.com/p/b3e5cd2004e8)
- [Java并发编程：volatile关键字解析](https://www.cnblogs.com/dolphin0520/p/3920373.html)