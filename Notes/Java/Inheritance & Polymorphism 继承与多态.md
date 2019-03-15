# Inheritance & Polymorphism 继承与多态

继承就是子类继承父类的特征和行为，使得子类对象（实例）具有父类的实例域和方法，或子类从父类继承方法，使得子类具有父类相同的行为。

## 特点

- Java 不支持多继承，但支持多重继承。(参看**Java实现多重继承**)
- 子类拥有父类非 private 的属性、方法。
- 子类可以用自己的方式实现父类的方法。(参看**override和overload**)

## 实现

### 继承类和继承接口

```java
public class Fairy {}
public class Cirno extends Fairy {} // 继承父类
```

```java
public interface IceAttack {}
public interface Baka {}
public class Cirno implements IceAttack, Baka {} // 继承接口
```

### `final`关键字

`final` 关键字声明类可以把类定义为不能继承的最终类、或者用于修饰方法使该方法不能被子类重写。

```java
final class Cirno {}

(public||private||default||protected) final void Attack() {}
```

### 多重继承

#### Java不能多重继承的原因

- 避免矛盾的overloading/overriding，避免diamond inheritance会带来的问题。
- 使Java内部机制更简单高效，亦方便用户使用。

#### Java实现多重继承

1. implement multiple interfaces

2. internal classes

   ```java
   public class Cirno {
       private class IceCirno extends IceFairy {}
       private class BakaCirno extends Baka {}
   }
   ```

##Polymorphism 多态

多态，意味着一个对象有着多重特征，可以在特定的情况下，表现不同的状态，从而对应着不同的属性和方法。旨在消除类型之间的耦合关系。

当使用多态方式调用方法时，首先检查父类中是否有该方法，如果没有，则编译错误；如果有，再去调用子类的同名方法。

### 覆盖(重写) Override

覆盖了一个方法并对其重写，以求达到不同的作用。对我们来说最熟悉的覆盖就是对接口方法的实现，在接口中一般只是 对方法进行了声明，而我们在实现时，就需要实现接口声明的所有方法。除了这个典型的用法以外，我们在继承中也可能会在子类覆盖父类中的方法。注意：

1. 覆盖方法的标志必须和被覆盖方法的标志完全匹配；
2. 覆盖方法的返回值类型必须和被覆盖方法一致；
3. 覆盖方法所抛出的异常必须和被覆盖方法的所抛出的异常一致，或者是其子类；
4. 被覆盖方法不能为 private，否则在其子类中只是新定义了一个方法，并没有对其进行覆盖。

### 重载 Overload

定义一些名称相同的方法，通过定义不同的输入参数来区分这些方法，然后再调用时，VM就会根据不同的参数样式，来选择合适的方法执行。注意：

1. 使用重载时只能通过不同的参数样式。不同的参数类型/个数/顺序，同一方法内的几个参数类型必须不一样，例如可以是 fun(int,float)，但是不能为 fun(int,int)；
2. 不能通过访问权限、返回类型、抛出的异常进行重载；
3. 方法的异常类型和数目不会对重载造成影响；
4. 对于继承，如果方法在父类是 private，就不能在子类重载。否则也只是定义一个新方法而无重载效果。

##References

- [菜鸟教程 - 继承](http://www.runoob.com/java/java-inheritance.html)
- [Java实现多重继承技巧](https://blog.csdn.net/hemin1003/article/details/55095347)
- [为什么Java不支持多重继承?](http://www.importnew.com/4604.html)
- [菜鸟教程 - 多态](http://www.runoob.com/java/java-polymorphism.html)
- [Java 编程下 Overload 和 Override 的区别](https://www.cnblogs.com/sunzn/archive/2013/03/30/2990187.html)