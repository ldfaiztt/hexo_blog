title: 基于jersey和Apache Tomcat构建Restful Web服务（一）
date: 2015-03-21 13:56:04
tags: [Jersey,Restful]
categories: Jersey
description: 基于jersey和Apache Tomcat构建Restful Web服务（一）
---

现如今，RESTful架构已然成为了最流行的一种互联网软件架构，它结构清晰、符合标准、易于理解、扩展方便，所以得到越来越多网站的采用。那么问题来了，它是什么呢？

##起源

REST（Representational state transfer）在 2000 年由 Roy Fielding 在博士论文中提出，他是 HTTP 规范 1.0 和 1.1 版的首席作者之一。

REST 中最重要的概念是资源（resources），使用全球 ID（通常使用 URI）标识。客户端应用程序使用 HTTP 方法（GET/ POST/ PUT/ DELETE）操作资源或资源集。RESTful Web 服务是使用 HTTP 和 REST 原理实现的 Web 服务。通常，RESTful Web 服务应该定义以下方面：

* Web 服务的基/根 URI，比如 http://host/<appcontext>/resources。
* 支持 MIME 类型的响应数据，包括 JSON/XML/ATOM 等等。
* 服务支持的操作集合（例如 POST、GET、PUT 或 DELETE）。

表 1 演示了典型 RESTful Web 服务中使用的资源 URI 和 HTTP 方法。（参考资料 提供了有关 RESTful Web 服务的更多介绍和设计考虑事项。）

表 1. RESTful Web 服务示例:

方法/资源 | 资源集合， URI 如：
http://host/<appctx>/resources | 成员资源，URI 如：
http://host/<appctx>/resources/1234
-----|------|----
GET    | 列出资源集合的所有成员。   | 检索标识为 1234 的资源的表示形式。
PUT    | 使用一个集合更新（替换）另一个集合。    | 更新标记为 1234 的数字资源。
POST    | 在集合中创建数字资源，其 ID 是自动分配的。   | 在下面创建一个子资源。
DELETE    | 删除整个资源集合。   | 删除标记为 1234 的数字资源。

那么又如何来构建Restful web服务呢？本文介绍一个简单易学的构建Restful web服务的方法，即是使用Jersey构建Restful web服务。

##JSR 311 (JAX-RS) 和 Jersey　

JSR 311 或 JAX-RS（用于 RESTful Web Services 的 Java API）的提议开始于 2007 年，1.0 版本到 2008 年 10 月定稿。目前，JSR 311 版本 1.1 还处于草案阶段。该 JSR 的目的是提供一组 API 以简化 REST 样式的 Web 服务的开发。

在 JAX-RS 规范之前，已经有 Restlet 和 RestEasy 之类的框架，可以帮助您实现 RESTful Web 服务，但是它们不够直观。Jersey 是 JAX-RS 的参考实现，它包含三个主要部分。


* 核心服务器（Core Server）：通过提供 JSR 311 中标准化的注释和API  
 标准化，您可以用直观的方式开发 RESTful Web 服务。
* 核心客户端（Core Client）：Jersey 客户端 API 帮助您与 REST 服务轻松通信。
* 集成（Integration）：Jersey 还提供可以轻松集成 Spring、Guice、Apache Abdera 的库。

　　有没有想立即动手写一个的冲动？那么就接着往下看吧。

##Hello World：第一个Jersey Web项目

**开发环境：**

* IDE：Eclipse
* Web 容器：Apache Tomcat 7.0（Jetty 和其他也可以）
* Jersey 库：Jersey 2.16bundle，包含所有必需的库

**开发 REST 服务**

首先，我们这个项目不是基于Maven Archetype的，如果想学习基于Maven Archetype的请移步至[官网:](https://jersey.java.net/documentation/latest/getting-started.html#new-from-archetype。)


下面介绍普通的RESTful Web服务构造方法：

**第一步：**

首先，打开Eclipse，创建一个Dynamic Web Project，项目名随意，此处起名为hello。然后点击Next，再次点击Next，到达如下界面：

![](http://images.cnitblog.com/blog2015/666211/201503/291306114278128.png)

勾选箭头所指地方，生成web.xml文件。

如果你直接点击了创建，没关系，还是有补救方法的，右键项目，点击箭头所指项：

![](http://images.cnitblog.com/blog2015/666211/201503/291306545057789.png)

即可生成文件web.xml文件。

**第二步：**

创建完成之后，将下载的Jersey bundle包解压，然后将里面的东西都复制到WEB-INF下的lib文件夹中即可。现在一个Jersey就搭建好了。

**第三步：**

现在，您已经设置好了开发第一个 REST 服务的环境，该服务对客户端发出 “Hello”。

要做到这一点，您需要将所有的 REST 请求发送到 Jersey 容器 —— 在应用程序的 web.xml 文件中定义 servlet 调度程序（参见清单 1）。除了声明 Jersey servlet 外，它还定义一个初始化参数，指示包含资源的 Java 包。

**清单 1. 在 web.xml 文件中定义 Jersey servlet 调度程度**

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.	com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.	com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
	  <display-name>Hello</display-name>
	  <welcome-file-list>
	    <welcome-file>index.html</welcome-file>
	    <welcome-file>index.htm</welcome-file>
	    <welcome-file>index.jsp</welcome-file>
	    <welcome-file>default.html</welcome-file>
	    <welcome-file>default.htm</welcome-file>
	    <welcome-file>default.jsp</welcome-file>
	  </welcome-file-list>
	     <servlet>  
	    <servlet-name>Jersey REST Service</servlet-name>  
	    <servlet-class>org.glassfish.jersey.servlet.ServletContainer</servlet-class> 
	    <init-param>  
	      <param-name>jersey.config.server.provider.packages</param-name> 
	      <param-value>com.test.restwebservice.rest</param-value>  
	    </init-param>  
	    <load-on-startup>1</load-on-startup>  
	  </servlet>  
	  <servlet-mapping>  
	    <servlet-name>Jersey REST Service</servlet-name>  
	    <url-pattern>/v1/*</url-pattern>  
	  </servlet-mapping> 
	</web-app>

现在您将编写一个名为 HelloResource 的资源，它接受 HTTP GET 并响应 “Hello Jersey”。

**清单 2. sample.hello.resources 包中的 HelloResource**

	@Path("/hello")
	public class HelloResource {
	    @GET
	    @Produces(MediaType.TEXT_PLAIN)
	    public String sayHello() {
	        return "Hello Jersey";
	    }
	}

该代码中有几个地方需要强调：

    * 资源类（Resource Class）：注意，资源类是一个简单的 Java 对象 (POJO)，可以实现任何接口。这增加了许多好处，比如可重用性和简单。
    * 注释（Annotation）：在 javax.ws.rs.* 中定义，是 JAX-RS (JSR 311) 规范的一部分。
    * @Path：定义资源基 URI。由上下文根和主机名组成，资源标识符类似于 http://localhost:8080/Jersey/rest/hello。
    * @GET：这意味着以下方法可以响应 HTTP GET 方法。
    * @Produces：以纯文本方式定义响应内容 MIME 类型。

##测试 Hello 应用程序

要测试应用程序，可以打开您的浏览器并输入 URL http://<host>:<port>/<appctx>/rest/hello。您将看到响应 “Hello Jersey”。这非常简单，使用注释处理请求、响应和方法。


---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**