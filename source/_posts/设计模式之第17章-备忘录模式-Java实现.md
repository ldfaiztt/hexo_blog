title: 设计模式之第17章-备忘录模式(Java实现)
date: 2015-01-29 21:41:07
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---

好男人就是我，我就是曾小贤。最近陈赫和张子萱事件闹得那是一个沸沸扬扬。想想曾经每年都有爱情公寓陪伴的我现如今过年没有了爱情公寓总是感觉缺少点什么。不知道你们可曾记得爱情公寓里的一个经典的桥段~每次关谷和唐悠悠吵架的时候，总是可以进行“存档”，先干其他的事情，而后有时间的时候再继续“读档”，这是多么好的一个技能啊，想想吧，每次吵架，存档后可以做其他事情进行冷静一下，然后读档的时候已经冷静好了，是不是会清醒很多呢，是不是就不会有那么多的误会无法解除了呢？在这个事件中，他们两位用到了接下来出场的“备忘录模式”。好了，接下来有请备忘录~（众人：鱼哥真是越来越啰嗦了啊。吐槽帝啊简直，不，还是一个吃货。）

##备忘录模式之自我介绍

鄙人不才，提供了一个不存在于真实世界的备忘录模式。说的通俗易懂一点就是传说中的“后悔药”。想想吧，当你做错什么事情的时候，只需“读档”到做错事之前就可以再来一遍，那是多少人心中所想，或者当你由于某些原因曾错过某些人某些事的时候，“读档”到这件事之前是多么令人心动。这样的话，有多少没有勇气去做的事情，会在有“后悔药”之后敢于一次又一次的尝试，在游戏中，敢于无数次的开荒、PK的人不正是因为无限的“复活”。好了，不再啰嗦，有关我的定义是：Without violating cncapsulation, capture and externalize an object's internal state so that the object can be restored to this state later.翻译过来就是说：在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可将该对象恢复到原先保存的状态。说的直白一点，就是说将一个一个对象进行备份，提供了一种程序数据的备份方法，它的通用类图是：

![](http://images.cnitblog.com/blog/666211/201501/282048309413100.png)

##备忘录模式之自我分析

分析未动，缺点先行：

* 首先来说，使用备忘录代价会比较高：如果原发器在生成备忘录时，必须拷贝并存储大量的信息，或者客户非常频繁地创建备忘录和恢复原发器状态，可能造成很大的开销。
* 在某些语言中难以保证只有原发器可访问备忘录的状态。
* 维护备忘录的潜在代价会比较高。由于管理器不知道备忘录中有多少个状态，因此存储备忘录时一个原本很小的管理器也可能产生大量的存储。

至于优点，列举如下：

* 保持封装边界。使用备忘录可以避免暴露一些只应由原发器管理却又必须存储在原发器之外的信息。
* 简化了原发器。由于把存储管理的重任交给了Originator，让客户管理它们请求的状态将会简化Originator，并且使得客户工作结束时无需通知原发器。

##备忘录模式之实现

既然都说到了爱情公寓，那么就以爱情公寓的关谷来举个栗子。首先是关谷类，关谷类记录了关谷当时吵架的心情以及关谷在存档之后的变化：

	public class GuanGu{
	    //吵架时关谷的状态
	    private String state = "";
	    //存档后的状态改变
	    public void changeState(){
	        this.state = "心情变得冷静下来";
	    }
	
	    public String getState(){
	        return state;
	    }
	
	    public void setState(String state){
	        this.state = state;
	    }
	
	    //保留备份
	    public Memento createMemento(){
	        return new Memento(this.state);
	    }
	    //恢复备份
	    public void restoreMemento(Memento memento){
	        this.setState(memento.getState());
	    }
	}

接下来就是比较重要的Memento类了，实现起来极其简单，非常纯粹的JavaBean：

	public class Memento{
	    //关谷的心情
	    private String state = "";
	    //通过构造函数传递心情
	    public Memento(String state){
	        this.state = state;
	    }
	
	    public String getState(){
	        return state;
	    }
	
	    public void setState(String state){
	        this.state = state;
	    }
	}

最后就是备忘录的管理员角色，用来对备忘录进行管理、保存和提供备忘录。备忘录的管理员类也很简单：

	public class Caretaker{
	    //备忘录对象
	    private Memento memento;
	    public Memento getMemento(){
	        return memento;
	    }
	    public void setMemento(Memento memento){
	        this.memento = memento;
	    }
	}

至此，备忘录模式就实现了。

##备忘录模式之应用场景

那么问题来了，什么时候才能使用备忘录模式呢：

* 需要保存和恢复数据的相关状态场景。
* 提供一个可回滚的操作，比如说各种编辑器中的Ctrl+Z组合键。
* 需要监控的副本场景中。
* 数据库连接的事务管理就是用的备忘录模式。

以上。预知后式如何，且听下回分解。

---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**
