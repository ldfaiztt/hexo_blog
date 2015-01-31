title: 设计模式之第21章-状态模式(Java实现)
date: 2015-01-31 14:29:54
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---
“what are you 干啥了？怎么这么萎靡不振？”“昨晚又是补新番，又是补小笼包，睡得有点晚啊。话说杨过的那个雕兄真是太好了，每天给找蛇胆，又陪练武功的，想不无敌都难啊，还有那个blablabla”（作者已被拖走）。咳咳，今天那个状态哥哥马不停蹄的赶过来，下面闪亮登场。

##状态模式之自我介绍

今天不在状态，可能是由于宇宙差的原因，好了，先说下定义：Allow an object to alter its behavior when its internal state changes. The object will appear to change its class.也就是说当一个对象内在改变的时候允许其改变行为，这个对象看起来像改变了其类。本人的核心就是“封装”，没错，不是传说中的“银鳞胸甲”的蓝装。状态的变更会引起行为的变更，从外部看来就像这个对象对应的类发生了改变一般，哥们的通用类图如下：

![](http://images.cnitblog.com/blog/666211/201501/311052440971032.png)

鱼哥不在我就多扯一点：Context将与状态相关的请求委托给当前的ConcreteState对象处理，然后呢，Context可以将自身作为一个参数传递给处理该请求的状态对象，使得状态对象在必要时可以访问Context，Context是客户使用的主要接口，客户可用状态对象来配置一个Context，一旦一个Context配置完毕，它的客户不再需要直接与状态对象打交道。Context或者ConcreteState子类都可以决定哪个状态是另外哪一个的后继，以及是在何种条件下进行状态转换的。

##状态模式之自我分析

首先介绍下如下的优点：

* 将与特定状态相关的行为局部化，并且将补不同状态的行为分割开来很好的体现了开放原则以及单一职责原则。增加状态以及修改状态变得简单易行。
* 避免了程序的复杂性，提高了系统的可维护性。使得结构清晰。
* State对象可被共享。

金无足赤，人无完人，我自然也不能例外了，缺点其实就一个：

* 状态过多的话，就会使得子类膨胀了。

##状态模式之实现

今天我们就以作者大大昨晚的活动来举栗子说明如何实现，首先我们实现下Context的类，这个是很重要的，代码如下：

	public class Context{
	    //定义状态
	    public final static State STATE1 = new ConcreteState1();
	    public final static State STATE2 = new ConcreteState2();
	
	    //当前状态
	    private State CurrentState;
	
	
	    //get方法获取当前状态
	    public State getCurrentState(){
	        return CurrentState;
	    }
	    //set方法设置当前状态
	    public State setCurrentState(State currentState){
	        this.CurrentState = currentState;
	
	    //切换状态
	        this.CurrentState.setContext(this);    
	    }
	
	    //行为委托
	    public void handle1(){
	        this.CurrentState.handle1();
	    }
	
	    public void handle2(){
	        this.CurrentState.handle2();
	    }
	
	}

恩，这个Context类定义了客户端需要的接口，并且还负责状态的切换，也就是工作都是靠他来完成了，接下来就是状态的抽象类了，定义了一个让子类访问的接口，以及抽象行为：

	public abstract class State{
	    //定义Context角色，提供子类访问
	    protected Context context;
	    //设置Context角色
	    public void setContext(Context context){
	        this.context = context;
	    }
	
	    //行为1
	    public abstract void handle1();
	    //行为2
	    public abstract void handle2();
	}

然后是第一个状态的具体实现的代码，继承自State抽象类：

	public class ConcreteState1 extends State{
	    @Override
	    public void handle1{
	        System.out.println("看小笼包");
	    }
	
	    @Override
	    public void handle2{
	        //设置当前状态为state2
	        super.context.setCurrentState(Context.STATE2);
	        //过渡到state2，由Context实现
	        super.context.handle2();
	    }
	
	}

最后是第二个状态具体实现类，是看新番了：

	public class ConcreteState1 extends State{
	
	
	    @Override
	    public void handle1{
	        //设置当前状态为state1
	        super.context.setCurrentState(Context.STATE1);
	        //过渡到state2，由Context实现        
	        super.context.handle1();
	    }
	
	    @Override
	    public void handle2{
	        System.out.println("看新番");
	    }
	
	
	    
	}

好吧，作者就在这两个状态中无限切换来着，然后，然后就无法自拔，然后就精神萎靡，再然后就没有然后了~

##状态模式之应用场景

要应用场景有二，欸看我干嘛。我不二，是场景二，错了，场景也不二，是场景有两个：

* 当一个对象的行为取决于它的状态，并且它必须在必须在运行时刻根据状态改变它的行为。
* 一个操作中有很多的分支语句以及条件语句时，并且这些分支/条件语句依赖于该对象的状态，也就轮到我登场了。

以上。（后话：据说因为状态模式黑作者，被作者关小黑屋一周。）
	



---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**