title: 设计模式之第15章-适配器模式(Java实现)
date: 2015-01-27 21:57:14
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---

“呔，来着何人，报上名来。”“这是谁啊，我怎么没见过”，“就是啊，我也没印象。”“我当然是适配器了，要不然还能是谁。”适配器模式碎碎念：我不就是昨天把你们的烤串都吃完了么，至于这么对我么。（作者按：嘿嘿，让你抢我东西吃，现在你的脸已被我画的连你妈都不认识了，何况他们乎~），“唉唉，别围着他了，我们先看看他耍什么花招。”

##适配器模式之自我介绍

没错，我就是适配器模式，你们可能不是很熟悉，那么说到Adapter你们应该不陌生吧。闲话就不说了，先说下我的定义吧：Convert the interface of a class into another interface clients expect.Adapter lets classes work togther that couldn't otherwise because of incompatible interfaces.意思就是说:将一个类的接口变成客户端所期待的另一种接口，从而使原本因接口不匹配而无法在一起工作的两个类能够在一起工作。

我的通用类图如下：

![](http://images.cnitblog.com/blog/666211/201501/261917319722082.jpg)

类图中的各个图的解释我就不多说了。其实在生活中我还是无处不在的，比如说电源适配器，使电源电压变化，说白了，我就是把一个接口或者类转换成其他的接口或类。

##适配器模式之自我分析

首先分析一下缺点：

 使得重定义Adaptee的行为比较困难，若想重定义需要生成Adaptee的子类，然后用Adapter引用其子类。

优点如下：

* Adapter可以重定义Adaptee的部分行为。
* 允许Adapter与多个Adaptee一起工作。
* 可以让两个没有任何关系的类在一起运行。
* 增加了子类的透明性。
* 提高了类的复用度。
* 灵活性比较好。想用就用，想删就删。

##适配器模式之实现

No code，no truth。说的再好，也不如代码管用，所以我就实现一个通用代码。

首先是一个目标接口的代码：

	public interface Target{
	    //目标角色自己的方法
	    public void request();
	}

然后是目标角色的实现方法：

	public class ConcreteTarget implements Target{
	    public void request(){
	        System.out.println("Nothing is important.");
	    }
	}

接下来是Adaptee的实现类，代码也会很少：

	public class Adaptee{
	    //原有业务逻辑
	    public void doSth(){
	        System.out.println("I want to eat delicious snacks");
	    }
	}

上面就是需要适配的类，在这里的方法什么的就不再多写了。接下来就是重中之重的适配器类的实现了：

	public class Adapter extends Adaptee implements Target{
	    public void request(){
	        super.doSth();
	    }
	}

它继承自Adaptee，同时是Target的接口实现，这样一来就将Adaptee和Target联系起来了，最后是测试用的类：

	public class Client{
	    public static void main(String[] args) {
	        //原有业务处理
	        Target target = new ConcreteTarget();
	        target.request();
	        //现在增加了适配器角色之后的业务逻辑
	        Target target2 = new Adapter();
	        target2.request();
	    }
	}

至此，我的实现就此完成了。

##适配器模式之应用场景

怎么样，相信我就是适配器了吧，你们竟然装作不认识我的样子，作者道：赶紧接着讲啊，讲完再说，大家都等着呢。好吧，鱼哥催了，接下来就是应用场景了：

* 你若想使用一个已经存在的类，而它的接口又不符合你的要求时，适配器模式走起。
* 若你想创建一个可以复用的类，该类可以与其他不相关的类或不可预见的类协同工作时，适配器模式等着你。
* 当你想使用一些已经存在的子类，但是不可能对每一个都进行子类化时，适配器模式欢迎你。

好了，That's all。荆轲刺秦王，设计模式手中藏。（此时抽象工厂妹纸将镜子拿出递给适配器模式，只听一声尖叫传来：有鬼啊~~~）

---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**