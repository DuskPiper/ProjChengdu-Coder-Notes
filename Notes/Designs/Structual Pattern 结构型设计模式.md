#Structual Pattern 结构型设计模式

结构型模式(Structural Pattern)描述如何将类或者对 象结合在一起形成更大的结构，就像搭积木，可以通过 简单积木的组合形成复杂的、功能更为强大的结构。

结构型模式分为类结构型模式和对象结构型模式：

- **类结构型模式**关心类的组合，由多个类组合成更大的系统，类结构型模式中一般只存在继承关系和实现关系。

- **对象结构型模式**关心类与对象的组合，通过关联关系使得在一个类中定义另一个类的实例对象，然后通过该对象调用其方法。 根据“合成复用原则”，在系统中尽量使用关联关系来替代继承关系，因此大部分结构型模式都是对象结构型模式。

结构型设计模式具体主要包含以下模式：

1. Adapter 适配器
2. Bridge 桥接
3. Composite 组合
4. Decorator 装饰
5. Facade 外观
6. Flyweight 享元
7. Proxy 代理



## Adapter 适配器

### 简述

把一个类接口转换成另一个用户需要的接口，使接口不兼容的类可以一起工作，别名为包装器(Wrapper)。

适配器模式既可以作为类结构型模式，也可以作为对象结构型模式。

### 实现

```java
public interface Duck {
    void quack();
}
public interface Turkey {
    void gobble();
}
public class WildTurkey implements Turkey {
    @Override
    public void gobble() {
        System.out.println("gobble!");
    }
}
public class TurkeyAdapter implements Duck {
    Turkey turkey;

    public TurkeyAdapter(Turkey turkey) {
        this.turkey = turkey;
    }

    @Override
    public void quack() {
        turkey.gobble();
    }
}
public class Demo {
    public static void main(String[] args) {
        Turkey turkey = new WildTurkey();
        Duck duck = new TurkeyAdapter(turkey);
        duck.quack();
    }
}
```



## Bridge 桥接

### 简述

将抽象部分与实现部分分离，使它们都可以独立的变化。桥接适用于把抽象化与实现化解耦，使得二者可以独立变化。这种模式涉及到一个作为桥接的接口，使得实体类的功能独立于接口实现类。这两种类型的类可被结构化改变而互不影响。

桥接模式属于结构型模式。

### 实现

```java
public interface DrawAPI {
   public void drawCircle(int radius, int x, int y);
}

public class RedCircle implements DrawAPI {
   @Override
   public void drawCircle(int radius, int x, int y) {
      System.out.println("Drawing Circle[ color: red, radius: "
         + radius +", x: " +x+", "+ y +"]");
   }
}

public class GreenCircle implements DrawAPI {
   @Override
   public void drawCircle(int radius, int x, int y) {
      System.out.println("Drawing Circle[ color: green, radius: "
         + radius +", x: " +x+", "+ y +"]");
   }
}

public abstract class Shape {
   protected DrawAPI drawAPI;
   protected Shape(DrawAPI drawAPI){
      this.drawAPI = drawAPI;
   }
   public abstract void draw();  
}

public class Circle extends Shape {
   private int x, y, radius;
 
   public Circle(int x, int y, int radius, DrawAPI drawAPI) {
      super(drawAPI);
      this.x = x;  
      this.y = y;  
      this.radius = radius;
   }
 
   public void draw() {
      drawAPI.drawCircle(radius,x,y);
   }
}

public class Demo {
   public static void main(String[] args) {
      Shape redCircle = new Circle(100,100, 10, new RedCircle());
      Shape greenCircle = new Circle(100,100, 10, new GreenCircle());
 
      redCircle.draw();
      greenCircle.draw();
   }
}
```



## Composite 组合

### 简述

又叫部分整体模式，是用于把一组相似的对象当作一个单一的对象。组合模式依据树形结构来组合对象，用来表示部分以及整体层次。

组合模式属于结构型模式，它创建了对象组的树形结构。

### 实现

```java
public class Employee {
   private String name;
   private String dept;
   private int salary;
   private List<Employee> subordinates;

   public Employee(String name, String dept, int sal) {
      this.name = name;
      this.dept = dept;
      this.salary = sal;
      subordinates = new ArrayList<Employee>();
   }
 
   public void add(Employee e) {
      subordinates.add(e);
   }
 
   public void remove(Employee e) {
      subordinates.remove(e);
   }
 
   public List<Employee> getSubordinates(){
     return subordinates;
   }
 
   public String toString(){
      return ("Employee :[ Name : "+ name +", dept : "+ dept + ", salary :"+ salary+" ]");
   }   
}

public class Demo {
   public static void main(String[] args) {
      Employee CEO = new Employee("Jeff", "CEO", 30000);
      Employee headSales = new Employee("Robert", "Head Sales", 20000);
      Employee headMarketing = new Employee("Michel", "Head Marketing", 20000);
      Employee clerk1 = new Employee("Laura", "Marketing", 10000);
      Employee clerk2 = new Employee("Bob", "Marketing", 10000);
      Employee salesExecutive1 = new Employee("Richard","Sales", 10000);
      Employee salesExecutive2 = new Employee("Rob","Sales", 10000);
 
      CEO.add(headSales);
      CEO.add(headMarketing);
      headSales.add(salesExecutive1);
      headSales.add(salesExecutive2);
      headMarketing.add(clerk1);
      headMarketing.add(clerk2);
 
      // print all emplyees
      System.out.println(CEO); 
      for (Employee headEmployee : CEO.getSubordinates()) {
         System.out.println(headEmployee);
         for (Employee employee : headEmployee.getSubordinates()) {
            System.out.println(employee);
         }
      }        
   }
}
```



## Decorator 装饰

### 简述

装饰模式以对客户透明的方式动态地给一个对象附加上更多的责任，换言之，客户端并不会觉得对象在装饰前和装饰后有什么不同。装饰模式可以在不需要创造更多子类的情况下，将对象的功能加以扩展。

通常有两种实现：

- **继承机制**，但是是静态的(嵌进代码的)，用户不能动态控制增加行为的方式和时机。
- **关联机制**，将一个类的对象嵌入另一个对象中，由另一个对象来决定是否调用嵌入对象的行为以便扩展自己的行为，我们称这个嵌入的对象为装饰器(Decorator)。

装饰模式属于结构型模式。

### 实现

```java
public interface Shape {
   void draw();
}

public class Rectangle implements Shape {
   public void draw() {
      System.out.println("Shape: Rectangle");
   }
}

public class Circle implements Shape {
   public void draw() {
      System.out.println("Shape: Circle");
   }
}

public abstract class ShapeDecorator implements Shape {
   protected Shape decoratedShape;
 
   public ShapeDecorator(Shape decoratedShape){
      this.decoratedShape = decoratedShape;
   }
 
   public void draw(){
      decoratedShape.draw();
   }  
}

public class RedShapeDecorator extends ShapeDecorator {  
  // function: set red border for whatever shape
 
   public RedShapeDecorator(Shape decoratedShape) {
      super(decoratedShape);     
   }
 
   @Override
   public void draw() {
      decoratedShape.draw();         
      setRedBorder(decoratedShape);
   }
 
   private void setRedBorder(Shape decoratedShape){
      System.out.println("Border Color: Red");
   }
}

public class Demo {
   public static void main(String[] args) {
 
      Shape circle = new Circle();
      ShapeDecorator redCircle = new RedShapeDecorator(new Circle());
      ShapeDecorator redRectangle = new RedShapeDecorator(new Rectangle());
      System.out.println("Circle with normal border");
      circle.draw();
 
      System.out.println("\nCircle of red border");
      redCircle.draw();
 
      System.out.println("\nRectangle of red border");
      redRectangle.draw();
   }
}
```



## Facade 外观

### 简述

隐藏系统的复杂性，并向客户端提供了一个客户端可以访问系统的**接口**。

外观模式属于结构型模式。

### 实现

```java
public interface Shape {
   void draw();
}

public class Rectangle implements Shape {
   @Override
   public void draw() {
      System.out.println("Rectangle::draw()");
   }
}

public class Square implements Shape {
   @Override
   public void draw() {
      System.out.println("Square::draw()");
   }
}

public class ShapeMaker {
   private Shape rectangle;
   private Shape square;
 
   public ShapeMaker() {
      rectangle = new Rectangle();
      square = new Square();
   }

   public void drawRectangle(){
      rectangle.draw();
   }
   public void drawSquare(){
      square.draw();
   }
}

public class Demo {
   public static void main(String[] args) {
      ShapeMaker shapeMaker = new ShapeMaker();
 
      shapeMaker.drawRectangle();
      shapeMaker.drawSquare();      
   }
}
```



## Flyweight 享元

### 简述

运用共享有效支持大量细粒度对象的复用。系统只使用少量的对象，而这些对象都很相似，状态变化很小，可以实现对象的多次复用。由于享元模式要求能够共享的对象必须是细粒度对象，因此又称轻量级模式。

主要用于减少创建对象的数量，以减少内存占用和提高性能。

享元模式是一种对象结构型模式。

###实现

```java
public class Circle implements Shape {
   private String color;
   private int x;
   private int y;
   private int radius;
 
   public Circle(String color){ this.color = color; }
   public void setX(int x) { this.x = x; }
   public void setY(int y) { this.y = y; }
   public void setRadius(int radius) { this.radius = radius; }
 
   @Override
   public void draw() {
      System.out.println("Circle: Draw() [Color : " + color 
         +", x : " + x +", y :" + y +", radius :" + radius);
   }
}

public class ShapeFactory {
   private static final HashMap<String, Shape> circleMap = new HashMap<>();
 
   public static Shape getCircle(String color) {
      Circle circle = (Circle)circleMap.get(color);
 
      if(circle == null) {
         circle = new Circle(color);
         circleMap.put(color, circle);
         System.out.println("Creating circle of color : " + color);
      }
      return circle;
   }
}

public class Demo {
   private static final String colors[] = { "Red", "Green", "Blue", "White", "Black" };
   public static void main(String[] args) {
      for(int i=0; i < 20; ++i) {
         Circle circle = (Circle)ShapeFactory.getCircle(getRandomColor());
         circle.setX((int)(Math.random()*100);
         circle.setY((int)(Math.random()*100);
         circle.setRadius(100);
         circle.draw();
      }
   }
   private static String getRandomColor() {
      return colors[(int)(Math.random() * colors.length)];
   }
}
```



## Proxy 代理

### 简述

用一个类代表某个特定类的功能，为其他对象提供一种代理以控制对该类的对象的访问。

这种类型的设计模式属于结构型模式。

### 实现

```java
public interface Image {
   void display();
}

public class RealImage implements Image {
 
   private String fileName;
 
   public RealImage(String fileName){
      this.fileName = fileName;
      loadFromDisk(fileName);
   }
 
   @Override
   public void display() {
      System.out.println("Displaying " + fileName);
   }
 
   private void loadFromDisk(String fileName){
      System.out.println("Loading " + fileName);
   }
}

public class ProxyImage implements Image{
 
   private RealImage realImage;
   private String fileName;
 
   public ProxyImage(String fileName){
      this.fileName = fileName;
   }
 
   @Override
   public void display() {
      if(realImage == null){
         realImage = new RealImage(fileName);
      }
      realImage.display();
   }
}

public class ProxyPatternDemo {
   
   public static void main(String[] args) {
      Image image = new ProxyImage("test_10mb.jpg");

      image.display();  // loading from disk
      System.out.println("");
      image.display();  // re-loading from RAM
   }
}
```





## 参考资料

- [结构型模式](https://design-patterns.readthedocs.io/zh_CN/latest/structural_patterns/structural.html)
- [菜鸟教程](http://www.runoob.com/design-pattern/design-pattern-tutorial.html)

