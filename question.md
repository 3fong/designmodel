## 问题

### ????AtomicInteger是否符合单一职责原则和接口隔离原则


```

/**
 * Atomically increments by one the current value.
 * @return the previous value
 */
public final int getAndIncrement() {//...}
```
java.util.concurrent 并发包提供了 AtomicInteger 这样一个原子类，其中有一个函数 getAndIncrement() 是这样定义的：给整数增加一，并且返回未増之前的值。我的问题是，这个函数的设计是否符合单一职责原则和接口隔离原则？为什么？

答: 符合单一职责原则,因为函数内聚的完成了数值的获取和新增,它是一个整体功能;     
不满足接口隔离原则:因为返回值是旧值,这样获取操作和新增操作其实是两个操作,针对获取来说,新增是不必要的行为.


### 依赖注入与基于接口而非实现的异同

1. 核心原则都是不直接依赖实现;而是依赖引用,提高代码依赖复用性.
    这里复用性:依赖注入是对象创建复用,是生命周期的延长;接口的复用是调用方代码的复用;
2. 依赖注入是更宽泛的实现方式,它可以是类和接口;而基于接口而非实现是一种原则,只适用于接口.

### 如何看待重复造轮子?什么时候重复造轮子?什么使用现成工具类和开源框架?

造轮子肯定是有成本的,当现有轮子满足自己的需要时优先选择现有的;    
通用功能不适合重复造轮子,但是核心技术都要自研.这里自研并非完全自己开发,可以基于某个基准二次开发;    
实际开发中优先使用工具类和开源框架,当不满足实际需求时再考虑自研;

### ??? 代码重复,除了实现逻辑重复、功能语义重复和代码执行重复是否还有其他的


### ??? “高内聚、松耦合”“单一职责原则”“接口隔离原则”“基于接口而非实现编程”“迪米特法则”，你能总结一下它们之间的区别和联系吗

高内聚,松耦合:是整体设计思想.其他四种原则都是它的一种实现指导    
单一职责原则:针对的是模块、类、接口的设计.按职责划分单一的类,代码内聚性高,但是耦合度可能还是较强;    
接口隔离原则:面向调用者的接口设计.较单一职责原则更细粒度的分类划分.只依赖自己需要的接口.耦合度低;    
基于接口而非实现编程:从使用者角度出发.通过接口引用外部依赖,避免生命周期耦合;    
迪米特法则:从对外关系出发.    


### logger类是否应该依赖注入,static final并且内部创建是否影响可测试性?

依赖注入可提高可测试的原因是,它可以轻松替换依赖的真实对象.我们mock数据的原因是业务逻辑存在不可控的情形.但是logger只写不读,同时不参与业务逻辑处理,所以不需要进行mock测试;


### ??? 只要把对象 new 操作和初始化操作设计为原子操作，就自然能禁止重排序

单例模式中指令重排序造成的线程不安全问题

```

public class IdGenerator { 
  private AtomicLong id = new AtomicLong(0);
  private static IdGenerator instance;
  private IdGenerator() {}
  public static IdGenerator getInstance() {
    if (instance == null) {
      synchronized(IdGenerator.class) { // 此处为类级别的锁
        if (instance == null) {
          instance = new IdGenerator();
        }
      }
    }
    return instance;
  }
  public long getId() { 
    return id.incrementAndGet();
  }
}
```

### 单例如何支持多实例

```
Singleton singleton1 = Singleton.getInstance(10, 50);
Singleton singleton2 = Singleton.getInstance(20, 30);
```
单例支持多实例的核心是创建多个单例:    
初始化是将可支持单例放入到一个容器中,例如map,这样就可以获取多实例了


#### 问题spring事务,数据库事务管理能否基于单一接口解决分布式事务问题?
场景:

跨表
跨库











