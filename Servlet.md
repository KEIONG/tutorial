# servlet详解

##  servlet是什么

Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。

使用 Servlet，您可以收集来自网页表单的用户输入，呈现来自数据库或者其他源的记录，还可以动态创建网页。

Java Servlet 通常情况下与使用 CGI（Common Gateway Interface，公共网关接口）实现的程序可以达到异曲同工的效果。但是相比于 CGI，Servlet 有以下几点优势：

- 性能明显更好。
- Servlet 在 Web 服务器的地址空间内执行。这样它就没有必要再创建一个单独的进程来处理每个客户端请求。
- Servlet 是独立于平台的，因为它们是用 Java 编写的。
- 服务器上的 Java 安全管理器执行了一系列限制，以保护服务器计算机上的资源。因此，Servlet 是可信的。
- Java 类库的全部功能对 Servlet 来说都是可用的。它可以通过 **sockets** 和 RMI 机制与 applets、数据库或其他软件进行交互。

### servlet架构

![Servlet 架构](https://www.runoob.com/wp-content/uploads/2014/07/servlet-arch.jpg)

### servlet的主要任务

Servlet 执行以下主要任务：

- 读取客户端（浏览器）发送的显式的数据。这包括网页上的 HTML 表单，或者也可以是来自 applet 或自定义的 HTTP 客户端程序的表单。
- **读取客户端（浏览器）发送的隐式的 HTTP 请求数据。这包括 cookies、媒体类型和浏览器能理解的压缩格式等等。**
- 处理数据并生成结果。这个过程可能需要访问数据库，执行 RMI 或 CORBA 调用，调用 Web 服务，或者直接计算得出对应的响应。
- 发送显式的数据（即文档）到客户端（浏览器）。该文档的格式可以是多种多样的，包括文本文件（HTML 或 XML）、二进制文件（GIF 图像）、Excel 等。
- **发送隐式的 HTTP 响应到客户端（浏览器）。这包括告诉浏览器或其他客户端被返回的文档类型（例如 HTML），设置 cookies 和缓存参数，以及其他类似的任务。**

## servlet的生命周期

Servlet 生命周期可被定义为从创建直到毁灭的整个过程。以下是 Servlet 遵循的过程：

- Servlet 初始化后调用 **init ()** 方法。
- Servlet 调用 **service()** 方法来处理客户端的请求。
- Servlet 销毁前调用 **destroy()** 方法。
- 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。

### init() 方法

init 方法被设计成只调用一次。它在第一次创建 Servlet 时被调用，在后续每次用户请求时不再调用。因此，它是用于一次性初始化，就像 Applet 的 init 方法一样。

Servlet 创建于用户第一次调用对应于该 Servlet 的 URL 时，但是您也可以指定 Servlet 在服务器第一次启动时被加载。

当用户调用一个 Servlet 时，就会创建一个 Servlet 实例，每一个用户请求都会产生一个新的线程，适当的时候移交给 doGet 或 doPost 方法。init() 方法简单地创建或加载一些数据，这些数据将被用于 Servlet 的整个生命周期。

init 方法的定义如下：

```
public void init() throws ServletException {
  // 初始化代码...
}
```

### service() 方法

service() 方法是执行实际任务的主要方法。Servlet 容器（即 Web 服务器）调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端。

每次服务器接收到一个 Servlet 请求时，服务器会产生一个新的线程并调用服务。service() 方法检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut，doDelete 等方法。

下面是该方法的特征：

```
public void service(ServletRequest request, 
                    ServletResponse response) 
      throws ServletException, IOException{
}
```

service() 方法由容器调用，service 方法在适当的时候调用 doGet、doPost、doPut、doDelete 等方法。所以，您不用对 service() 方法做任何动作，您只需要根据来自客户端的请求类型来重写 doGet() 或 doPost() 即可。

doGet() 和 doPost() 方法是每次服务请求中最常用的方法。下面是这两种方法的特征。

### doGet() 方法

GET 请求来自于一个 URL 的正常请求，或者来自于一个未指定 METHOD 的 HTML 表单，它由 doGet() 方法处理。

```
public void doGet(HttpServletRequest request,
                  HttpServletResponse response)
    throws ServletException, IOException {
    // Servlet 代码
}
```

### doPost() 方法

POST 请求来自于一个特别指定了 METHOD 为 POST 的 HTML 表单，它由 doPost() 方法处理。

```
public void doPost(HttpServletRequest request,
                   HttpServletResponse response)
    throws ServletException, IOException {
    // Servlet 代码
}
```

### destroy() 方法

destroy() 方法只会被调用一次，在 Servlet 生命周期结束时被调用。**destroy() 方法可以让您的 Servlet 关闭数据库连接、停止后台线程、把 Cookie 列表或点击计数器写入到磁盘，并执行其他类似的清理活动。**

在调用 destroy() 方法之后，servlet 对象被标记为垃圾回收。destroy 方法定义如下所示：

```
  public void destroy() {
    // 终止化代码...
  }
```

### 架构图

下图显示了一个典型的 Servlet 生命周期方案。

- 第一个到达服务器的 HTTP 请求被委派到 Servlet 容器。
- Servlet 容器在调用 service() 方法之前加载 Servlet。
- 然后 Servlet 容器处理由多个线程产生的多个请求，每个线程执行一个单一的 Servlet 实例的 service() 方法。

![Servlet 生命周期](https://www.runoob.com/wp-content/uploads/2014/07/Servlet-LifeCycle.jpg)

## servlet表单数据

很多情况下，需要传递一些信息，从浏览器到 Web 服务器，最终到后台程序。浏览器使用两种方法可将这些信息传递到 Web 服务器，分别为 GET 方法和 POST 方法。

###  GET 方法

GET 方法向页面请求发送已编码的用户信息。页面和已编码的信息中间用 ? 字符分隔，如下所示：

```
http://www.test.com/hello?key1=value1&key2=value2
```

GET 方法是默认的从浏览器向 Web 服务器传递信息的方法，它会产生一个很长的字符串，出现在浏览器的地址栏中。如果您要向服务器传递的是密码或其他的敏感信息，请不要使用 GET 方法。GET 方法有大小限制：请求字符串中最多只能有 1024 个字符。

这些信息使用 QUERY_STRING 头传递，并可以通过 QUERY_STRING 环境变量访问，Servlet 使用 **doGet()** 方法处理这种类型的请求。

### POST 方法

另一个向后台程序传递信息的比较可靠的方法是 POST 方法。POST 方法打包信息的方式与 GET 方法基本相同，但是 POST 方法不是把信息作为 URL 中 ? 字符后的文本字符串进行发送，**而是把这些信息作为一个单独的消息。消息以标准输出的形式传到后台程序，**您可以解析和使用这些标准输出。Servlet 使用 doPost() 方法处理这种类型的请求。

### 使用 Servlet 读取表单数据

Servlet 处理表单数据，这些数据会根据不同的情况使用不同的方法自动解析：

- **getParameter()：**您可以调用 request.getParameter() 方法来获取表单参数的值。
- **getParameterValues()：**如果参数出现一次以上，则调用该方法，并返回多个值，例如复选框。
- **getParameterNames()：**如果您想要得到当前请求中的所有参数的完整列表，则调用该方法。

## Servlet常用对象和方法

### ServletConfig

- String getServletName() -- 获取当前Servlet在web.xml中配置的名字
- String getInitParameter(String name) -- 获取当前Servlet指定名称的初始化参数的值
- Enumeration getInitParameterNames() -- 获取当前Servlet所有初始化参数的名字组成的枚举
- ServletContext getServletContext() -- 获取代表当前web应用的ServletContext对象

在Servlet的配置文件中，可以使用一个或多个<init-param>标签为servlet配置一些初始化参数。

当servlet配置了初始化参数后，web容器在创建servlet实例对象时，会自动将这些初始化参数封装到ServletConfig对象中，并在调用servlet的init方法时，将ServletConfig对象传递给servlet。进而，程序员通过ServletConfig对象就可以得到当前servlet的初始化参数信息。这样做的好处是：如果将数据库信息、编码方式等配置信息放在web.xml中，如果以后数据库的用户名、密码改变了，则直接很方便地修改web.xml就行了，避免了直接修改源代码的麻烦。

~~~ xml
<servlet>
        <servlet-name>ServletConfigTest</servlet-name>
        <servlet-class>com.vae.servlet.ServletConfigTest</servlet-class>
        <init-param>
            <param-name>name1</param-name>
            <param-value>value1</param-value>
        </init-param>
        <init-param>
            <param-name>encode</param-name>
            <param-value>utf-8</param-value>
        </init-param>
</servlet>
然后在代码中获取上面的两个参数。代码实现如下：
~~~



~~~ java


package com.vae.servlet;

import java.io.IOException;
import java.util.Enumeration;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ServletConfigTest extends HttpServlet {

    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        ServletConfig config = this.getServletConfig();  //拿到init方法中的ServletConfig对象

        // --获取当前Servlet 在web.xml中配置的名称（用的不多）
         String sName = config.getServletName();
         System.out.println("当前Servlet 在web.xml中配置的名称:"+sName);
         
        // --获取当前Servlet中配置的初始化参数（只能获取一个）经常用到
        // String value = config.getInitParameter("name2");
        // System.out.println(value);

        // --获取当前Servlet中配置的初始化参数（全部获取）经常用到
         Enumeration enumration = config.getInitParameterNames();
         while(enumration.hasMoreElements()){
         String name = (String) enumration.nextElement();
         String value = config.getInitParameter(name);
         System.out.println(name+":"+value);
         }
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        doGet(request, response);
    }
}
~~~

## HttpServletRequest

HttpServletRequest 对象：主要作用是用来接收客户端发送过来的请求信息，

例如：请求的参数，发送的头信息等都属于客户端发来的信息，service()方法中形参接收的是 HttpServletRequest 接口的实例化对象，表示该对象主要应用在HTTP 协议上，该对象是由 Tomcat 封装好传递过来。

HttpServletRequest 是 ServletRequest 的子接口，ServletRequest 只有一个子接口，就是 HttpServletRequest。既然只有一个子接口为什么不将两个接口合并为一个？ 

从长远上讲：现在主要用的协议是 HTTP 协议，但以后可能出现更多新的协议。若以后想要支持这种新协议，只需要直接继承 ServletRequest 接口就行了。（扩展性）



**1）、常用方法**

![img](https://img-blog.csdnimg.cn/20190608114536638.png)

**2）、获取请求头**

![img](https://img-blog.csdnimg.cn/20190608114619637.png)

**3）、获取客户端请求参数**

![img](https://img-blog.csdnimg.cn/20190608114738853.png)

~~~ java
public class Servlet01 extends HttpServlet {
 
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("Servlet01.....");
		
		System.out.println("==============常用方法============");
		// 获取请求的完整路径 （从http到"?"前面）
		String url = request.getRequestURL() + "";
		System.out.println("获取请求的完整路径：" + url);
		// 获取请求的部分路径 （从端口后"?"前面）
		String uri = request.getRequestURI();
		System.out.println("获取请求的部分路径：" + uri);
		// 获取请求的参数字符串 （从"?"开始到最后）
		String queryString = request.getQueryString();
		System.out.println("获取请求的参数字符串：" +queryString);
		// 获取请求类型  （GET/POST）
		String method = request.getMethod();
		System.out.println("获取请求类型：" + method);
		// 获取请求协议版本
		String p = request.getProtocol();
		System.out.println("获取请求协议版本：" + p);
		// 获取站点名
		String contextPath = request.getContextPath();
		System.out.println("获取站点名：" + contextPath);
		
		System.out.println("===========获取请求头==========");
		// 获取指定请求头
		String host = request.getHeader("host");
		System.out.println("Host：" + host);
		// 获取所有请求头的名称枚举集合
		Enumeration<String> enumeration = request.getHeaderNames();
		while(enumeration.hasMoreElements()) {
			System.out.println(enumeration.nextElement());
		}
		
		
		System.out.println("==========获取请求的参数===========");
		// 获取指定名称的参数值
		String uname = request.getParameter("uname");
		String upwd = request.getParameter("upwd");
		System.out.println("用户名：" + uname + "，用户密码：" +upwd);
		// 获取指定名称的参数的所有值
		String[] hobbys = request.getParameterValues("hobby");
		// 判断并遍历
		if (hobbys != null && hobbys.length > 0) {
			for (String hobby : hobbys) {
				System.out.println("爱好：" + hobby);
			}
		}
			
	}
 
}

~~~

## HttpServletResponse

HttpServletResponse 的主要功能用于服务器对客户端的请求进行响应，将Web 服务器处理后的结果返回给客户端。

![img](https://img-blog.csdnimg.cn/20190608163346972.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg2Mzk4OA==,size_16,color_FFFFFF,t_70)

2、刷新和页面自动跳转

~~~ java
// 设置refresh响应头
// response.setHeader("refresh", "2"); // 2秒刷新一次
response.setHeader("refresh", "2;URL=http://www.baidu.com"); // 2秒钟后跳转到百度
response.setHeader("refresh","3;URL=ok.html");//3秒钟后跳转到ok.html页面
————————————————
版权声明：本文为CSDN博主「惹笑了一滩草」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_44863988/article/details/91305695
~~~

3、数据响应
getWriter()获取字符流(只能响应回字符)；
getOutputStream()获取字节流(能响应一切数据)。
响应回的数据到客户端被浏览器解析。
注意：两者不能同时使用。
若同时使用会报错：java.lang.IllegalStateException: getWriter() has already been called for this response

设置响应类型时，可以在server中Tomcat的web.xml中查找（Ctrl键+F键）

~~~java
// 设置响应类型(默认是纯文本)
response.setContentType("application/json");

String json = "{\"uanme\":\"zhangsan\",\"upwd\":\"123\"}";

// 得到字符输出流
PrintWriter writer = response.getWriter();
writer.write(json);
writer.flush();
writer.close();
//设置响应类型
response.setContentType("text/html");
		
//得到字节输出流
ServletOutputStream out = response.getOutputStream();
out.write("hehe".getBytes());
out.write("<h2>hehe</h2>".getBytes());
out.flush();
out.close();
~~~

## ServletContext

ServletContext是Servlet 上下文，是个接口，是 Web 应用中所有 Servlet 在 Web 容器中的运行时环境。**这个运行时环境随着 Web 应用的启动而创建，随着 Web 应用的关闭而销毁。也就是说，一个 Web 应用，就一个 Servlet 运行时环境，即一个 Servlet 上下文，即一个ServletContext 对象。**

这个 Servlet 运行环境中都包含哪些具体的“环境”呢？即 ServletContext 对象中都包含哪些具体的信息呢？不仅包含了 web.xml 文件中的配置信息，还包含了当前应用中所有Servlet 可以共享的数据。可以这么说，ServeltContext 可以代表整个应用。所以，ServletConetxt有另外一个名称：application。(EL表达式就是取这个名字)

~~~ java
String getInitParameter ()：
获 取 web.xml 文 件 的 <context-param/> 中 指 定 名 称 的 上 下 文 参 数 值 。 例 如getInitParameter(“myDBDriver”);会返回字符串“com.mysql.jdbc.Driver”。

Enumeration getInitParameterNames()：
获取 web.xml 文件的<context-param/>中的所有的上下文参数名称。其返回值为枚举类型 Enumeration<String>。

 void setAttribute(String name, Object object)：
在 ServletContext 的公共数据空间中，也称为域属性空间，放入数据。这些数据对于 Web
应用来说，是全局性的，与整个应用的生命周期相同。当然，放入其中的数据是有名称的，
通过名称来访问该数据。

Object getAttribute(String name)：
从 ServletContext 的域属性空间中获取指定名称的数据。

void removeAttribute(String name)：
从 ServletContext 的域属性空间中删除指定名称的数据。

String getRealPath(String path)：
获取当前 Web 应用中指定文件或目录在本地文件系统中的路径，是基于盘符的路径。

String getContextPath()：
获取当前应用在 Web 容器中的名称。
~~~



### Request域中



在 Request 中也存在域属性空间，用于存放有名称的数据。该数据只在**当前 Request 请求中可以进行访问**。

~~~ java
对于 Request 中的域属性操作的方法有：
void setAttribute(String name, Object object)：
在 Request 域属性空间中放入数据。其生命周期与 Request 的生命周期相同。

Object getAttribute(String name)：
从 Request 的域属性空间中获取指定名称的数据。

void removeAttribute(String name)：
从Request 的域属性空间中删除指定名称的数据。由于这里我们举例子需要用到另外一个方法，所以将这个方法也进行介绍。

RequestDispatcher getRequestDispatcher(String path);
该方法用于创建请求转发器，而该请求转发器有一个方法 forward()，用于完成将请求对
象转发给下一个资源。forward()方法的原型如下：
void forward(HttpServletRequest request, HttpServletResponse response);

~~~

### Session域中

Session 是一个专门用于存放数据的集合，我们一般称这个用于存放数据的内存空间为域属性空间，简称域。HttpSession 中具有三个方法，是专门用于对该域属性空间中数据进行写、读操作的。

若要对 Session 进行操作，则可以通过 HttpServletRequest 的 getSession()方法获取。该
方法具有两个重载的方法。***public HttpSession getSession(boolean create)***和***public HttpSession getSession()***

~~~ java
public void setAttribute(String name, Object value)
该方法用于向 Session 的域属性空间中放入指定名称、指定值的域属性。

public Object getAttribute(String name)
该方法用于从 Session 的域属性空间中读取指定名称为域属性值。

public void removeAttribute(String name)
该方法用于从 Session 的域属性空间中删除指定名称的域属性。

~~~

## Servlet  过滤器编写

Servlet 过滤器可以动态地拦截请求和响应，以变换或使用包含在请求或响应中的信息。

可以将一个或多个 Servlet 过滤器附加到一个 Servlet 或一组 Servlet。Servlet 过滤器也可以附加到 JavaServer Pages (JSP) 文件和 HTML 页面。调用 Servlet 前调用所有附加的 Servlet 过滤器。

Servlet 过滤器是可用于 Servlet 编程的 Java 类，可以实现以下目的：

- 在客户端的请求访问后端资源之前，拦截这些请求。
- 在服务器的响应发送回客户端之前，处理这些响应。

根据规范建议的各种类型的过滤器：

- 身份验证过滤器（Authentication Filters）。
- 数据压缩过滤器（Data compression Filters）。
- 加密过滤器（Encryption Filters）。
- 触发资源访问事件过滤器。
- 图像转换过滤器（Image Conversion Filters）。
- 日志记录和审核过滤器（Logging and Auditing Filters）。
- MIME-TYPE 链过滤器（MIME-TYPE Chain Filters）。
- 标记化过滤器（Tokenizing Filters）。
- XSL/T 过滤器（XSL/T Filters），转换 XML 内容。

过滤器通过 Web 部署描述符（web.xml）中的 XML 标签来声明，然后映射到您的应用程序的部署描述符中的 Servlet 名称或 URL 模式。

当 Web 容器启动 Web 应用程序时，它会为您在部署描述符中声明的每一个过滤器创建一个实例。

Filter的执行顺序与在web.xml配置文件中的配置顺序一致，一般把Filter配置在所有的Servlet之前。

### Servlet过滤器方法

过滤器是一个实现了 javax.servlet.Filter 接口的 Java 类。javax.servlet.Filter 接口定义了三个方法：



~~~ java
public void doFilter (ServletRequest, ServletResponse, FilterChain)
//该方法完成实际的过滤操作，当客户端请求方法与过滤器设置匹配的URL时，Servlet容器将先调用过滤器的doFilter方法。FilterChain用户访问后续过滤器。
    
public void init(FilterConfig filterConfig)
//web 应用程序启动时，web 服务器将创建Filter 的实例对象，并调用其init方法，读取web.xml配置，完成对象的初始化功能，从而为后续的用户请求作好拦截的准备工作（filter对象只会创建一次，init方法也只会执行一次）。开发人员通过init方法的参数，可获得代表当前filter配置信息的FilterConfig对象。
    
public void destroy()
//Servlet容器在销毁过滤器实例前调用该方法，在该方法中释放Servlet过滤器占用的资源。
~~~



### FiterConfig使用

~~~ xml
<filter>
    <filter-name>LogFilter</filter-name>
    <filter-class>com.runoob.test.LogFilter</filter-class>
    <init-param>
        <param-name>Site</param-name>
        <param-value>菜鸟教程</param-value>
    </init-param>
</filter>
~~~



~~~ java
public void  init(FilterConfig config) throws ServletException {
    // 获取初始化参数
    String site = config.getInitParameter("Site"); 
    // 输出初始化参数
    System.out.println("网站名称: " + site); 
}
~~~



