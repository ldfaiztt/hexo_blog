title: 设计模式之第18章-观察者模式(Java实现)
date: 2015-01-30 09:28:37
tags: [Java,Design Patterns]
categories: [Java,Design Patterns]
---
话说曾小贤，也就是陈赫这些天有些火，那么这些明星最怕的，同样最喜欢的是什么呢？没错，就是狗仔队。英文的名字比较有意思，是paparazzo，这一说法据说来自意大利电影《滴露牡丹开》中一个专门偷拍明星照片的一个摄影师的名字，“Paparazzo”，中文译为帕帕拉齐，俗语就是狗仔队。这些明星因狗仔队而荣，获得曝光率，也因狗仔队而损，被曝光负面新闻，不管怎么说，总之是“火起来了”，让明星们又爱又恨。（众人：鱼哥，你扯远了）。咳咳，这个狗仔队其实嘛，也就是所谓进行监视观察活动，接下来就让观察者来给我们讲讲观察者与设计模式不得不说的那些个事儿。

##观察者模式之自我介绍

额就四（我就是）观察者，也被称作依赖或者发布订阅，是在项目中经常用到的一种模式。定义如下：Define a one-to-many dependency between objects so that when one objects changes state, all its dependents are notified and updated automatically.翻译过来就是说：定义对象间一种一对多的依赖关系，使得每当一个对象改变状态，则所有依赖于它的对象都会得到通知并被自动更新。观察者的通用类图如下：

![](http://images.cnitblog.com/blog/666211/201501/291342587843071.png)

##观察者模式之自我分析

首先来说下好处：

* 支持广播通信。
* 目标和观察者之间抽象耦合。

接着是缺点部分：

* 因为一个观察者并不知道其它观察者的存在，它可能对改变目标的最终代价一无所知。
* 然后又是效率问题了，开发效率以及运行效率方面都有可能存在问题，一个被观察者，多个观察者，开发调试比较复杂，而且一个观察者卡壳，其它观察者也会被影响。
* 另外多级触发的效率也需要考虑到。

##观察者模式之实现

至于实现那么就以“贱人曽”来举个栗子吧。就拿曾小贤来举栗子（贤哥，我不是有意的，不要怪我撒，我知道你是LOLer，打得还不错），首先是被观察者Observable，就是名人，比如说曽大大，习大大，Linus大大等等：

	public interface Obeservable{
	    //增加观察者
	    public void addObserver(Observer observer);
	    //删除观察者
	    public void deleteObserver(Observer observer);
	    //发生改变通知观察者
	    public void notifyObservers(String context);
	}

这个是通用的观察者接口，所有的观察者都可以实现这个接口，接下来是小贤的接口：

	public class ZengXiaoXian implements IZengXiaoXian,Obserable{
	    //定义动态数组存放不同媒体的狗仔队
	    private ArrayList<Observer> observerList = new ArrayList<>();
	    //增加观察者
	    public void addObserver(Observer observer){
	        this.observerList.add(observer);
	    }
	    //删除观察者
	    public void deleteObserver(Observer observer){
	        this.observerList.remove(observer);
	    }
	    //发生改变通知观察者
	    public void notifyObservers(String context){
	        for (Observer observer : observerList ) {
	            observer.update(context);
	        }
	    }
	    //吃饭
	    public void eat(){
	        System.out.println("曾小贤要吃饭了");
	        this.notifyObservers("曾小贤要吃饭了");
	    }
	    //睡觉
	    public void sleep(){
	        System.out.println("曾小贤要睡觉了");
	        this.notifyObservers("曾小贤要睡觉了");
	    }
	    //玩
	    public void play(){
	        System.out.println("曾小贤要玩耍了");
	        this.notifyObservers("曾小贤要玩耍了");
	    }
	}

被观察者已经实现了，然后是观察者的实现，首先依然是观察者的接口：

	public interface Observer{
	    //发现被观察的人有动静，就开始准备写稿子，整头条什么的
	    public void update(String context);
	}

然后就是狗仔队的实现了，狗仔队一号出动：

	public class Paparazzo1 implements Observer{
	    //狗仔队一号一旦发现有什么爆料，就告诉老板
	    public void update(String str){
	        System.out.println("观察到曾小贤和张子萱拥吻，开始汇报");
	        this.reportToXX(str);
	    }
	    //汇报给XX媒体
	    private void reportToXX(String context){
	        System.out.println("老大，我看到"+context);
	    }
	}

如果你还想要其它狗仔队请自行实现Paparazzo2、3等等等等，那么我们用一个场景类来实现当时的现场情况：

	public class Client{
	    public static void main(String[] args) {
	        //出来一个狗仔队
	        Observer paparazzo = new Paparazzo();
	
	        //曾小贤出场
	        ZengXiaoXian zengxiaoxian = new ZengXiaoXian();
	        zengxiaoxian.addObserver(paparazzo);
	
	        //看看曾小贤在干嘛
	        zengxiaoxian.eat();
	        zengxiaoxian.play();
	        zengxiaoxian.sleep();
	    }
	}

　好了，基本实现就是这样子了。

##观察者模式之应用场景

当你遇到以下任意一种情况，可以考虑使用额来实现：

* 当一个抽象模型有两个方面，其中一个依赖于另一个方面。
* 当对一个对象的改变需要同时改变其它对象，而不知道具体有多少对象有待改变。
* 当一个对象必须通知其它对象，而它又不能假定其它对象是谁。

这个时候就轮到我登场了，狗仔队就是额，额就四狗仔队，噢耶。“动二动二，我是动幺，这里有一名疑似精神病院跑出来的患者，赶紧给予抓捕带回”，“鱼哥，救额，额不是精神病，你快告诉他们。”（作者按：少一个了，这样以后就少一个人和我抢零食了。默默离开）。金坷垃刺秦王，设计模式任我闯。



---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**