

### 设计模式pdf与源码



## 感悟

```java
书从来不是看的而是用的
    在这个学习资料料⼏乎爆炸的时代，甚⾄至你可以轻易就获取⼏个T的视频，⼩手轻⼀点就收藏⼀堆文章，但却很少去看。学习的过程从不只是简单的看一遍就可以，对于一些实操性的技术书籍，如果真的
希望学习到知识，那么⼀定是把这本书⽤用起来⽽绝对不是看起来。
```



## 创造者模式

### 1. 工厂方法模式

其是在⽗父类中提供⼀一个创建对象的⽅方法， 允许⼦子类决定实例例化对象的类型。  主要意图是定义⼀一个创建对象的接口，让其⼦类⾃己决定实例例化哪⼀一个⼯厂类，⼯厂模式使其创建过程延迟到⼦类行。 

优点：业务解耦，单一职责，一个类或方法只干一件事

```java
其实就是工厂定义了总的接口，让其子类根据自身业务需求，实现其方法。
```



### 2， 抽象工厂模式

抽象⼯厂是一个中心⼯厂，创建其他⼯厂的模式。   

```java
抽象⼯厂模式，所要解决的问题就是在⼀个产品族，存在多个不同类型的产品(Redis集群、操作系
统)情况下，接⼝选择的问题。⽽这种场景在业务开发中也是⾮常多⻅见的，只不过可能有时候没有
将它们抽象化出来
```

### 3. 建造者

建造者模式所完成的内容就是通过将多个简单对象通过⼀步的组装构建出⼀个复杂对象的过程。  

```java
将⼀个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示。
    
一些基本物料不会变，⽽其组合经常变化的时候 ，就可以选择这样的设计模式来构建代码
```

### 4. 原型模式

```java
便于通过克隆方式创建复杂对象、也可以避免重复做初始化操作、
不需要与类中所属的其他类耦合等。但也有一些缺点如果对象中包括了了循环引⽤用的克隆，以及类中
深度使⽤对象的克隆，都会使此模式变得异常麻烦。
```



**心得**

```java
初期是代码的优化，中期是设计模式的使⽤用，后期是把控全局服务的搭建。不断的加强⾃自⼰己对全局
能⼒力的把控，也加深⾃自己对细节的处理。可上可下才是⼀一个程序员最佳处理⽅方式，选取做合适的才
是最好的选择。
```



### 5.单例模式

```java
⼀个全局使用的类频繁的创建和消费，从⽽提升提升整体的代码的性能。
```

### 举例

```java
1. 数据库的连接池不会反复创建
2. spring中⼀个单例例模式bean的生成和使用
```

#### 1. 懒汉模式（线程不安全）

```java
package com.chen;

// 1.   懒汉模式（线程不安全）

/**
 * 单例例模式有⼀一个特点就是不不允许外部直接创建，也就是 new Singleton_01() ，
 * 因此这⾥里里在默认的构造函数上添加了了私有属性 private 。
 */
public class Singleton_01 {
    private static Singleton_01 instance;

    // 私有构造器
    private Singleton_01(){
    }

    // 创建静态方法返回单例对象
    public static Singleton_01 getInstance(){
        if(null != instance) return instance;
        instance = new Singleton_01();
        return instance;
    }
}
```

**2. 懒汉模式（线程安全，加锁）**

```java
package com.chen;

/**
 * @author TrueNewBee
 * @date 2021/4/28 13:46
 * @details 懒汉模式（线程安全）
 */

/**
 * 此种模式虽然是安全的，但由于把锁加到⽅方法上后，
 * 所有的访问都因需要锁占⽤用导致资源的浪费。
 * 如果不不是特殊情况下，不不建议此种⽅方式实现单例例模式。
 */

public class Singleton_02 {
    private static Singleton_02 instance;

    private Singleton_02(){

    }
    // 加了锁，线程安全
    public static synchronized Singleton_02 getInstance(){
        if (null != instance) return instance;
        instance = new Singleton_02();
        return instance;
    }
}
```

**3.饿汉模式（线程安全）**

```java
package com.chen;

/**
 * @author TrueNewBee
 * @date 2021/4/28 13:49
 * @details 饿汉模式（线程安全）
 */

/**
 * 虽然是线程安全，但是一次性加载所有东西，占用过多资源，
 * 故称为 饿汉！！
 */
public class Singleton_03 {
    // 创建一个静态私有对象
    private static Singleton_03 instance = new Singleton_03();

    private Singleton_03(){
    }
    public static Singleton_03 getInstance(){
        return instance;
    }
}
```

**4.使用类的内部类（线程安全， 墙裂推荐）**

```java
package com.chen;

/**
 * @author TrueNewBee
 * @date 2021/4/28 14:01
 * @details 使用类的内部类（线程安全） 推荐推荐推荐推荐推荐推荐推荐推荐
 */

/**
 * 使⽤用类的静态内部类实现的单例例模式，既保证了了线程安全有保证了了懒加载，
 * 同时不不会因为加锁的⽅式耗费性能
 */
public class Singleton_04 {

    // 创建私有内部类生成单例对象
    private static class SingletonHolder{
        private static Singleton_04 instance = new Singleton_04();
    }

    private Singleton_04(){
    }

    // 返回单例对象
    public static Singleton_04 getInstance(){
        return SingletonHolder.instance;
    }
}
```

**5.双重锁校验（线程安全）**

```java
package com.chen;

/**
 * @author TrueNewBee
 * @date 2021/4/28 14:13
 * @details 双重锁校验（线程安全）
 */

/**
 * 双重锁的⽅方式是⽅方法级锁的优化，减少了了部分获取实例例的耗时。
 * 同时这种⽅方式也满⾜足了了懒加载。
 */
public class Singleton_05 {
    private static Singleton_05 instance;

    private Singleton_05() {
    }

    public static Singleton_05 getInstance() {
        if (null != instance) return instance;
        synchronized (Singleton_05.class) {
            if (null == instance) {
                instance = new Singleton_05();
            }
        }
        return instance;
    }
}
```

**6.CAS[AtomicReference] (线程安全)**

```java
package com.chen;

/**
 * @author TrueNewBee
 * @date 2021/4/28 14:19
 * @details CAS[AtomicReference] (线程安全)
 * 由于太长了，就直接CV过来了
 */

import java.util.concurrent.atomic.AtomicReference;

/**
 * java并发库提供了了很多原⼦子类来⽀支持并发访问的数据安全
 * 性； AtomicInteger 、 AtomicBoolean 、 AtomicLong 、 AtomicReference 。
 * AtomicReference 可以封装引⽤用⼀一个V实例例，⽀支持并发访问如上的单例例⽅方式就是使⽤用了了这样的⼀一个
 * 特点。
 * 使⽤用CAS的好处就是不不需要使⽤用传统的加锁⽅方式保证线程安全，⽽而是依赖于CAS的忙等算法，依赖
 * 于底层硬件的实现，来保证线程安全。相对于其他锁的实现没有线程的切换和阻塞也就没有了了额外
 * 的开销，并且可以⽀支持较⼤大的并发性。
 * 当然CAS也有⼀一个缺点就是忙等，如果⼀一直没有获取到将会处于死循环中
 */
public class Singleton_06 {
    private static final AtomicReference<Singleton_06> INSTANCE = new
            AtomicReference<Singleton_06>();
    private static Singleton_06 instance;

    private Singleton_06() {
    }

    public static final Singleton_06 getInstance() {
        for (; ; ) {
            Singleton_06 instance = INSTANCE.get();
            if (null != instance) return instance;
            INSTANCE.compareAndSet(null, new Singleton_06());
            return INSTANCE.get();
        }
    }

    public static void main(String[] args) {
        System.out.println(Singleton_06.getInstance());
//       org.itstack.demo.design.Singleton_06@2b193f2d
        System.out.println(Singleton_06.getInstance());
//        org.itstack.demo.design.Singleton_06@2b193f2d
    }
}
```

**7.Effective Java作者推荐的枚举单例例(线程安全)**

```java
package com.chen;

/**
 * @author TrueNewBee
 * @date 2021/4/28 14:29
 * @details Effective Java作者推荐的枚举单例例(线程安全)
 */

/**
 * Effective Java 作者推荐使⽤用枚举的⽅方式解决单例例模式，此种⽅方式可能是平时最少⽤用到的。
 * 这种⽅方式解决了了最主要的；线程安全、⾃自由串串⾏行行化、单⼀一实例例
 */
public enum Singleton_07 {
    INSTANCE;
    public void test(){
        System.out.println("hi~");
    }
}

/**
 * 调用方法
 * @Test
 * public void test() {
 * Singleton_07.INSTANCE.test();
 */
```

### 关于单例的总结

```java
虽然只是一个很平常的单例例模式，但在各种的实现上真的可以看到java的基本功的体现，这⾥包括了；懒汉、饿汉、线程是否安全、静态类、内部类、加锁、串串行化等。在平时的开发中如果可以确保此类是全局可⽤不需要做懒加载，那么直接创建并给外部调⽤用即可。但如果是很多的类，有些需要在⽤用户触发⼀一定的条件后(游戏关卡)才显示，那么一定要⽤用懒加载。线程的安全上可以按需选择。
```



## 结构模式

### 1. 适配器器模式  

```java
适配器模式要解决的主要问题就是多种差异化类型的接⼝做统⼀输出，这在我们学习⼯⼚厂⽅法模式中也
有所提到不同种类的奖品处理，其实那也是适配器的应用。
```



### 2. 桥接模式

```java
桥接模式的主要作⽤就是通过将抽象部分与实现部分离，把多种可匹配的使⽤进⾏行组合。说⽩了了核⼼实现也就是在A类中含有B类接⼝，通过构造函数传递B类的实现，这个B类就是设计的桥 。
    
比如第三方平台收钱吧，把支付宝微信与卖鸡蛋灌饼的所连接
    
```



### 3. 组合模式

**很多时候因为你的极致追求和稍有倔强的⼯匠精神，即使在⾯对同样的业务需求，你能完成出最好
的代码结构和最易于扩展的技术架构。 不要被远不能给你指导提升能⼒的影响到放弃⾃己的追求**

```java
就是把几种模式组合到一起，原谅我太多，没细细看，可以自己去看pdf
 

```



### 4. 装饰器模式

**应用场景，单点登入**

```java
装饰器起到了增强的作用	
    
装饰器主要解决的是直接继承下因功能的不断横向扩展导致⼦类膨胀的问题，⽽是⽤用装饰器模式后就会
⽐直接继承显得更更加灵活同时这样也就不再需要考虑⼦子类的维护。
```



```java
在装饰器器模式中有四个⽐比较重要点抽象出来的点；
1. 抽象构件⻆色(Component) - 定义抽象接⼝
2. 具体构件⻆色(ConcreteComponent) - 实现抽象接⼝，可以是一组
3. 装饰⻆色(Decorator) - 定义抽象类并继承接⼝中的⽅法，保证一致性
4. 具体装饰⻆色(ConcreteDecorator) - 扩展装饰具体的实现逻辑
```



### 5. 外观模式

```java
通过中间件的⽅式实现外观模式，这样的设计可以很好的增强代码的隔离性，以及复用
性，不不仅使⽤上⾮常灵活也降低了了每⼀个系统都开发这样的服务带来的⻛险。
```



### 6. 享元模式

**技术类书籍和其他书籍不同，只要不去⽤看了也就只是轻描淡写，很难接纳和理解。就像设计模式，虽然可能看了了几遍，但是在实际编码中仍然很少会⽤，大部分原因还是没有认认真的跟着实操。事必躬亲才是学习编程的最好是⽅式。**

```java
关于享元模式的设计可以着重学习享元⼯厂的设计，在⼀些有⼤量重复对象可复用的场景下，使⽤此场景在服务端减少接⼝口的调⽤，在客户端减少内存的占用。是这个设计模式的主要应⽤方式
```



### 7.代理模式

**除了了这些优化提升外，还有那么⼴阔的技术体系栈，都可能因为你只是注重CRUD⽽而被忽略；字节码编程、领域驱动设计架构、代理模式中间件开发、 JVM虚拟机实现原理等**

```java
代理模式有点像老大和⼩小弟，也有点像分销商。主要解决的是问题是为某些资源的访问、对象的类的易
⽤操作上提供方便使用的代理服务。而这种设计思想的模式经常会出现在我们的系统中，或者你用到过的组件中，它们都提供给你⼀一种⾮常简单易⽤的⽅式控制原本你需要编写很多代码的进行使⽤的服务类。
```



### 2021-05-02 17:19:18

其实还有余下10个模式，没写，大致看看，，主要是解耦，一个类干一件事，模式都差不错！
