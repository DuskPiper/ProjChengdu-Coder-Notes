# Behavioral Pattern 行为型设计模式

行为型模式是对在不同的对象之间划分责任和算法的抽象化。行为型模式不仅仅关注类和对象的结构，而且重点关注它们之间的相互作用。

行为型模式分为类行为型模式和对象行为型模式两种：

- 类行为型模式：类的行为型模式使用继承关系在几个类之间分配行为，类行为型模式主要通过多态等方式来分配父类与子类的职责。
- 对象行为型模式：对象的行为型模式则使用对象的聚合关联关系来分配行为，对象行为型模式主要是通过对象关联等方式来分配两个或多个类的职责。根据“合成复用原则”，系统中要尽量使用关联关系来取代继承关系，因此大部分行为型设计模式都属于对象行为型设计模式。

行为型设计模式具体包含以下模式，按重要程度排序：

1. Iterator 迭代器模式
2. Observer 观察者模式
3. Command 命令模式
4. Strategy 策略模式
5. Chain of Responsibility 职责链模式
6. State 状态模式
7. Template Method 模版方法模式
8. Mediator 中介者模式
9. Memento 备忘录模式
10. Interpreter 解释器模式
11. Visitor 访问者模式
12. Null 空对象



## Chain of Responsibility 责任链

### 简述

使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链发送该请求，直到有一个对象处理它为止。

Handler：定义处理请求的接口，并且实现后继链（successor）。

![责任链图示](https://github.com/CyC2018/CS-Notes/raw/master/docs/notes/pics/691f11eb-31a7-46be-9de1-61f433c4b3c7.png)

### 实现

- 定义

```java 
public abstract class Handler { // 定义抽象的handler
    
    protected Handler successor;
    
    public Handler(Handler successor) {
        this.successor = successor;
    }
    
    protected abstract void handle (Request request);
}

public class Request { // 定义request用以传递信息
    
    private RequestType requestType;
    private String fileDir;
    
    public Request(RequestType requestType, String fileDir) {
        this.requestType = requesttype;
        this.fileDir = fileDir;
    }
    
    public RequestType getRequestType() { return requestType; }
    public String getFileDir() { return fileDir; }
}

public enum RequestType {COPY_FILE, DELETE_FILE} // 用枚举定义常量

public class CopyHandler extends Handler {
    public CopyHandler(Handler successor) {
        super(successor);
    }
    
    @Override
    protected void handle(Request request) {
        if (request.getRequestType() == RequestType.COPY_FILE) {
            String fileDir = request.getFileDir();
            /*handle request here*/
        } else if (successor != null) {
            successor.handle(request)
        }
    }
}

public class DeleteHandler extends Handler {
    public DeleteHandler(Handler successor) {
        super(successor)
    }
    
    @Override
    protected void handle(Request request) {
        if (request.getRequestType() == RequestType.DELETE_FILE) {
            String fileDir = request.getFileDir();
            /*handle request here*/
        } else if (successor != null) {
            successor.handle(request)
        }
    }
}
```

- 使用

```java
Handler copier = new CopyHandler(null);
Handler deleter = new DeleteHandler(copier); // 责任链：Delete -> Copy -> null

Request request1 = new Request(RequestType.COPY_FILE, "./file1.txt");
Request request2 = new Request(RequestType.DELETE_FILE, "./file2.txt");

deleter.handle(request1); // copier called
deleter.handle(request2); // deleter called
```

### JDK

- [java.util.logging.Logger#log()](http://docs.oracle.com/javase/8/docs/api/java/util/logging/Logger.html#log%28java.util.logging.Level,%20java.lang.String%29)
- [Apache Commons Chain](https://commons.apache.org/proper/commons-chain/index.html)
- [javax.servlet.Filter#doFilter()](http://docs.oracle.com/javaee/7/api/javax/servlet/Filter.html#doFilter-javax.servlet.ServletRequest-javax.servlet.ServletResponse-javax.servlet.FilterChain-)



## Command 命令

### 简述

将一个请求封装为一个对象，从而使我们可用不同的请求对客户进行参数化；对请求排队或者记录请求日志，以及支持可撤销的操作。命令模式是一种对象行为型模式，其别名为动作(Action)模式或事务(Transaction)模式。

- Command：命令
- Receiver：命令接收者，也就是命令真正的执行者
- Invoker：通过它来调用命令
- Client：可以设置命令与命令的接收者

![命令图示](https://github.com/CyC2018/CS-Notes/raw/master/docs/notes/pics/ae1b27b8-bc13-42e7-ac12-a2242e125499.png)

### 实现

- 定义

```java
public interface Command {
    void execute();
}

public class Light {
    private boolean isOn = false;

    public void turnOn() {
        /*turn light on*/
        isOn = true;
    }

    public void turnOff() {
        /*turn light off*/
        isOn = false;
    }
}

public class LightOnCommand implements Command {
    Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOn();
    }
}

public class LightOffCommand implements Command {
    Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOff();
    }
}

public class Invoker { // 遥控器
    private Command[] commands;
    private final int slotNum = 7; // 按钮数量
    
    public Invoker() {
        this.commands = new Command[slotNum];
    }
    
    public void setCommand(Command command, int slot) { // 设置特定按钮功能
        commands[slot] = command;
    }
    
    public void onButtonTapped(int slot) {
        command[slot].execute();
    }
}
```

- 使用

```java
Invoker invoker = new Invoker();
Light light = new Light();
Command lightOnCommand = new LightOnCommand(light);
Command lightOffCommand = new LightOffCommand(light);
invoker.setCommand(lightOnCommand, 0);
invoker.setCommand(lightOffCommand, 1);

for(int i = 0, int i < Integer.MAX_VALUE; i ++) { // 按爆遥控器
    invoker.onButtonTapped(i % 2);
}
```

### JDK

- [java.lang.Runnable](http://docs.oracle.com/javase/8/docs/api/java/lang/Runnable.html)
- [Netflix Hystrix](https://github.com/Netflix/Hystrix/wiki)
- [javax.swing.Action](http://docs.oracle.com/javase/8/docs/api/javax/swing/Action.html)



##Interpreter 解释器

### 简述

给定一个语言之后，解释器模式可以定义出其文法的一种表示，并同时提供一个解释器。客户端可以使用这个解释器来解释这个语言中的句子。

- TerminalExpression：终结符表达式，每个终结符都需要一个 TerminalExpression。
- Context：上下文，包含解释器之外的一些全局信息。

解释器模式在实际开发中使用少，因为会引起效率、性能及维护等问题。

[解释器示例](https://blog.csdn.net/yanbober/article/details/45537601)

### JDK

- [java.util.Pattern](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html)
- [java.text.Normalizer](http://docs.oracle.com/javase/8/docs/api/java/text/Normalizer.html)
- All subclasses of [java.text.Format](http://docs.oracle.com/javase/8/docs/api/java/text/Format.html)
- [javax.el.ELResolver](http://docs.oracle.com/javaee/7/api/javax/el/ELResolver.html)



## Iterator 迭代器

### 简述

提供一种顺序访问聚合对象元素的方法，并且不暴露聚合对象的内部表示。

Iterator 主要定义了 hasNext() 和 next() 方法。

### 实现

- 定义

```java
interface Iterator {
    public Object next();
    public boolean hasNext();
}

interface Aggregate {
    public void add(Object object);
    public void remove(Object object);
    public Iterator iterator();
}

class ConcreteIterator implements Iterator {
    private List list = new ArrayList();
    private int cursor = 0;
    
    public ConcreteIterator(List list) {
        this.list = list;
    }
    
    public boolean hasNext() {
        if (cursor == list.size()) {
            return false;
        } else {
            return true;
        }
    }
    
    public Object next() {
        Object object = null;
        if(this.hasNext()) {
            object = list.get(cursor ++;)
        }
        return object;
    }
}

class ConcreteAggregate implements Aggregate {
    private List list = new ArrayList();
    
    public void add(Object obj) {/*add*/}
    public void remove(Object obj) {/*remove*/}
    
    public Iterator iterator() {
        return new ConcreteIterator(list);
    }
}
```

### JDK

- [java.util.Iterator](http://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)
- [java.util.Enumeration](http://docs.oracle.com/javase/8/docs/api/java/util/Enumeration.html)



# Mediator 中介者







## 参考资料

- [CyC2018/CS-Notes](https://github.com/CyC2018/CS-Notes/blob/master/docs/notes/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F.md#%E4%B8%89%E8%A1%8C%E4%B8%BA%E5%9E%8B)

- [图说设计模式 行为型模式](https://design-patterns.readthedocs.io/zh_CN/latest/behavioral_patterns/behavioral.html)
- [设计模式(行为型)之解释器模式(Interpreter Pattern)](https://blog.csdn.net/yanbober/article/details/45537601)