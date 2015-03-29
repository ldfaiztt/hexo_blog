title: 基于jersey和Apache Tomcat构建Restful Web服务（二）
date: 2015-03-29 17:22:25
tags: [Jersey,Restful]
categories: Jersey
description: 本次主要介绍如何在路径中代入参数，以及如何使返回值是Json格式等等
---

上篇博客介绍了REST以及Jersey并使用其搭建了一个简单的“Hello World”，那么本次呢，再来点有趣的东西，当然也是很简单了，仅仅是在路径中包含参数而已了。接下来开始动手实践吧。

##在路径中包含参数

接下来就在上次的基础上进行改动即可，或者是再添加一个方法，随意了，这个方法主要就是在路径中加入输入的参数，并且根据参数的不同，它的返回值也不同，返回值为“Hello”+你输入的参数。这里用到了“PathParam”这个参数，具体代码如下：

	@GET
	        @Path("/{username}")
	        @Produces(MediaType.TEXT_PLAIN)
	        public String getHello(@PathParam("username") String userName) {
	            System.out.println(userName);
	            return "Hello "+userName;
	        }

这次使用Firefox提供的RESTClient来测试，输入路径后结果如下：

![](http://images.cnitblog.com/blog2015/666211/201503/291704401145414.png)

怎么样，很不错吧。但是貌似字符串用的不多欸，有木有，一般都是返回的Json格式的数据。那么如何让它返回Json格式的数据呢？

##使用POJO和List来使结果返回Json格式　

上面仅仅是简单的字符串返回值，有够无聊的，下面再来点更有趣的：

首先，你需要在WEB-INF的lib下加入一个用于处理Json的jar包，然后你需要创建一个People的POJO，我的POJO如下：

	@XmlRootElement
	public class People {
	    
	    private String name;
	    private String password;
	    
	    public String getName() {
	        return name;
	    }
	    public void setName(String name) {
	        this.name = name;
	    }
	    public String getPassword() {
	        return password;
	    }
	    public void setPassword(String password) {
	        this.password = password;
	    }

此方法仅仅包含简单的set/get方法。当然了，不要忘了对此类加Jersey的注解：@XmlRootElement。

接下来在你的类中加入方法：

	@GET
	        @Produces("application/json")
	        @Consumes("application/json")
	        public List<People> getPass(){
	            
	            List<People> list = new ArrayList<People>();
	            People people = new People();
	            for (int i = 0; i < 6; i++) {
	                
	                people.setName("Li"+i);
	                people.setPassword("123456"+i);
	                list.add(people);
	            }
	            return list;
	        }

好了，接下来就是测试了，和前面一样，输入路径，将显示如下结果：

![](http://images.cnitblog.com/blog2015/666211/201503/291715448491893.png)

其他的诸如POST、PUT、DELETE方法与此类似，不再一一示范。

欲想了解更多，请参考以下资料：<https://jersey.java.net/documentation/latest/getting-started.html#new-from-archetype>



---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**