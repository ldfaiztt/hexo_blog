title: 设计模式之第16章-代理模式(Java实现)
date: 2015-01-28 22:13:33
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---

“现在朋友圈真是太让人蛋疼了啊。”“怎么说？”“一堆代理，各种卖东西的，看着好烦人。”“哎，删了呗。”“都是朋友，哪里好意思删啊。”“这倒也是、、、哎，迫于生计，没办法咯。还好我不玩。”“对了，你不就是代理的鼻祖么，身为代理模式，你作何感想。”“以代理之道还治代理之身啊。”

##代理模式之自我介绍

最近出场率超级高，哦不，一直以来出场率都挺高的说的大名鼎鼎的模式，就是我-代理模式是也。有关我的定义如下：Provide a surrogate or placeholder for another object to control access to it.翻译过来就是说:为其他的对象提供一种代理以控制对这个对象的访问。通用类图如下：

![](http://images.cnitblog.com/blog/666211/201501/272027336759678.jpg)

代理模式又被称作委托模式，它是一项基本设计技巧。许多其他的模式，如策略、访问者模式本质上是在更特殊场合采用了代理模式，在日常应用中，代理模式也可以提供非常好的访问控制。在Struts2中的Form元素映射中就采用了动态代理模式。

##代理模式之自我分析

使用我的好处如下：

* 职责清晰。真实的角色就是实现实际的业务逻辑，不用关心其他非本职责的事务。
* 高扩展性，具体的主题角色可以随时变化，只要其实现了接口，不用管它如何变化，都可以使用接口来使用方法。

##代理模式之实现

具体的情景我们就以朋友圈卖吃的来说吧，恩，就卖辣条好了。首先是一个公司的接口类，实现每个公司的简单功能：

	public interface ICompany{
	    //打上品牌
	    public void brand(String brand);
	    //出售
	    public void sell();
	}

	public class Company implements ICompany{
	    private String name = "";
	    //构造函数获取名字
	    public Company(String name){
	        this.name = name;
	    }
	
	    //注册商标
	    public void brand(String name){
	        System.out.println("公司的名字是："+name);
	    }
	
	    //出售产品
	    public void sell(){
	        System.out.println("最后三天清仓处理，买一箱送三箱。。。");
	    }
	}

最后是最重要的代理实现了：

	public class Proxy implements ICompany{
	    private ICompany cp = null;
	    //通过构造函数传递代理的公司
	    public Proxy(ICompany cp){
	        this.cp = cp;
	    }
	    //代理在朋友圈打品牌
	    public viod brand(String name){
	        this.cp.brand(name);
	    }
	
	    //代理在卖东西
	    public void sell(){
	        this.cp.sell();
	    }
	}

好了，基本上实现就是这样了，最后友情送上场景类一个：

	public class Client{
	    public static void main(String[] args) {
	        //卫龙辣条，我的最爱
	        ICompany cp = new Company("卫龙");
	        //定义一个代理，来卖辣条
	        ICompany proxy = new Proxy(cp);
	        //代理打品牌
	        proxy.brand("卫龙");
	        //代理出售
	        proxy.sell();
	    }
	}

至此，实现部分完毕。

##代理模式之应用场景

场景真是数也数不清了，找人LOL、魔兽各种代练，找人代理卖房买房什么的老多了。下面说几种常见的代理情况：

* 远程代理：为一个对象在不同地址空间提供局部的代理代表。
* 虚代理：根据需要创建开销很大的对象。
* 保护代理：控制对原始对象的访问。
* 智能指引：取代了简单的指针，它在访问对象时执行一些附加操作。

荆轲刺秦王，设计模式心中藏，好了，今天到此结束。

---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**