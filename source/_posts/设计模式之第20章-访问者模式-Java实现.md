title: 设计模式之第20章-访问者模式(Java实现)
date: 2015-01-31 01:17:35
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---
“嘿，你脸好红啊。”“精神焕发。”“怎么又黄了？”“怕冷，涂的，涂的，蜡。”“身上还有酒味，露馅了吧，原来是喝酒喝的啊。”“嘿嘿，让，让你发现了，今天来几个朋友，然后就小聚一下，小饮，几杯啦。”“小日子过得不错嘛。”“那是自然，要不然，再去喝两杯。”“别介，我还有要事要做呢，鱼哥你别坑我。”“什么，什么要紧事，能比的上，喝酒啊”。“走，陪我，陪我喝两杯去。”（作者已被拉走。）访问者登场。

##访问者模式之自我介绍

累的死俺的杰特们（ladies and gentlemen）,大家好，我地球土生土长的模式，叫做访问者。定义是：Represent an operation to be performed on the elements of an object strctrue.Visitor lets you define a new operation without changing the classes of the elements on which it operates.译过来的意思是：封装一些作用于某种数据结构中的各元素的操作，它可以在不改变数据结构的前提下定义作用于这些元素的新的操作。通用类图是：

![](http://images.cnitblog.com/blog/666211/201501/310014188473429.png)

有关于每个类的定义以及解释由于图中已经做了详细的解释，我也就不再赘述了~

##访问者模式之自我分析

我晓得又该做什么劳什子自我分析，好吧，你们的口味是先听缺点再听优点，我偏不这么来，我要先介绍我的优点：

* 首先呢，额可是很符合传说中的单一职责原则，具体的元素角色负责数据的加载，Visitor为每一个类声明一个Visit操作，两个不同的职责非常明确的分离出来，各自演绎变化。
* 其次呢，我拥有优秀的扩展性。由于职责分开，想要增加对数据的操作是很简单的。
* 最后捏，灵活性非常高。

接下来，我就说一说我的缺点撒：

* 具体元素访问对访问者公布了细节，不符合迪米特法则。
* 然后就是具体元素变更比较困难。
* 最后嘛，我还不小心违背了依赖倒置原则。访问者依赖的是具体元素，而不是抽象元素，这破坏了依赖倒置原则。

恩，优缺点貌似就这么多了，如果你们觉得还有什么，尽可提出。

##访问模式之实现

	public abstract class Element{
	    //业务逻辑
	    public abstract void doSth();
	    //允许来访者
	    public abstract void accept(IVisitor visitor);
	}

抽象元素的定义比较简单，就两个抽象方法，一个是业务逻辑的，一个是接受来访者拜访的抽象方法，接下来就是具体元素的实现类了：

public class COncreteElement2 extends Element{
    //吃大餐
    public void doSth(){
        System.out.println("吃大餐");
    }
    public void accept(IVisitor visitor){
        visitor.visit(this);
    }
}

这是第一个抽象类的具体实现，其实就是聚餐的主题之一，吃撒，接着是聚餐的第二大主题，喝撒，没听到领导经常说的嘛：“大家吃好喝好啊~”：

	public class ConcreteElement1 extends Element{
	    //喝酒
	    public viod doSth(){
	        System.out.println("聚餐是要喝酒的");
	    }
	    public void accept(IVisitor visitor){
	        visitor.visit(this);
	    } 
	}

接下来呢，就是接口访问者了：

	public interface IVisitor{
	    public void visit(ConcreteElement1 el1);
	    public void visit(ConcreteElement2 el2);
	}

怎么样，是不是超级简单呢？其实吧，有几个具体元素类，就有几个访问方法与之对应。然后就是具体访问者的实现：

	public class Visitor implements IVisitor{
	    public void visit(ConcreteElement1 el1){
	        el1.doSth();
	    }
	    public void visit(ConcreteElement2 el2){
	        el2.doSth();
	    }
	}

其实也就是两个元素的访问，没什么的。结构对象就是提供一个访问对象的接口而已了：

	public class ObjectStruture{
	    //对象生成器
	    public static Element createElement(){
	    //掷硬币，决定先吃饭还是先喝酒
	        Random rand = new Random();
	        if(rand.nextInt(2)>1){
	            return new ConcreteElement1();
	        }
	        else {
	            return new ConcreteElement2();
	        }
	    }
	}

好了，至此实现完毕。

##访问者模式之应用场景


* 一个对象结构包含很多类对象，它们有不同的接口，你呢，又想对这些对象实施一些依赖于其具体类的操作的时候。用我吧。
* 当你需要对一个对象结构中的对象进行很多不同的并且不相关的操作，你又不想让这些操作“污染”这些对象类。请用访问者。
* 当你定义对象结构的类很少改变，但经常需要在此结构定义新的操作的时候。来试试访问者。

不早了，该回去洗洗睡了，鱼哥还等着我喝酒呢，那就拜拜了您呐~预知后事如何，且听下回分解内您那~


---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**