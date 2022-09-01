# 优化项目中大量if...else...


在项目中经常会用if..else...去判断逻辑，当业务越来越复杂，一个逻辑会有大量的判断，如下面的代码

```java
if(type.equals("o1")){
  // 执行o1逻辑
}else if(type.equals("o2")){
  // 执行o2逻辑
}else if(type.equals("o3")){
  // 执行o3逻辑
}else if(type.equals("o4")){
  // 执行o4逻辑
}else if(type.equals("o5")){
  // 执行o5逻辑
}
```

后面再增加逻辑o6,o7...一直到o99呢，这样写代码就会越写越长，后期越来越复杂，维护越来越难，嗯，已经闻到坏代码的味道，优化它。

首先想是否能通过设计模式来解决呢？

## 1.策略模式

**策略模式**：一个类的行为或其他算法可以在运行时更改。这种类型的设计模式属于行为型模式。

在策略模式中，我们创建表示各种策略的对象和一个行为随着策略对象改变而改变context对象。策略对象改变context对象的执行算法。

**关键词：实现同一个接口**

那如何实现策略模式呢？

我们把上面大量if...else...的代码，用策略模式来实现下

### 1.1首先创建一个接口

```java
public interface Strategy{
  public void doSomething();
}
```

### 1.2.实现接口的类

```java
public class OperationO1 implements Strategy{
  
  @Override
  public void doSomething(){
    System.out.println("执行o1逻辑。");
  }
}
```

```java
public class OperationO2 implements Strategy{
  
  @Override
  public void doSomething(){
    System.out.println("执行o2逻辑。");
  }
}
```

```java
public class OperationO3 implements Strategy{
  
  @Override
  public void doSomething(){
    System.out.println("执行o3逻辑。");
  }
}
```

### 1.3.创建Context类

```java
public class Conetext(){
  
  private Strategy strategy;
  
  public Conetext(Strategy strategy){
    this.strategy = strategy;
  }
  
  public void operation(){
    strategy.doSomething();
  }
}
```

### 1.4.验证

```java
public class Test{
  
  public static void main(String[] args){
    Conetext conetext = new Conetext(new OperationO1());
    conetext.operation();
    conetext = new Conetext(new OperationO2());
    conetext.operation();
    conetext = new Conetext(new OperationO3());
    conetext.operation();
  }
  
}
```

```sh
执行打印
执行o1逻辑。
执行o2逻辑。
执行o3逻辑。
```

ok，策略模式代码已经实现了，发现唉不对啊，这好像更if...else没有什么关系？下面来演示在项目中的实战。

## 2.表驱动

这就要引出另一个词**表驱动**

**表驱动法**，又称之为表驱动、表驱动方法。 “表”是几乎所有数据结构课本都要讨论的非常有用的数据结构。表驱动方法出于特定的目的来使用表，程序员们经常谈到“表驱动”方法，但是课本中却从未提到过什么是"表驱动"方法。表驱动方法是一种使你可以在表中查找信息，而不必用很多的逻辑语句（if或Case）来把它们找出来的方法。事实上，任何信息都可以通过表来挑选。在简单的情况下，逻辑语句往往更简单而且更直接。但随着逻辑链的复杂，表就变得越来越富有吸引力了。

表驱动+策略模式

### 2.1.新增策略管理类

```java
@Component
public class StrategyManager {

    private Map<String,Strategy> dispatcher = new HashMap<>();

    @PostConstruct
    private void initDispatcher(){
        dispatcher.put("o1",new OperationO1());
        dispatcher.put("o2",new OperationO2());
        dispatcher.put("o3",new OperationO3());
    }

    public void doStrategy(String type) throws Exception{
        Strategy strategy = dispatcher.get(type);
        if(strategy != null){
            strategy.doSomething();
        } else {
          throw new Exception("未找到业务策略。");  
        }
    }

}
```

### 2.2.注入并使用

```java
@Autowired
private StrategyManager strategyManager;

public void test(String type){
  strategyManager.doStrategy(type);
}
```

后期再添加新策略，只要在策略管理类中添加新增策略的实现类。

## 总结

最后记住一下秘诀

> 互斥条件表驱动，
>
> 嵌套条件校验链，
>
> 短路条件早return，
>
> 零散条件可组合。




