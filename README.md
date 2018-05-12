# DesignPatterns for Java

# 1. 单例模式

内部类实现线程安全的单例模式

# 2. 责任链模式
## 2.1 概念

一个请求有多个对象来处理，这些对象形成一条链，根据条件确定具体由谁来处理，如果当前对象不能处理则传递给该链中的下一个对象，直到有对象处理它为止。
责任链模式通过将请求和处理分离开来，以进行解耦。职责链模式结构的核心在于引入了一个抽象处理者。

职责链模式通过建立一条链来组织请求的处理者，请求将沿着链进行传递，请求发送者无须知道请求在何时、何处以及如何被处理，实现了请求发送者与处理者的解耦。
在软件开发中，如果遇到有多个对象可以处理同一请求时可以应用职责链模式，例如在Web应用开发中创建一个过滤器(Filter)链来对请求数据进行过滤，
在工作流系统中实现公文的分级审批等等，使用职责链模式可以较好地解决此类问题。

## 2.2 模式结构

Handler（抽象处理者）： 定义一个处理请求的接口，提供对后续处理者的引用
ConcreteHandler（具体处理者）： 抽象处理者的子类，处理用户请求，可选将请求处理掉还是传给下家；在具体处理者中可以访问链中下一个对象，以便请求的转发。

## 2.3 优点

降低耦合度，分离了请求与处理，无须知道是哪个对象处理其请求
简化对象的相互连接，仅保持一个指向后者的引用，而不需保持所有候选接受者的引用
扩展容易，新增具体请求处理者，只需要在客户端重新建链即可，无需破坏原代码

## 2.4 纯与不纯

纯：要么承担全部责任，要么将责任推给下家，不允许出现某一个具体处理者对象在承担了一部分或全部责任后又将责任向下传递的情况。
不纯：允许某个请求被一个具体处理者部分处理后再向下传递，或者一个具体处理者处理完某请求后其后继处理者可以继续处理该请求，而且一个请求可以最终不被任何处理者对象所接收。

一个纯的责任链模式要求一个具体的处理者对象只能在两个行为中选择一个：一是承担责任，而是把责任推给下家。不允许出现某一个具体处理者对象在承担了一部分责任后又 把责任向下传的情况。
在一个纯的责任链模式里面，一个请求必须被某一个处理者对象所接收；在一个不纯的责任链模式里面，一个请求可以最终不被任何接收端对象所接收。
纯的责任链模式的实际例子很难找到，一般看到的例子均是不纯的责任链模式的实现。

## 2.5 demo

demo1:

客户端创建了三个处理者对象，并指定第一个处理者对象的下家是第二个处理者对象，第二个处理者对象的下家是第三个处理者对象。
然后客户端将请求传递给第一个处理者对象。

demo2:

1.用Filter模拟处理Request、Response

2.思路细节技巧：

(1)Filter的doFilter方法改为doFilter(Request,Resopnse,FilterChain),有FilterChain引用，为利用FilterChain调用下一个Filter做准备

(2)FilterChain继承Filter,这样，FilterChain既是FilterChain又是Filter,那么FilterChain就可以调用Filter的方法doFilter(Request,Resopnse,FilterChain)

(3)FilterChain的doFilter(Request,Resopnse,FilterChain)中，有index标记了执行到第几个Filter,当所有Filter执行完后request处理后，就会return,以倒序继续执行response处理

# 3. 策略模式

OOP三个基本特征是:封装、继承、多态。通过继承，我们可以基于差异编程，也就是说，对于一个满足我们大部分需求的类，可以创建它的一个子类并只改变我们不期望的那部分。
但是在实际使用中，继承很容易被过度使用，并且过度使用的代价是比较高的，所以我们减少了继承的使用，使用组合或委托代替。策略模式使用委托的方式。

## 3.1 概念

策略模式定义了一系列的算法，并将每一个算法封装起来，而且使他们可以相互替换，让算法独立于使用它的客户而独立变化。

分析下定义，策略模式定义和封装了一系列的算法，它们是可以相互替换的，也就是说它们具有共性，而它们的共性就体现在策略接口的行为上，
另外为了达到最后一句话的目的，也就是说让算法独立于使用它的客户而独立变化，我们需要让客户端依赖于策略接口。

## 3.2 使用场景

1. 针对同一类型问题的多种处理方式，仅仅是具体行为有差别时；
2. 需要安全地封装多种同一类型的操作时；
3. 出现同一抽象类有多个子类，而又需要使用 if-else 或者 switch-case 来选择具体子类时。

## 3.3 策略模式涉及到的角色

1. 环境(Context)角色：持有一个Strategy的引用。

2. 抽象策略(Strategy)角色：这是一个抽象角色，通常由一个接口或抽象类实现。此角色给出所有的具体策略类所需的接口。

3. 具体策略(ConcreteStrategy)角色：包装了相关的算法或行为。

## 3.4 demo

demo1:简单的使用工厂进行封装策略
demo2:使用注解动态配置策略

# 4. 模板方法模式

模板方法模式使用继承的方式。

## 4.1 概念

定义一个算法的骨架，将骨架中的特定步骤延迟到子类中。模板方法模式使得子类可以不改变算法的结构即可重新定义该算法的某些特定步骤。

## 4.2 demo

模板方法模式展示了经典重用的一种形式，通用算法被放在基类中，通过继承在不同的子类中实现该通用算法。
我们通过定义通用类BubbleSorter，把冒泡排序的算法骨架放在基类，然后实现不同的子类分别对int数组、double数组、List集合进行排序。但这样是有代价的，因为继承是非常强的关系，派生类不可避免地与基类绑定在一起了。但如果我现在需要用快速排序而不是冒泡排序来进行排序，
但快速排序却没有办法重用setArray、getLength、needSwap和swap方法了。不过，策略模式提供了另一种可选的方案。

# 5. 工厂模式

工厂模式（Factory）允许我们只依赖于抽象接口就能创建出具体对象的实例，所以在开发中，如果具体类是高度易变的，那么该模式就非常有用。

## 5.1 简单工厂模式

## 5.2 工厂方法模式

工厂方法模式把简单工厂模式的内部逻辑判断移到了客户端

## 5.3 总结

简单工厂模式和工厂方法模式都封装了对象的创建，它们使得高层策略模块在创建类的实例时无需依赖于这些类的具体实现。但是两种工厂模式之间又有差异：

简单工厂模式：最大的优点在于工厂类包含了必要的判断逻辑，根据客户端的条件动态地实例化相关的类。但这也是它的缺点，当扩展功能的时候，需要修改工厂方法，违反了开放封闭原则
工厂方法模式：符合开放封闭原则，但这带来的代价是扩展的时候要增加相应的工厂类，增加了开发量，而且需要修改客户端代码