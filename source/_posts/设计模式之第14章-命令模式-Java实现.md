title: 设计模式之第14章-命令模式(Java实现)
date: 2015-01-26 23:19:14
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---
“小明，滚出去。”“小明，这个问题怎么做？”（可怜的小明无奈躺枪。小明：老师，我和你有什么仇什么怨，我和你有什么仇什么怨啊到底、、、老师：小明，滚出去。习惯了而已。小明：、、、）对于这种现象，有请命令模式来做一下解说。

##命令模式之自我介绍

知道高内聚么？不知道吧，其实我也不知道。（作者按：某模式开启装逼模式，装逼模式正在开启中、、、开启失败，回滚。）所以让作者大大给我们科普一下（苦命的我啊~），所谓高内聚是软件工程的一个概念，主要是面向对象的设计，说的是类与类的关系，所谓高内聚就是说一个模块内各个元素结合的紧密程度比较高。高内聚就是很好符合了单一职责的设计原则。好了，科普结束，觅食去了。恩恩，鱼哥说的就是我想说的，而我就是一个高内聚的模式。定义是：Encapsulate a request as an object ,thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.译为：将一个请求封装成一个对象，从而让你使用不同的请求把客户端参数化，对请求排队或者记录请求日志，可以提供命令的撤销和恢复功能。通用类图如下：

![](http://images.cnitblog.com/blog/666211/201501/251213065783499.png)

图上这么清楚的解释把我想要说的的都说完了，我就不啰嗦了~

##命令模式之自我分析

　使用我可以达到以下效果：

* 将调用操作的对象与知道如何实现该操作的对象解耦。
* 可以将多个命令装配成一个复合的命令。
* 增加新的命令很容易，无需改变已有的类。
* 类间解耦，调用者角色与接收者之间没有任何依赖关系。
* 可以与我的兄弟职责链模式结合，实现命令族解析任务，与模板方法结合，可以减少命令子类膨胀的问题。

不足：

* 若命令有上千个，就需要有上千个类，类膨胀过于严重，但是可以结合模板方法，减少类的膨胀。

##命令模式之实现

那我们就拿老师与小明的故事来实现一下命令模式，首先自然是接收命令的人的抽象类了：

	public abstract class Receiver{
	    //抽象接受者，定义接受者所需要完成的业务
	    public abstract void out();
	
	}

这里定义了一个抽象类，以及一个抽象方法：出去。

然后是具体的接收者的实现类，可以是小墙、小东、小西等等，这里拿小明举栗子：

	public class XiaoMing extends Receiver{
	    //接受者执行的命令
	    public void out(){
	        System.out.println("小明，滚出去。");
	    }
	}

接下来是command的抽象类：

	public abstract class Command{
	    //每个命令执行一个方法
	    public abstract void execute();
	}

之后是具体的Command的实现类：

	public class ConcreteCommand extends Command{
	    //对相应的Receiver类进行命令处理
	    private Receiver receiver;
	    //构造函数传递接受者
	    public ConcreteCommand(Receiver receiver){
	        this.receiver = receiver;
	    }
	
	    //实现一个命令
	    public void execute(){
	        this.receiver.out();
	    }
	}

最后是Invoker类的代码实现：

	public class Invoker{
	    private Command command;
	    //接收命令
	
	    public void setCommand(Command command){
	        this.command = command;
	    }
	
	    //执行命令
	    public void action(){
	        this.commadn.execute();
	    }
	}

经调用之后，就是我们常见的：小明，滚出去了。额，感觉有哪里不对。

##命令模式之应用场景

在以下的场景中，可以使用Command模式来实现：

* 不同的时刻、排列和执行请求。
* 支持取消操作。
* 支持修改日志。
* 用构建在原语操作上的高层操作构造一个系统。

以上。预知后式如何，且听下回分解。


---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.gitcafe.com)。**
> **博文链接：<http://voidy.gitcafe.com/>**