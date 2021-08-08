# 引入

​                                                           

![image-20210731155304853](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210731155304.png)



# Tomcat

## HTTP协议

协议：Protocol

### 定义

HTTP协议是**Hyper Text Transfer Protocol（超文本传输协议）**的缩写, HTTP是万维网（WWW:World Wide Web）的数据通信的基础。

HTTP是一个简单的请求-响应协议，它通常运行在TCP之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。

![image-20210802110327666](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802110328.png)


HTTP是一个基于**TCP/IP**通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802110337.png" alt="image-20210802110337133" style="zoom:67%;" />

### 特点

1支持客户/服务器模式

HTTP协议支持客户端服务端模式，需要使用浏览器作为客户端来访问服务端。

2简单快速

客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、POST等。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。

3灵活

HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type（Content-Type是HTTP包中用来表示内容类型的标识）加以标记。

4无连接

每次请求一次，释放一次连接。所以无连接表示每次连接只能处理一个请求。优点就是节省传输时间，实现简单。我们有时称这种无连接为短连接。对应的就有了长链接，长连接专门解决效率问题。当建立好了一个连接之后，可以多次请求。但是缺点就是容易造成占用资源不释放的问题。当HTTP协议头部中字段Connection：keep-alive表示支持长链接。

5单向性

服务端永远是被动的等待客户端的请求。

6无状态

HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。为了解决HTTP协议无状态，于是，两种用于保持HTTP连接状态的技术就应运而生了，一个是Cookie，而另一个则是Session。



HTTP协议发展和版本

http协议在1991年发布第一个版本版本号为0.9。随后WWW联盟（WWW Consortium-W3C）于1994年成立，http协议被纳入到W3C组织中进行维护和管理。








http1.0

最早在1996年在网页中使用，内容简单，所以浏览器的每次请求都需要与服务器建立一个TCP连接，服务器处理完成后立即断开TCP连接（无连接），服务器不跟踪每个客户端也不记录过去的请求（无状态）,请求只能由客户端发起（单向性）。

http1.1

到1999年广泛在各大浏览器网络请求中使用，HTTP/1.0中默认使用Connection: close。在HTTP/1.1中已经默认使用Connection: keep-alive（长连接），避免了连接建立和释放的开销，但服务器必须按照客户端请求的先后顺序依次回送相应的结果，以保证客户端能够区分出每次请求的响应内容。通过Content-Length字段来判断当前请求的数据是否已经全部接收。不允许同时存在两个并行的响应。

1.1中最重要的一个特点是支持“长连接”，即“一次连接可以多次请求”。



HTTP 1.1支持持久连接（HTTP/1.1的默认模式使用带流水线的持久连接），在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立和关闭连接的消耗和延迟。一个包含有许多图像的网页文件的多个请求和应答可以在一个连接中传输，但每个单独的网页文件的请求和应答仍然需要使用各自的连接。HTTP 1.1还允许客户端不用等待上一次请求结果返回，就可以发出下一次请求，但服务器端必须按照接收到客户端请求的先后顺序依次回送响应结果，以保证客户端能够区分出每次请求的响应内容，这样也显著地减少了整个下载过程所需要的时间。

http2.0

长连接

在HTTP/2中，客户端向某个域名的服务器请求页面的过程中，只会创建一条TCP连接，即使这页面可能包含上百个资源。  单一的连接应该是HTTP2的主要优势，单一的连接能减少TCP握手带来的时延。HTTP2中用一条单一的长连接，避免了创建多个TCP连接带来的网络开销，提高了吞吐量。

多路复用 (Multiplexing)

HTTP2.0中所有加强性能的核心是二进制传输，在HTTP1.x中，我们是通过文本的方式传输数据。在HTTP2.0中引入了新的编码机制，所有传输的数据都会被分割，并采用二进制格式编码。





多路复用，连接共享。不同的request可以使用同一个连接传输（最后根据每个request上的id号组合成正常的请求）。

HTTP2.0中，有两个概念非常重要：帧（frame）和流（stream）。
帧是最小的数据单位，每个帧会标识出该帧属于哪个流，流是多个帧组成的数据流。
所谓多路复用，即在一个TCP连接中存在多个流，即可以同时发送多个请求，对端可以通过帧中的表示知道该帧属于哪个请求。在客户端，这些帧乱序发送，到对端后再根据每个帧首部的流标识符重新组装。通过该技术，可以避免HTTP旧版本的队头阻塞问题，极大提高传输性能。






首部压缩（Header Compression）

由于1.1中header带有大量的信息，并且得重复传输，2.0使用encoder来减少需要传输的hearder大小。

服务端推送（Server Push）

在HTTP2.0中，服务端可以在客户端某个请求后，主动推送其他资源。
可以想象一下，某些资源客户端是一定会请求的，这时就可以采取服务端push的技术，提前给客户端推送必要的资源，就可以相对减少一点延迟时间。在浏览器兼容的情况下也可以使用prefetch。

更安全

HTTP2.0使用了tls的拓展ALPN做为协议升级，除此之外，HTTP2.0对tls的安全性做了近一步加强，通过黑名单机制禁用了几百种不再安全的加密算法。



# Servlet

## 1.   引入

### 介绍

Servlet是Server Applet的简称，称为服务端小程序，是JavaEE平台下的技术标准，基于Java语言编写的服务端程序。Web容器或应用服务器实现了Servlet标准所以Servlet需运行在Web容器或应用服务器中。Servlet主要功能在于能在服务器中执行并生成数据。

### 特点

使用单进程多线程方式运行

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802204053.png" alt="image-20210802204049056" style="zoom:80%;" />

### 应用程序中的位置

![image-20210802204157808](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802204157.png)

### 静态资源和动态资源的区分

- 静态资源：每次访问都不需要运算，直接就可以返回的资源，如HTML CSS JS 多媒体文件等等 每次访问获得地资源都是一样的
- 动态资源：每次访问都需要运算代码生成的资源如 Servlet JSP 每次访问获得的结果可能都是不一样的

Servlet 作为一种动态资源技术 是后面学习框架的基础

### Servlet在程序中的地位

Servlet是可以接受Http请求并作出相应的一种技术,是JAVA语言编写的一种动态资源
Servlet是前后端衔接的一种技术,不是所有的JAVA类都可以接收请求和作出相应,Servlet可以

在MVC模式中,Servlet作为Controller层(控制层)主要技术,用于和浏览器完成数据交互,控制交互逻辑

### ==servlet三大域对象==

**Servlet三大域对象的应用 request、session、application（ServletContext）**

**ServletContext是一个全局的储存信息的空间，服务器开始就存在，服务器关闭才释放。**

**request，一个用户可有多个；session，一个用户一个；而servletContext，所有用户共用一个。所以，为了节省空间，提高效率，ServletContext中，要放必须的、重要的、所有用户需要共享的线程又是安全的一些信息。**

## 案例1：初步认识

- 创建一个JAVAWEB项目，并在项目中开发一个自己的Servlet，继承HttpServlet类

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802205500.png" alt="image-20210802205500530" style="zoom:50%;" />

  ![image-20210802205532234](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802205532.png)

  

- 在MyServlet类中重写service方法

  ![image-20210802205558243](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802205558.png)

- 在service方法中定义具体的功能代码

  ```java
  @Override
  protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      //动态生成数据
      //随机生成一个整数
      int num = new Random().nextInt();
      String message = (num%2==0)?"happy birthday":"happy new year";
      //对浏览器做出响应
      PrintWriter writer = resp.getWriter();//该打印流指向了浏览器
      writer.write(message);
  
  }
  ```

  

- 在web.xml中配置Servlet的映射路径

  ```xml
  <!--向Tomcat声明一个Servlet-->
  <servlet>
      <servlet-name>myFirstServlet</servlet-name><!--这只是别名-->
      <servlet-class>com.chs.servlet.Myservlet</servlet-class>
  </servlet>
  <!--给Servlet匹配一个七个球的映射路径-->
  <servlet-mapping>
      <servlet-name>myFirstServlet</servlet-name>
      <url-pattern>/mySerlvet.do</url-pattern>
  </servlet-mapping>
  ```

  

- 打开浏览器请求Servlet资源

  ![image-20210802205836668](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802205836.png)

## 案例2：登录页

### 需求：

准备一个登陆页，可以输入要用户名和密码

输入完毕向Servlet提交用户名和密码

Servlet接收到用户名和密码之后校验是否正确

如果正确响应success

如果不正确响应Failed

### 具体步骤：

- 项目结构：

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802210210.png" alt="image-20210802210210084" style="zoom:67%;" />

- 开发登录页：

  ```html
  <!DOCTYPE html>
  <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Title</title>
      </head>
      <body>
          <form method="get" action="loginServlet.do">
              <table style="margin: 0px auto" width="300px" cellpadding="0px" cellspacing="0px" border="1px">
                  <tr>
                      <td>用户名</td>
                      <td>
                          <input type="text" name="username" >
                      </td>
                  </tr>
                  <tr>
                      <td>密码</td>
                      <td>
                          <input type="password" name="pwd">
                      </td>
                  </tr>
                  <tr align="center">
                      <td colspan="2">
                          <input type="submit" value="登录">
                      </td>
                  </tr>
              </table>
          </form>
      </body>
  </html>
  ```

- 开发后台Servlet

  ```java
  public class LoginServlet extends HttpServlet {
      @Override
      protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("login servlet invoked");
          // 获取请求中的数据
          String username = req.getParameter("username");
          String pwd = req.getParameter("pwd");
          // 判断数据
          String message=null;
          if(username.equals("mashibing")&& pwd.equals("123456")){
              message="Success";
          }else{
              message="Fail";
          }
          // 作出响应
          resp.getWriter().write(message);
      }
  ```

  

- 配置Servlet

  ```xml
  <servlet>
      <servlet-name>loginServlet</servlet-name>
      <servlet-class>com.mashibing.servlet.LoginServlet</servlet-class>
  </servlet>
  
  <servlet-mapping>
      <servlet-name>loginServlet</servlet-name>
      <url-pattern>/loginServlet.do</url-pattern>
  </servlet-mapping>
  
  
  <welcome-file-list>
      <welcome-file>login.html</welcome-file>
  </welcome-file-list>
  
  ```

运行测试

![image-20210802210437528](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802210437.png)

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802210446.png" alt="image-20210802210446147" style="zoom:67%;" />



## 2. HttpServletRequest

一个http可以分为三个部分：**请求行 请求头 请求体**

### 请求行

- 请求方式：GET   
- 请求的URL： http://192.168.56.220:8080/logOnDemo/logon.html
- 协议及版本： HTTP/1.1

### 请求头

![image-20210802210812138](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802210812.png)



### 请求体

get方式提交的请求数据通过地址栏提交 ,没有请求体
post方式提交请求数据单独放到请求体中,请求时所携带的数据 (post方式)

### http支持的请求方式

![image-20210802211209830](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210802211209.png)

#### ==get和post的区别==

==(面试）==

- GET在浏览器回退时是无害的，而POST会再次提交请求。

- 
  GET产生的URL地址可以被Bookmark，而POST不可以。

- 
  GET请求会被浏览器主动cache，而POST不会，除非手动设置。

- 
  GET请求只能进行url编码，而POST支持多种编码方式。

- 
  GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。

- 
  GET请求在URL中传送的参数是有长度限制的，而POST则没有。对参数的数据类型GET只接受ASCII字符，而POST即可是字符也可是字节。

- 
  GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。

- 
  GET参数通过URL传递，POST放在Request body中。



### 具体操作：获得客户端请求信息

HttpServletRequest对象代表客户端浏览器的请求，当客户端浏览器通过HTTP协议访问服务器时，HTTP请求中的所有信息都会被Tomcat所解析并封装在这个对象中，通过这个对象提供的方法，可以获得客户端请求的所有信息。

#### 1.获取请求行信息

```java
req.getRequestURL()://返回客户端浏览器发出请求时的完整URL。

req.getRequestURI()://返回请求行中指定资源部分。

req.getRemoteAddr()://返回发出请求的客户机的IP地址。

req.getLocalAddr()://返回WEB服务器的IP地址。

req.getLocalPort()://返回WEB服务器处理Http协议的连接器所监听的端口。
```

#### 2.获取请求头信息

```java
req.getHeader("headerKey")://根据请求头中的key获取对应的value。
String headerValue = req.getHeader("headerKey");
req.getHeaderNames()://获取请求头中所有的key，该方法返回枚举类型。
Enumeration<String> headerNames = req.getHeaderNames();
```



测试代码

```java
public class Servlet3 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println(req.getRequestURL());//返回客户端浏览器发出请求时的完整URL。
        System.out.println(req.getRequestURI());//返回请求行中指定资源部分。
        System.out.println(req.getRemoteAddr());//返回发出请求的客户机的IP地址。
        System.out.println(req.getLocalAddr());//返回WEB服务器的IP地址。
        System.out.println(req.getLocalPort());//返回WEB服务器处理Http协议的连接器所监听的端口。
        System.out.println("主机名: " + req.getLocalName());
        System.out.println("客户端PORT: " + req.getRemotePort());
        System.out.println("当前项目部署名: " + req.getContextPath());
        System.out.println("协议名:"+req.getScheme());
        System.out.println("请求方式:"+req.getMethod());

        // 根据请求头名或者请求头对应的值
        System.out.println(req.getHeader("Accept"));
        // 获得全部的请求头名
        Enumeration<String> headerNames = req.getHeaderNames();
        while (headerNames.hasMoreElements()){
            String headername = headerNames.nextElement();
            System.out.println(headername+":"+req.getHeader(headername));
        }
    }
}
```



#### 3.获取请求数据

- 根据key获取指定value:

  ```java
  req.getParameter("key")://根据key获取指定value。
  ```

- 获```java取复选框(checkbox组件)中的值（多个值）：

  ```java
  eq.getParameterValues("checkboxkey")://获取复选框(checkbox组件)中的值，返回一个String[]。
  ```

- 获取所有提交数据的key

  ````java
  req.getParameterNames()://获取请求中所有数据的key，该方法返回一个枚举类型。
  ````

- 使用Map结构获取提交数据

  ```java
  req.getParameterMap()://获取请求中所有的数据并存放到一个Map结构中，该方法返回一个Map，其中key为String类型value为String[]类型。
  ```

- 设置请求编码

  ```java
  req.setCharacterEncoding("utf-8")
  ```

  请求的数据包基于字节在网络上传输，Tomcat接收到请求的数据包后会将数据包中的字节转换为字符。在Tomcat中使用的是ISO-8859-1的单字节编码完成字节与字符的转换，所以数据中含有中文就会出现乱码，可以通过req.setCharacterEncoding("utf-8")方法来对提交的数据根据指定的编码方式重新做编码处理。

## 案例3:HTTP请求

**需求 ：获得前端客户端表单中请求的数据信息**

### 前端代码

#### 开发form表单注意事项

1. form 不是from

2. form表单内部不是所有的标签信息都会提交 一些输入信息  input select textarea ... ...

3. 提交的标签必须具备**name属性**  name属性的作用是让后台区分数据  id便于在前端区分数据

4. 提交的标签一般都要具备**value属性**  value属性确定我们要提交的具体的数据

5. ==get post==[区别](#get和post的区别)
     get方式数据是通过URL携带
     提交的数据只能是文本
     提交的数据量不大
     get方式提交的数据相对不安全

     post 将数据单独打包放到请求体中
     提交的数据可以是文本可以是各种文件
     提交的数据量理论上没有上限
     post方式提交数据相对安全

  ==readonly只读== 也是会提交数据的
  ==hidden==  隐藏 也是会提交数据
  ==disabled== 不可用 显示但是不提交

**代码：**

```java
<form method="get" action="myServlet">
    <table style="margin: 0px auto" width="300px" cellpadding="0px" cellspacing="0px" border="1px">
        <tr>
            <td>用户名</td>
            <td>
                <input type="text" name="username" id="in1" value="12345" disabled >
            </td>
        </tr>
        <tr>
            <td>密码</td>
            <td>
                <input type="password" name="pwd">
            </td>
        </tr>
        <tr>
            <td>性别</td>
            <td>
                <input type="radio" name="gender" value="1" checked>男
                <input type="radio" name="gender" value="0">女
            </td>
        </tr>
        <tr>
            <td>爱好</td>
            <td>
                <input type="checkbox" name="hobby" value="1">蓝球
                <input type="checkbox" name="hobby" value="2">足球
                <input type="checkbox" name="hobby" value="3">羽毛球
                <input type="checkbox" name="hobby" value="4">乒乓球
            </td>
        </tr>
        <tr>
            <td>个人简介</td>
            <td>
                <!--文本域 双标签 页面上显示的文字是双标签中的文本 不是value属性

                    文本域提交的数据不是value属性值,是双标签中的文本
                -->
                <textarea name="introduce" >b</textarea>
            </td>
        </tr>

        <tr>
            <td>籍贯</td>
            <td>
                <!--
                select
                option没有定义value属性 那么就提交option中间的文字(不推荐)

                -->
                <select name="provience">
                    <option value="1">a京</option>
                    <option value="2">b津</option>
                    <option value="3">c冀</option>
                </select>
            </td>
        </tr>
        <tr align="center">

            <td colspan="2">
                <input type="submit" value="提交数据">
            </td>
        </tr>
    </table>
</form>
```

**效果：**

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210803102738.png" alt="image-20210803102513541" style="zoom: 50%;" />

### Servlet代码

```java
public class MyServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // req获取参数
        // 如果 前端发过来的数据由数据名但是没有值, getParameter返回的是一个空字符串  ""
        // 获取的参数在提交的数据中名都没有,getParameter返回的是null
        String username = req.getParameter("username");
        System.out.println("username:"+username);
        System.out.println("password:"+req.getParameter("pwd"));
        System.out.println("gender:"+req.getParameter("gender"));
        // hobby=1&hobby=2&hobby=3 想要获得多个同名的参数 getParameterValues 返回的是一个Sting数组
        String[] hobbies = req.getParameterValues("hobby");
        System.out.println("hobbies:"+ Arrays.toString(hobbies));

        // textarea
        System.out.println("introduce:"+req.getParameter("introduce"));

        // select
        System.out.println("provience:"+req.getParameter("provience"));

        System.out.println("___________________________");
        // 如果不知道参数的名字?
        // 获取所有的参数名
        Enumeration<String> pNames = req.getParameterNames();
        while(pNames.hasMoreElements()){
            String pname = pNames.nextElement();
            String[] pValues = req.getParameterValues(pname);
            System.out.println(pname+":"+Arrays.toString(pValues));
        }

        System.out.println("________________________________");
        Map<String, String[]> pmap = req.getParameterMap();
        Set<Map.Entry<String, String[]>> entries = pmap.entrySet();
        for (Map.Entry<String, String[]> entry : entries) {
            System.out.println(entry.getKey()+":"+Arrays.toString(entry.getValue()));
        }

    }
}

```

运行结果：

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210803102814.png" alt="image-20210803102714186" style="zoom:50%;" />

## 3. HttpServletResponse

http响应部分可以分为三部分：**响应行，响应头，响应体**

### 响应行

![image-20210803161927018](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210803161928.png)

响应状态码列表

![image-20210803161950030](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210803161950.png)

### 响应头

Content-Type：响应内容的类型(MIME)

![image-20210803162052600](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210803162052.png)

### 响应实体

即**服务器响应回来的内容**

​		HttpServletResponse对象代表服务器的响应，封装了响应客户端浏览器的**流对象**，以及向客户端浏览器响应的**响应头、响应数据、响应状态码等信息**。



#### ContentType:响应设置

```java
resp.setContentType("MIME")://该方法可通过MIME-Type设置响应类型。
```

###### **MIME**

全称是**Multipurpose Internet Mail Extensions**，即多用途互联网邮件扩展类型。
这是HTTP协议中用来定义文档性质及格式的标准。对HTTP传输内容类型进行了全面定义。
服务器通过MIME告知响应内容类型，而浏览器则通过MIME类型来确定如何处理文档。

**HTTP content-type 类型表：**

https://www.runoob.com/http/http-content-type.html

**常见的媒体格式类型如下：**

- text/html ： HTML格式
- text/plain ：纯文本格式
- text/xml ： XML格式
- image/gif ：gif图片格式
- image/jpeg ：jpg图片格式
- image/png：png图片格式

以application开头的媒体格式类型：

- application/xhtml+xml ：XHTML格式
- application/xml： XML数据格式
- application/atom+xml ：Atom XML聚合格式
- application/json： JSON数据格式
- application/pdf：pdf格式
- application/msword ： Word文档格式
- application/octet-stream ： 二进制流数据（如常见的文件下载）
- application/x-www-form-urlencoded ： <form encType="">中默认的encType，form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式）

另外一种常见的媒体格式是上传文件之时使用的：

- multipart/form-data ： 需要在表单中进行文件上传时，就需要使用该格式

**常见的字节型响应：**

- image/jpeg：图片类型为jpeg或jpg格式。


- image/gif: 图片类型为gif格式。

**设置响应编码**

```java
response.setCharacterEncoding("utf-8");

response.setContentType("text/html;charset=utf-8");
```

设置服务端为浏览器产生响应的响应编码，服务端会根据此编码将响应内容的字符转换为字节。同时客户端浏览器会根据此编码方式显示响应内容。

**在响应中添加附加信息（文件下载）**

在实现文件下载时，我们需要修改响应头，添加附加信息。

```java
response.setHeader("Content-Disposition",   "attachment; filename="+文件名);
```

==Content-Disposition:attachment==

该附加信息表示作为对下载文件的一个标识字段。不会在浏览器中显示而是直接做下载处理。

filename=文件名,表示指定下载文件的文件名。

解决文件名中文乱码问题:

```java
resp.addHeader("Content-Disposition","attachment;filename="+new String (file.getName().getBytes("gbk"),"iso-8859-1"));
```



### 测试代码

```java
public class MyServlet2 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置响应码
        //resp.setStatus(500);
        //resp.setStatus(405, "request method not supported");

        // 设置响应头
        //resp.setHeader("Date","2022-11-11");
        // 自定义头
        // resp.setHeader("aaa", "bbb");
        // 高速浏览器响应的数据是什么? 浏览器根据此头决定 数据如何应用
        // 设置MIME类型 json  xml 文件下载  ... ...
        // resp.setHeader("content-type", "text/css");
        resp.setContentType("text/html");// 专门用于设置Content-Type 响应头
        resp.getWriter().write("<h1>this is tag h1</h1>");
    }
}

```

## 乱码问题

### 1 控制台乱码

设置Tomcat中 conf下logging.properties中所有的UTF-8编码为GBK即可

### 2 post请求乱码

通过HttpServletRequest设置请求编码

```java
  req.setCharacterEncoding("UTF-8");
```

### 3 get请求乱码

需要手动进行编码解码,或者设置tomcat中的server.xml中的URI编码.tomcat9已经解决了该问题

```xml
<Connector   port="8080" protocol="HTTP/1.1"
    connectionTimeout="20000"
    redirectPort="8443" URIEncoding="utf-8"  />
```

### 4 响应乱码

通过HttpServletResponse设置响应编码

```java
//以UTF-8编码处理数据
resp.setContentType("UTF-8");
//设置响应头,以便浏览器知道以何种编码解析数据
resp.setContentType("text/html;charset=UTF-8");
```



## 4. servlet生命周期

### 四个阶段

Servlet的生命周期是由容器管理的，分别经历四个阶段：

| 阶段               | 次数 | 时机       |
| ------------------ | ---- | ---------- |
| 创建 new           | 1次  | 第一次请求 |
| 初始化 init()      | 1次  | 实例化之后 |
| 执行服务 service() | 多次 | 每次请求   |
| 销毁 destroy()     | 1次  | 停止服务   |

当客户端浏览器第一次请求Servlet时，容器会实例化这个Servlet，然后调用一次init方法，并在新的线程中执行service方法处理请求。service方法执行完毕后容器不会销毁这个Servlet而是做缓存处理，当客户端浏览器再次请求这个Servlet时，容器会从缓存中直接找到这个Servlet对象，并再一次在新的线程中执行Service方法。当容器在销毁Servlet之前对调用一次destory方法。

**在Servlet中我们一般不要轻易使用成员变量!!!! 可能会造成线程安全问题**

**如果要使用的话,应该尽量避免对成员变量产生修改**

**如果要产生修改我们应该注意线程安全问题**

**如果我们自己添加线程安全编码处理,会严重影响效率**

**综上所述:原则,能不用成员变量就不用!!!**

servlet代码：

```java
public class MyServlet4 extends HttpServlet {
    // 成员变量
    public MyServlet4()   {// 构造一个Servlet对象的方法
        System.out.println("MyServlet4 Constructor invoked");
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    @Override
    public void init() throws ServletException {// 初始化
        System.out.println("MyServlet4 init invoked");
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 执行服务
        System.out.println("MyServlet4 service invoked");
    }
    @Override
    public void destroy() {// 销毁
        System.out.println("MyServlet4 destory invoked");
    }
}
```

配置：

```xml
<servlet>
    <servlet-name>myServlet4</servlet-name>
    <servlet-class>com.mashibing.servlet.MyServlet4</servlet-class>
    <load-on-startup>6</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>myServlet4</servlet-name>
    <url-pattern>/myServlet4.do</url-pattern>
</servlet-mapping>
```

​		多次请求servlet并查看控制台输出即可印证上述结论,值得注意的是,如果需要Servlet在服务启动时就实例化并初始化,我们可以在servlet的配置中添加load-on-startup配置启动顺序,配置的数字为启动顺序,应避免冲突且应**>6**

![image-20210803205409578](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210803205411.png)

**Servlet处理请求的过程**

​		当浏览器基于get方式请求我们创建Servlet时，我们自定义的Servlet中的doGet方法会被执行。doGet方法能够被执行并处理get请求的原因是，容器在启动时会解析web工程中WEB-INF目录中的web.xml文件，在该文件中我们配置了Servlet与URI的绑定，容器通过对请求的解析可以获取请求资源的URI，然后找到与该URI绑定的Servlet并做实例化处理(注意：只实例化一次，如果在缓存中能够找到这个Servlet就不会再做次实例化处理)。在实例化时会使用Servlet接口类型作为引用类型的定义，并调用一次init方法，由于HttpServlet中重写了该方法所以最终执行的是HttpServlet中init方法(HttpServlet中的Init方法是一个空的方法体)，然后在新的线程中调用service方法。由于在HttpServlet中重写了Service方法所以最终执行的是HttpServlet中的service方法。在service方法中通过request.getMethod()获取到请求方式进行判断如果是Get方式请求就执行doGet方法，如果是POST请求就执行doPost方法。如果是基于GET方式提交的，并且在我们自定义的Servlet中又重写了HttpServlet中的doGet方法，那么最终会根据Java的多态特性转而执行我们自定义的Servlet中的doGet方法。

![image-20210803205435158](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210803205435.png)

- 老程序员喜欢重写doGet()和doPost()方法 然后挑一个方法里面直接调用另一个方法，因为两个方法很相似。
- 但是又可以直接重写service，可以同时处理get和post

## 5. ServletContext和ServletConfig

### ServletContext(application)

#### 定义

ServletContext官方叫Servlet上下文。服务器会为每一个Web应用创建一个ServletContext对象。这个对象全局唯一，而且Web应用中的所有Servlet都共享这个对象。所以叫全局应用程序共享对象

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210804094215.png" alt="image-20210804094215771" style="zoom:67%;" />

#### 作用

- 相对路径转绝对路径
- 获取容器的附加信息
- 读取配置信息
- 全局容器

#### 操作

- 获取项目的部署名
  ==context.getContextPath()==

- 相对路径转绝对路径(文件上传下载时需要注意)

  ==context.getRealPath("path")==

  该方法可以将一个相对路径转换为绝对路径，在文件上传与下载时需要用到该方法做路径的转换。

- 获取容器的附加信息

  ==servletContext.getServerInfo()==

- 返回Servlet容器的名称和版本号

  - 返回Servlet容器所支持Servlet的主版本号

    ==servletContext.getMajorVersion()==

  - 返回Servlet容器所支持Servlet的副版本号

    ==servletContext.getMinorVersion()==

- 获取web.xml文件中的信息

  ```xml
  <context-param>
      <param-name>key</param-name>
      <param-value>value</param-value>
  </context-param>
  ```

  - 读取web.xml文件中<context-param>标签中的配置信息

    ==servletContext.getInitParameter("key")==

  - 读取web.xml文件中所有param-name标签中的值。

    ==servletContext.getInitParameterNames()==

- 全局容器

  - 向全局容器中存放数据。

    ==servletContext.setAttribute("key",ObjectValue)==

  - 从全局容器中获取数据。

    ==servletContext.getAttribute("key")==

  - 根据key删除全局容器中的value。

    ==servletContext.removeAttribute("key")==

##### 测试代码：

**xml配置**

```xml
<context-param>
    <param-name>username</param-name>
    <param-value>mashibing</param-value>
</context-param>
<context-param>
    <param-name>password</param-name>
    <param-value>123456</param-value>
</context-param>
<servlet>
    <servlet-name>servlet1</servlet-name>
    <servlet-class>com.msb.testServlet.Servlet1</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servlet1</servlet-name>
    <url-pattern>/servlet1.do</url-pattern>
</servlet-mapping>
```

**servlet**

```java
public class Servlet1 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1 通过req获取Servlet对象
        ServletContext servletContext1 = req.getServletContext();
        //通过继承的方法获取Servlet对象
        ServletContext servletContext2 = this.getServletContext();
        //比较两种获取方法获取的对象
        System.out.println(servletContext1==servletContext2);

        //2 获取部署名
        String contextPath = servletContext1.getContextPath();
        System.out.println("contextPath"+contextPath);

        //3 将一个相对路径转换为项目的绝对路径
        String fileUpload = servletContext1.getRealPath("fileUpload");
        System.out.println(fileUpload);

        //4 获取容器的附加信息
        String serverInfo = servletContext1.getServerInfo();
        System.out.println(serverInfo);

        //5 获取Servlet容器的名称和版本号
        //主版本号
        int majorVersion = servletContext1.getMajorVersion();
        //副版本号
        int minorVersion = servletContext1.getMinorVersion();
        System.out.println("majorVersion"+majorVersion);
        System.out.println("minorVersion"+minorVersion);

        //6 读取web.xml文件信息
        //获取配置的全局初始信息
        String username = servletContext1.getInitParameter("username");
        String password = servletContext1.getInitParameter("password");
        System.out.println("username"+username);
        System.out.println("password"+password);
        //配置信息名称未知情况下 获取全局初始信息
        Enumeration<String> pNames = servletContext1.getInitParameterNames();
        while (pNames.hasMoreElements()){
            String e = pNames.nextElement();
            System.out.println(e+":"+ servletContext1.getInitParameter(e));
        }



        //7 向ServletContext对象中增加数据域对象
        List<String> data = new ArrayList<>();
        Collections.addAll(data,"张三","李四","王武");
        servletContext1.setAttribute("list",data);
        servletContext1.setAttribute("gender","boy");
        //getAttribute也可以获得初始信息对象
        List<String> list = (List<String>) servletContext1.getAttribute("list");
        System.out.println(list);
        String gender = (String) servletContext1.getAttribute("gender");
        System.out.println(gender);


    }
}

```

测试结果

![image-20210804112522391](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210804112525.png)

#### 生命周期

​		当容器启动是会创建ServletContext对象并一直缓存该对象，知道容器关闭后该对象生命周期结束。ServletContext对象的生命周期非常长，所以在使用全局容器时不建议存放业务数据



### ServletConfig

#### 定义

ServletConfig对象对应web.xml文件中的<servlet>节点。当Tomcat初始化一个Servlet时，会将该Servlet的配置信息，封装到一个ServletConfig对象中。

#### 操作

我们通过Config对象读取servlet节点中的配置信息

```xml
<servlet>
    <servlet-name>servletName</servlet-name>
    <servlet-class>servletClass</servlet-class>
    <init-param>
        <param-name>key</param-name>
        <param-value>value</param-value>
    </init-param>
</servlet>
```

- 读取web.xml文件中<servlet>标签中<init-param>标签中的配置信息。

  ==servletConfig.getInitParameter("key");==

- 读取web.xml文件中当前<servlet>标签中所有<init-param>标签中的值。

  ==servletConfig.getInitParameterNames();==



##### 测试代码 

 **xml配置**

```xml
<servlet>
    <servlet-name>servlet3</servlet-name>
    <servlet-class>com.msb.testServlet.Servlet3</servlet-class>
    <init-param>
        <param-name>brand</param-name>
        <param-value>ASUS</param-value>
    </init-param>
    <init-param>
        <param-name>screen</param-name>
        <param-value>三星</param-value>
    </init-param>
</servlet>
<servlet>
    <servlet-name>servlet4</servlet-name>
    <servlet-class>com.msb.testServlet.Servlet4</servlet-class>
    <init-param>
        <param-name>pinpai</param-name>
        <param-value>联想</param-value>
    </init-param>
    <init-param>
        <param-name>pingmu</param-name>
        <param-value>京东方</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>servlet3</servlet-name>
    <url-pattern>/servlet3.do</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <servlet-name>servlet4</servlet-name>
    <url-pattern>/servlet4.do</url-pattern>
</servlet-mapping>

```

**servlet**

```java
public class Servlet3 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletConfig servletConfig = this.getServletConfig();
        System.out.println(servletConfig.getInitParameter("brand"));
        System.out.println(servletConfig.getInitParameter("screen"));
    }
}
```

```java
public class Servlet4 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletConfig servletConfig = this.getServletConfig();
        System.out.println(servletConfig.getInitParameter("pinpai"));
        System.out.println(servletConfig.getInitParameter("pingmu"));
    }
}

```



## 6. URL的匹配规则

### 精确匹配

精确匹配是指<url-pattern>中配置的值必须与url完全精确匹配。

```xml
<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>/demo.do</url-pattern>
</servlet-mapping>
```

http://localhost:8888/demo/demo.do 匹配

http://localhost:8888/demo/suibian/demo.do 不匹配



### 扩展名匹配

在<url-pattern>允许使用统配符“*”作为匹配规则，“*”表示匹配任意字符。在扩展名匹配中只要扩展名相同都会被匹配和路径无关。注意，在使用扩展名匹配时在<url-pattern>中不能使用“/”，否则容器启动就会抛出异常。

```xml
<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

http://localhost:8888/demo/abc.do 匹配

http://localhost:8888/demo/suibian/haha.do 匹配

http://localhost:8888/demo/abc 不匹配



### 路径匹配

根据请求路径进行匹配，在请求中只要包含该路径都匹配，“*”表示任意路径以及子路径

```xml
<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>/suibian/*</url-pattern>
</servlet-mapping>
```

http://localhost:8888/demo/suibian/haha.do 匹配

http://localhost:8888/demo/suibian/hehe/haha.do 匹配

http://localhost:8888/demo/hehe/heihei.do 不匹配



### 任意匹配

匹配“/ 匹配所有但不包括JSP页面

```xml
<url-pattern>/</url-pattern>
```

http://localhost:8888/demo/suibian.do匹配

http://localhost:8888/demo/addUser.html匹配

http://localhost:8888/demo/css/view.css匹配

http://localhost:8888/demo/addUser.jsp不匹配

http://localhost:8888/demo/user/addUser.jsp不匹配

### 匹配所有

```xml
<url-pattern>/*</url-pattern>
```

http://localhost:8888/demo/suibian.do匹配

http://localhost:8888/demo/addUser.html匹配

http://localhost:8888/demo/suibian/suibian.do匹配

### 优先顺序

当一个url与多个Servlet的匹配规则可以匹配时，则按照 “ 精确路径 > 最长路径 >扩展名”这样的优先级匹配到对应的Servlet。

#### 案例分析

Servlet1映射到 /abc/*

Servlet2映射到 /*

Servlet3映射到 /abc

Servlet4映射到 *.do

当请求URL为“/abc/a.html”，“/abc/*”和“/*”都匹配，Servlet引擎将调用Servlet1。

当请求URL为“/abc”时，“/abc/*”和“/abc”都匹配，Servlet引擎将调用Servlet3。

当请求URL为“/abc/a.do”时，“/abc/*”和“*.do”都匹配，Servlet引擎将调用Servlet1。

当请求URL为“/a.do”时，“/*”和“*.do”都匹配，Servlet引擎将调用Servlet2。

当请求URL为“/xxx/yyy/a.do”时，“/*”和“*.do”都匹配，Servlet引擎将调用Servlet2。

### URL映射方式

在web.xml文件中支持将多个URL映射到一个Servlet中，但是相同的URL不能同时映射到两个Servlet中。

#### 方式一

```xml
<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>/suibian/*</url-pattern>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

#### 方式二

```xml
<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>/suibian/*</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <servlet-name>demoServlet</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```





## 7. 注解开发

**基于注解式开发Servlet**

​	在Servlet3.0以及之后的版本中支持注解式开发Servlet。对于Servlet的配置不在依赖于web.xml配置文件而是使用@WebServlet将一个继承于javax.servlet.http.HttpServlet的类定义为Servlet组件。

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface WebServlet {
    String name() default "";

    String[] value() default {};

    String[] urlPatterns() default {};

    int loadOnStartup() default -1;

    WebInitParam[] initParams() default {};

    boolean asyncSupported() default false;

    String smallIcon() default "";

    String largeIcon() default "";

    String description() default "";

    String displayName() default "";
}

```

### 相关属性

![image-20210804163546326](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210804163548.png)



## 8. forward 请求转发

### forward处理流程

1. 清空Response存放响应正文数据的缓冲区
2. 如果目标资源为Servlet或JSP，就调用它们的service()方法，把该方法产生的响应结果发送到客户端；如果目标资源文件系统中的静态HTML，就读取文档中的数据把它发送到客户端。

![image-20210804165930737](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210804165930.png)

- 请求转发是一种**服务器的行为**,是对浏览器屏蔽
- 浏览器的地址栏不会发生变化
- 请求的参数是可以从源组件传递到目标组件的
- 请求对象和响应对象没有重新创建,而是传递给了目标组件
- 请求转发可以帮助我们完成页面的跳转
- 请求转发可以转发至WEB-INF里
- 请求转发只能转发给当前项目的内部资源,不能转发至外部资源
- 常用forward

### forword处理特点

1. 由于forword()方法先清空用于存放响应正文的缓冲区，因此源Servlet生成的响应结果不会被发送到客户端，只有目标资源生成的响应结果才会被发送到客户端。
2. 如果源Servlet在进行请求转发之前，已经提交了响应结（flushBuffer(),close()方法），那么forward()方法抛出IllegalStateException。为了避免该异常，不应该在源Servlet中提交响应结果。

#### 注意

1. 在forward转发模式下,请求应该完全交给目标资源去处理,我们在源组件中,不要作出任何的响应处理

2. 在forward方法调用之后,也不要在使用req和resp对象做其他操作了

   

### 测试代码

servlet1 请求转发至 servlet2

**servlet1**

```java
@WebServlet(urlPatterns = "/servlet1.do")
public class Servlet1 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("servlet1 service invoked");
        String money = req.getParameter("money");
        System.out.println("money:"+money);
        // 设置响应类型和编码(include模式下)
        /*  resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");*/
        // 增加响应内容
        //resp.getWriter().println("servlet1在转发之前增加的响应内容");
        // 请求转发给另一个组件
        // 获得一个请求转发器
        //RequestDispatcher requestDispatcher = req.getRequestDispatcher("servlet2.do");
        //RequestDispatcher requestDispatcher = req.getRequestDispatcher("aaa.html");
        //RequestDispatcher requestDispatcher = req.getRequestDispatcher("index.jsp");
        //RequestDispatcher requestDispatcher = req.getRequestDispatcher("WEB-INF/bbb.html");
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("https://www.baidu.com");
        // 由请求转发器作出转发动作
        requestDispatcher.forward(req,resp);// 托管给目标资源(forward多一些)
        //requestDispatcher.include(req,resp);  // 让目标资源完成部分工作
        // 继续增加响应信息 (include模式)
        //resp.getWriter().println("servlet1在转发之后增加的响应内容");
       
    }
}

```

**Servlet2**

```java
@WebServlet(urlPatterns = "/servlet2.do")
public class Servlet2 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("servlet2 service invoked");
        // 接收参数
        String money = req.getParameter("money");
        System.out.println("money:"+money);
        // 作出响应 (在forWord模式下)
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");
        PrintWriter writer = resp.getWriter();
        writer.println("支付宝到账:"+money+"元");
    }
}

```



### include（了解）

在include转发模式下,设置响应可以在转发之前,也可以在转发之后

```java
/*servlet1在转发之前增加的响应内容*/
// 设置响应类型和编码(include模式下)
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");
        // 增加响应内容
        resp.getWriter().println("servlet1在转发之前增加的响应内容");

// 获得一个请求转发器
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("servlet2.do");
// 让目标资源完成部分工作
		requestDispatcher.include(req,resp);  

/*servlet1在转发之后增加的响应内容*/
// 继续增加响应信息 (include模式)
		resp.getWriter().println("servlet1在转发之后增加的响应内容");
```

但是不常用的





## 9. senRedirect 响应重定向

响应重定向是通过HttpServletResponse对象sendRedirect(“路径”)的方式。实现是——服务器通知浏览器,让浏览器去自主请求其他资源的一种方式

![image-20210804171130006](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210804171205.png)

### 运作流程：

1. 用户在浏览器端输入特定URL，请求访问服务器端的某个Servlet。
2. 服务器端的Servlet返回一个状态码为302的响应结果，该响应结果的含义为：让浏览器端再请求访问另一个Web资源，在响应结果中提供了另一个Web资源的URL。另一个Web资源有可能在同一个Web服务器上，也有可能不再同一个Web服务器上。
3. 当浏览器端接收到这种响应结果后，再立即自动请求访问另一个Web资源。
4. 浏览器端接收到另一个Web资源的响应结果。

### 测试代码:

**servlet3**

```java
@WebServlet(urlPatterns = "/servlet3.do")
public class Servlet3 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("servlet3 service invoked");
        String money = req.getParameter("money");
        System.out.println("money:"+money);
        // 响应重定向
        resp.sendRedirect("servlet4.do?money="+money);
        //resp.sendRedirect("WEB-INF/bbb.html");
        //resp.sendRedirect("https://www.baidu.com");
    }
}

```

**servlet4**

```java
@WebServlet(urlPatterns = "/servlet4.do")
public class Servlet4 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("servlet4 service invoked");
        String money = req.getParameter("money");
        System.out.println("money:"+money);
    }
}
```

### 注意

1. 重定向是服务器给浏览器重新指定请求方向 是一种**浏览器行为** 地址栏会发生变化
2. 重定向时,请求对象和响应对象都会再次产生,请求中的参数是不会携带
3. 重定向也可以帮助我们完成页面跳转
4. 重定向不能帮助我们访问WEB-INF中的资源
5. 重定向可以定向到外部资源



## 10. 路径问题

### 前端路径

项目结构

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210804210749.png" alt="image-20210804201746315" style="zoom:67%;" />

测试代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--
    base标签的作用是在相对路径之前自动补充base[href]中的值
    如果base标签不写,那么默认就是当前文件所在的路径
    -->
    <base href="http://127.0.0.1:8080/testServlet4_war_exploded/">
    <!--<base href="http://127.0.0.1:8080/testServlet4_war_exploded/a/a2/">-->
</head>
<body>
this is page a1
<br/>
    <!--
    相对(基准)路径:以当前文件本身的位置去定位其他文件,相对自己的路径,以当前文件所在的位置为基准位置
    绝对(基准)路径:以一个固定的位置去定位其他文文件,以一个固定的路径作为定位文件的基准位置,和文件本身位置无关
    相对路径,不以/开头,就是相对路径  ..代表向上一层
    绝对路径,以/开头   在页面上 /代表从项目的部署目录开始找  从webapps中开始找
    页面的绝对路径要有项目名,除非我们的项目没有设置项目名
    -->
    <a href="a2.html" TARGET="_self">相对路径跳转至A2</a>
    <a href="../../b/b2/b1.html" TARGET="_self">相对路径跳转至b1</a>
    <br/>
    <a href="a/a2/a2.html" TARGET="_self">base相对路径跳转至A2</a>
    <a href="b/b2/b1.html" TARGET="_self">base相对路径跳转至b1</a>
    <br/>
    <a href="/testServlet4_war_exploded/a/a2/a2.html" TARGET="_self">绝对路径跳转至A2</a>
    <a href="/testServlet4_war_exploded/b/b2/b1.html" TARGET="_self">绝对路径跳转至b1</a>
</body>
</html>
```



#### 总结

1. 以/开头的路径是绝对路径,不以/开头是相对路径

2. 绝对路径/后面要写当前服务的上下文路径名

3.  ==../==代表向上一层的路径

4. ==base标签==可以简化相对路径,当使用相对路径时,默认会在相对路径之前补充 base中的内容；如果没有定义base 默认就是当前文件所在的路径



 

## 11. 会话管理

### 认识Cookie和Session

Cookie对象与HttpSession对象的作用是维护客户端浏览器与服务端的会话状态的两个对象。由于HTTP协议是一个无状态的协议，所以服务端并不会记录当前客户端浏览器的访问状态，但是在有些时候我们是需要服务端能够记录客户端浏览器的访问状态的，如获取当前客户端浏览器的访问服务端的次数时就需要会话状态的维持。在Servlet中提供了Cookie对象与HttpSession对象用于维护客户端与服务端的会话状态的维持。二者不同的是Cookie是通过客户端浏览器实现会话的维持，而HttpSession是通过服务端来实现会话状态的维持。

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210805102003.png" alt="image-20210805101816416" style="zoom: 50%;" />

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210805102039.png" alt="image-20210805102039111" style="zoom: 50%;" />

### Cookie

Cookie是一种保存少量信息至浏览器的一种技术,第一请求时,服务器可以响应给浏览器一些Cookie信息,第二次请求,浏览器会携带之前的cookie发送给服务器,通过这种机制可以实现在浏览器端保留一些用户信息.为服务端获取用户状态获得依据

#### 特点

- Cookie使用字符串存储数据

- Cookie使用Key与Value结构存储数据

- 单个Cookie存储数据大小限制在4097个字节

- Cookie存储的数据中不支持中文，Servlet4.0中支持

- Cookie是与域名绑定所以不支持跨一级域名访问

- Cookie对象保存在客户端浏览器内存上或系统磁盘中

- Cookie分为持久化Cookie(保存在磁盘上)与状态Cookie(保存在内存上)

- 浏览器在保存同一域名所返回Cookie的数量是有限的。不同浏览器支持的数量不同，Chrome浏览器为50个

- 浏览器每次请求时都会把与当前访问的域名相关的Cookie在请求中提交到服务端。



#### 创建对象和响应

```java
Cookie cookie = new Cookie("key","value")
//通过new关键字创建Cookie对象
response.addCookie(cookie)
//通过HttpServletResponse对象将Cookie写回给客户端浏览器。
```





#### 数据的获取

```java
//通过HttpServletRequest对象获取Cookie，返回Cookie数组。
Cookie[] cookies = request.getCookies()
```





#### Cookie持久化和状态Cookie

- ==状态Cookie==：浏览器会缓存Cookie对象。浏览器关闭后Cookie对象销毁。
- ==持久化Cookie==：浏览器会对Cookie做持久化处理，基于文件形式保存在系统的指定目录中。在Windows10系统中为了安全问题不会显示Cookie中的内容。

​       当Cookie对象创建后**默认为状态Cookie**。可以使用Cookie对象下的==cookie.setMaxAge(60)==方法设置失效时间，单位为秒。一旦设置了失效时间，那么该Cookie为持久化Cookie，浏览器会将Cookie对象持久化到磁盘中。当失效时间到达后文件删除。



#### 测试代码

**通过响应对象 向浏览器响应cookie**

```java
@WebServlet(urlPatterns = "/servlet1.do")
public class Servlet1 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 通过响应对象,向浏览器响应一些Cookie
        Cookie c1=new Cookie("age","10");// 状态Cookie 重启即清除
        Cookie c2=new Cookie("gender", "男");//持久化Cookie 让浏览器保留1分钟
        //c2.setMaxAge(60);// 秒钟    持久化Cookie 让浏览器保留1分钟
        resp.addCookie(c1);
        resp.addCookie(c2);
    }
}
```

**获取请求中cookie**

```java
@WebServlet(urlPatterns = "/servlet2.do")
public class Servlet2 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 读取请求中的Cookie
        Cookie[] cookies = req.getCookies();
        //cookies不为null
        if(null != cookies){
            for (Cookie cookie : cookies) {
                System.out.println(cookie.getName()+"="+cookie.getValue());
            }
        }
    }
}
```

#### 案例：通过Cookie判断用户是否访问过当前Servlet

需求：

​                                                                                                                                                                                      当客户端浏览器第一次访问Servlet时返回“您好，欢迎您第一次访问！”，第二次访问时返回“欢迎您回来！”

```java
@WebServlet(urlPatterns = "/servlet3.do")
public class Servlet3 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 如果是第一访问当前Servlet.向浏览器响应一个cookie ("servlet3","1")
        // 如果是多次访问,就再次数上+1
        Cookie[] cookies = req.getCookies();
        boolean  flag =false ;
        if(null !=cookies){
            for (Cookie cookie : cookies) {
                String cookieName = cookie.getName();
                if(cookieName.equals("servlet3")){
                    // 创建Cookie次数+1
                    Integer value = Integer.parseInt(cookie.getValue())+1;
                    Cookie c=new Cookie("servlet3", String.valueOf(value));
                    resp.addCookie(c);
                    System.out.println("欢迎您第"+value+"访问");
                    flag=true;
                }
            }
        }
        if(!flag){
            System.out.println("欢迎您第一次访问");
            Cookie c=new Cookie("servlet3", "1");
            resp.addCookie(c);
        }
    }
}
```



### Session



### 案例:判断用户是否登录

#### 需求:

实现登录一次即可,在一次会话内,可以反复多次访问WEB-INF/ welcome.html,如果没有登录过,跳转到登录页,登录成功后,可以访问

#### 项目结构:

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210805142941.png" alt="image-20210805142907533" style="zoom:67%;" />

#### 组件介绍：

##### login.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form method="get" action="loginServlet.do">
    用户名:<input type="text" name="username" ><br/>
    密码:<input type="password" name="password" ><br/>
    <input type="submit" >
</form>
</body>
</html>
```



##### main.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
   this is main page
</body>
</html>
```



##### LoginServlet

用来校验登录的，登陆成功将用户信息存户HttpSession，否则返回到登录页。

```java
@WebServlet(urlPatterns = "/loginServlet.do")
public class LoginServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取用户名和密码
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        // 如果用户名和密码为 msb 1234
        if("msb".equals(username)  && "1234".equals(password)){
            // 将用户信息放在HTTPSession中
            User user =new User(null, null, "msb", "1234");
            HttpSession session = req.getSession();
            session.setAttribute("user", user);
            // 登录成功 跳转至 main.html
            resp.sendRedirect(req.getContextPath()+"/mainServlet.do");
        }else{
            // 登录失败 回到login.html
            resp.sendRedirect(req.getContextPath()+"/login.html");
        }
    }
}

```



##### MainServlet

用来向mian.html中跳转的，同时验证登录的，可以直接跳转，否则回到登录页。

```java

@WebServlet(urlPatterns = "/mainServlet.do")
public class MainServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //跳转至main.html
        HttpSession session = req.getSession();
        User user = (User)session.getAttribute("user");
        if(null != user){
            // 判断如果登录过 允许跳转  HTTPSession中如果有登陆过的信息
            req.getRequestDispatcher("/WEB-INF/main.html").forward(req,resp);
        }else{
            // 如果没有登录过 回到登录去登录  HTTPSession中如果有登陆过的信息
            resp.sendRedirect("login.html");
        }
    }
}

```



##### User

存储用户信息的实体类

```java
public class User implements Serializable {
    private Integer uid;
    private String realname;
    private String username;
    private String pasword;
```





# JSP



## 指令标签

三种指令标签

| 指令           | 描述                                                |
| -------------- | --------------------------------------------------- |
| <%@ page %>    | 定义网页依赖属性，如脚本语言，error页面、缓存需求等 |
| <%@ include %> | 包含其他文件                                        |
| <%@ taglib%    | 引入标签库的定义                                    |

### page标签

```java
<%--告知浏览器以什么格式和编码解析 响应的数据--%>

    <%@ page contentType="text/html;charset=UTF-8"  %>

    <%--设置JSP页面转换的语言--%>

    <%@ page language="java"%>

    <%--导包--%>

    <%@ page import="com.msb.entity.User" %>

    <%--在转换成java代码时使用的编码 一般不用设置--%>

    <%@ page pageEncoding="UTF-8" %>

    <%--指定错误页 当页面发生错误时 指定跳转的页面--%>

    <%@ page errorPage="error500.JSP" %>
    <%--指定当前页为异常提示页 当前页面可以接收异常对象 --%>

    <%@page isErrorPage="true" %>
```



errorPage是一种处理错误提示也的功能除了JSP有的错误提示页功能

javaEE中自带其他的错位提示页处理功能，具体配置如下

```xml
<error-page>

    <error-code>500</error-code>

    <location>/error500.JSP</location>

</error-page>



<error-page>

    <error-code>404</error-code>

    <location>/error404.JSP</location>

</error-page>
```

当JSP中发生了异常时,如果JSP中配置的错误页和web.xml 中配置的错误页冲突了,JSP page指令的 errorPage优先级更高

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210807110128.png" alt="image-20210807110126899" style="zoom:67%;" />



### include标签

JSP可以通过Include指令来包含其他文件。被包含的文件可以是JSP文件、HTML文件或文本文件。包含的文件就好像是该JSP文件的一部分，会被同时编译执行。除了include指令标签可以实现引入以外，使用jsp:include也可以实现引入

```jsp

<%--静态引入使用的是 include 指令标签

    被引入的JSP页面不会生成java代码 被引入的网页和当前页生成代码后形成了一个java文件--%>

<%@include file="head.JSP"%>

<%--动态引入 JSP标签中的 include选项

    被引入的JSP页面会生成独立的java代码 

    在生成的java代码中 使用JSPRuntimeLibrary.include(request, response, "head.JSP", out, false);引入其他页面

    --%>

<jsp:include page="head.JSP"/>
```

查看转译以后的java源代码文件中的区别

静态引入：@include被引入的网页和当前页生成代码后形成了一个java文件

动态引入：jsp:include被引入的JSP页面会生成独立的java代码

### taglib指令标签

JSP API允许用户自定义标签，一个自定义标签库就是自定义标签的集合。Taglib指令引入一个自定义标签集合的定义，包括库路径、自定义标签。

语法：

```jsp
<%@ taglib   uri="uri" prefix="prefixOfTag" %>
```



## 内置对象

### 九大对象

**四大域对象**

- pageContext  page域     当前页面内可用

- request       reqeust域  单次请求

- session       session域   单次会话

- application   application 域项目运行

**响应对象**

- response 

**输出流对象**

- out 打印流

**其他三个对象**

- servletConfig:由于JSP本身也是一个Servlet,所以容器也会给我们准备一个ServletConfig

- page        就是他this对象 当前JSP对象本身  

- exception   异常对象,在错误提示页上使用,当isErrorpage=true 才具有该对象

## 案例一：在浏览器上访问Emp表 动态地分等级

EmpDaoImpl.java

实现类

```java
public class EmpDaoImpl implements EmpDao {
    private String url="jdbc:mysql://127.0.0.1:3306/mydb?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai";
    private String username="root";
    private String password="root";
    @Override
    public List<Emp> findAll() {
        List<Emp> list =new ArrayList<>();
        Connection connection =null;
        PreparedStatement pstat=null;
        ResultSet resultSet=null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            connection = DriverManager.getConnection(url, username, password);
            pstat = connection.prepareStatement("select * from emp");
            resultSet = pstat.executeQuery();
            while(resultSet.next()){
                Integer empno=resultSet.getInt("empno");
                Integer deptno=resultSet.getInt("deptno");
                Integer mgr=resultSet.getInt("mgr");
                String ename=resultSet.getString("ename");
                String job=resultSet.getString("job");
                Double sal=resultSet.getDouble("sal");
                Double comm=resultSet.getDouble("comm");
                Date hiredate=resultSet.getDate("hiredate");
                Emp emp =new Emp( empno,  ename,  job,  mgr,  hiredate,  sal,  comm,  deptno);
                list.add(emp);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            if(null!=resultSet){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(null!=pstat){
                try {
                    pstat.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(null!=connection){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }

        return list;
    }
}

```

Emp.java

```java
public interface EmpDao {
    List<Emp> findAll();
}
```



EmpServlet

```java
@WebServlet("/empServlet.do")
public class EmpServlet extends HttpServlet {
    // dao对象
    EmpDao empDao=new EmpDaoImpl();
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取数据
        List<Emp> list = empDao.findAll();
        // 将数据放入请求域
        req.setAttribute("emps", list);
        // 请求转发给JSP
        req.getRequestDispatcher("showEmp.jsp").forward(req,resp);
    }
}
```

showEmp.jsp

```jsp
<%@ page import="java.util.List" %>
<%@ page import="com.msb.pojo.Emp" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <style>
        table{
            border: 3px solid blue;
            width: 80%;
            margin: 0px auto;
        }
        td,th{
            border: 2px solid green;
        }
    </style>
</head>
<body>
    <table cellspacing="0px" cellpadding="0px">
        <tr>
            <th>编号</th>
            <th>姓名</th>
            <th>上级编号</th>
            <th>职务</th>
            <th>入职日期</th>
            <th>薪资</th>
            <th>补助</th>
            <th>部门号</th>
            <th>薪资等级</th>
        </tr>
        <%
            List<Emp> emps = (List<Emp>) request.getAttribute("emps");
            for (Emp emp : emps) {
        %>
            <tr>
                <td><%=emp.getEmpno()%></td>
                <td><%=emp.getEname()%></td>
                <td><%=emp.getMgr()%></td>
                <td><%=emp.getJob()%></td>
                <td><%=emp.getHiredate()%></td>
                <td><%=emp.getSal()%></td>
                <td><%=emp.getComm()%></td>
                <td><%=emp.getDeptno()%></td>
                <td><%--out.print("<td>")--%>
         <%
             Double sal = emp.getSal();
             if(sal<=500){
                 out.print("A");
             }else if( sal <=1000){
                 out.print("B");
             }else if( sal <=1500){
                 out.print("C");
             }else if( sal <=2000){
                 out.print("D");
             }else if( sal <=3000){
                 out.print("E");
             }else if( sal <=4000){
                 out.print("F");
             }else {
                 out.print("G");
             }
         %>
                </td>
            </tr>
        <%
            }
        %>
    </table>
</body>
</html>
```



## EL表达式

Expression Languaga

EL表达式中定义了一些可以帮助我们快捷从域对象中取出数据的写法,**基本语法为**

```jsp
${域标志.数据名.属性名(可选)}
```

**四个域标志关键字分别为**

- requestScope         request域

- sessionScope          session域

- applicationScope   application域

- pageScope             page域

### EL表达式快捷取出域对象

- requestScope         request域

- sessionScope          session域

- applicationScope   application域

- pageScope             page域

```jsp
<%@ page import="com.msb.pojo.User" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>Title</title>
    </head>
    <body>
        <%--向pageContext域中放数据--%>
        <%
        pageContext.setAttribute("msg", "pageContextMessage");
        pageContext.setAttribute("userx", new User(1,"大黄","abcdefg"));
        %>
        <%--
    从域中取出数据
    El表达式在获取对象属性值得时候,是通过对象的属性的get方法获取的
    保证对象的要获取的属性必须有对应get方法才可以
    EL表达式在使用时是不需要import其他类的
    El如果获取的是NULL值,是不展示任何信息的
    --%>
        pageContext域中的数据:<br/>
        msg:${pageScope.msg}<br/>
        username:${pageScope.userx.name}<br/>
        <hr/>
        request域中的数据:<br/>
        msg:${requestScope.msg}<br/>
        username:${requestScope.user.name}<br/>
        <hr/>
        session域中的数据:<br/>
        msg:${sessionScope.msg}<br/>
        username:${sessionScope.users[1].name}<br/>
        <hr/>
        application域中的数据:<br/>
        msg:${applicationScope.msg}<br/>
        username:${applicationScope.userMap.a.name}<br/>
        <hr/>
        <%--EL表达式在取出数据的时候是可以省略域标志的
    EL表达式会自动依次到四个域中去找数据
    --%>
        PageContext username:${userx.name}<br/>
        Request username:${user.name}<br/>
        Session username:${users[1].name}<br/>
        Application username:${userMap.a.name}<br/>
        <hr/>
        <%--
    ${数据的名字}如果省略域标志,取数据的顺序如下
    pageContext
    request
    session
    application
    --%>
        ${msg}
        <hr/>
        <%--
    移除域中的数据
    --%>
        <%
        //pageContext.removeAttribute("msg");// pageContext.removeAttribute()方法会移除四个域中的所有的同名的数据
        //request.removeAttribute("msg");
        %>
        pagecontextMsg:${pageScope.msg}<br/>
        requestMsg:${requestScope.msg}<br/>
        sessionMsg:${sessionScope.msg}<br/>
        applicationMsg:${applicationScope.msg}<br/>
        <hr/>
        <%--
    EL表达式获取请求中的参数
    --%>
        username:${param.username}<br/>
        hobby:${paramValues.hobby[0]}
        hobby:${paramValues.hobby[1]}
    </body>
</html>

```



#### 总结

- EL表达式定义在JSP页面上,在转译之后的java文件中,会被转化成java代码

- EL表达式是一种后台技术,服务器上运行,不是在浏览器上运行,不能用于HTML页面

- EL表达式底层是通过反射实现的,在获取对象属性值时是通过对象的get方法实现的

  

### EL表达式运算符

#### 运算符

**算数运算符**: + - * / %

**比较运算符:** 

==  eq equals

&gt;gt greater then

<     lt   lower then

&gt;=  ge  greater then or equals

<=  le   lower then or equals

!=   ne   not equals

**逻辑运算符**: || or    && and 

**三目运算符**: ${条件 ?表达式1 : 表达式2}

**判空运算符**: empty

#### 使用

```jsp
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%--
+两端如果有字符串,会尝试将字符串转换成数字之后进行加法运算
/如果除以0 结果为Infinity 而不是出现异常
%如果和0取余数,那么会出现异常
--%>
    算数运算符：
    <hr/>
    ${10 + 10}<br/>
    ${"10" + 10}<br/>
    ${"10" + "10"}<br/>
    <%--${"10a" + 10}<br/>--%>
    ${10/0}<br/>
    <%-- ${10%0}<br/>--%>
    关系运算符/比较运算符
    <%--
    比较运算符推荐写成字母形式,不推荐使用 == >=  <=
    --%>
    <hr/>
    ${10 == 10}<br/>
    ${10 eq 10}<br/>
    ${10 gt 8}<br/>
    逻辑运算符
    <hr/>
    ${ true || false}<br/>
    ${ true or false}<br/>
    ${ true && false}<br/>
    ${ true and false}<br/>
    条件运算符/三目运算符
    <hr/>
    ${(100-1)%3==0?10+1:10-1}<br/>
    判断空运算符
    <%--empty 为null 则为true--%>
    <%  //向域中放入数据
        pageContext.setAttribute("a",null);
        pageContext.setAttribute("b","");
        int[] arr ={};
        pageContext.setAttribute("arr",arr);
        List list =new ArrayList();
        pageContext.setAttribute("list",list);
    %>
    <hr/>
    ${empty a}<br/>
    ${empty b}<br/><%--字符串长度为0 则认为是空--%>
    ${empty arr}<br/><%--数组长度为0 认为不是空--%>
    ${empty list}<br/><%--集合长度为0 认为是空--%>
    ${list.size() eq 0}<br/><%--集合长度为0 认为是空--%>
</body>
</html>
```



## 案例一优化：使用EL表达式优化查询员工信息的页面处理

```jsp
<%@ page import="java.util.List" %>
<%@ page import="com.msb.pojo.Emp" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>Title</title>
        <style>
            table{
                border: 3px solid blue;
                width: 80%;
                margin: 0px auto;
            }
            td,th{
                border: 2px solid green;
            }
        </style>
    </head>
    <body>
        <table cellspacing="0px" cellpadding="0px">
            <tr>
                <th>编号</th>
                <th>姓名</th>
                <th>上级编号</th>
                <th>职务</th>
                <th>入职日期</th>
                <th>薪资</th>
                <th>补助</th>
                <th>部门号</th>
                <th>薪资等级</th>
            </tr>
            <%
            List<Emp> emps = (List<Emp>) request.getAttribute("emps");
            for (Emp emp : emps) {
                pageContext.setAttribute("emp", emp);//将员工对象放入PageContext 域
                %>
            <tr>
                <td>${emp.empno}</td>
                <td>${emp.ename}</td>
                <td>${emp.mgr}</td>
                <td>${emp.job}</td>
                <td>${emp.hiredate}</td>
                <td>${emp.sal}</td>
                <td>${emp.comm}</td>
                <td>${emp.deptno}</td>
                <td>
                    ${emp.sal le 500?"A":""}
                    ${emp.sal gt 500 and emp.sal le 1000?"B":""}
                    ${emp.sal gt 1000 and emp.sal le 1500?"C":""}
                    ${emp.sal gt 1500 and emp.sal le 2000?"D":""}
                    ${emp.sal gt 2000 and emp.sal le 3000?"E":""}
                    ${emp.sal gt 3000 and emp.sal le 4000?"F":""}
                    ${emp.sal gt 4000?"G":""}
                </td>
            </tr>
            <%
            }
            %>
        </table>
    </body>
</html>

```

## JTSL

JSTL（Java server pages standarded tag library，即JSP标准标签库）是由JCP（Java community Proces）所制定的标准规范，它主要提供给Java Web开发人员一个标准通用的标签库，并由Apache的Jakarta小组来维护。

**使用前提**

1. 需要导包 

2. 页面中通过taglib指令引入标签库

   ```jsp
   <%@   taglib uri="标签库的定位" prefix="前缀(简称)" %>
   ```

   uri可以在对应的tld文件中找到

### 核心标签库

导入语句为

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
```

#### 操作对象的标签c:set/out/remove

- ==&lt;c:set>==         向域对象中放入数据  setAttribute
- ==&lt;c:out>==        从域对象中取出数据  getAttribute
- ==&lt;c:remove>== 从域对象中移除数据   removeAttribute

```jsp
<%--
    c:set
        scope 指定放数据的域 可选值 page request session application
        var   数据的名称
        value 数据
    --%>
    <c:set scope="page" var="msg" value="pageMessage"></c:set>
    <c:set scope="request" var="msg" value="requestMessage"></c:set>
    <c:set scope="session" var="msg" value="sessionMessage"></c:set>
    <c:set scope="application" var="msg" value="applicationMessage"></c:set>
    <%--移除指定域中的值--%>
   <%-- <c:remove var="msg" scope="page"></c:remove>
    <c:remove var="msg" scope="request"></c:remove>--%>
    <c:remove var="msg" scope="session"></c:remove>
    <c:remove var="msg" scope="application"></c:remove>
    <%--通过EL表达式取出域中的值--%>
    <hr/>
    ${pageScope.msg}<br/>
    ${requestScope.msg}<br/>
    ${sessionScope.msg}<br/>
    ${applicationScope.msg }<br/>
    <hr/>


    <%--通过c:out标签获取域中的值--%>
    <c:out value="${pageScope.msg}" default="page msg not found"/>
    <c:out value="${requestScope.msg}" default="request msg not found"/>
    <c:out value="${sessionScope.msg}" default="session msg not found"/>
    <c:out value="${applicationScope.msg}" default="application msg not found"/>
</body>
</html>

```

#### 多条件分支标签c:if和c:choose

```jsp
<%--
    随机生成一个分数  0-100
    >=90 A
    >=80 B
    >=70 C
    >=60 D
    <60  E
    --%>
    <%
        int score =new Random().nextInt(101);
        pageContext.setAttribute("score", score);
    %>
    <%--
    test  判断条件
    c:if可以将test的结果放入指定的域中
    scope 指定存放的域
    var   数据名
    --%>
    分数:${score}<br/> 等级:
    <c:if test="${score ge 90}" scope="page" var="f1">A</c:if>
    <c:if test="${score ge 80 and score lt 90}" scope="page" var="f2">B</c:if>
    <c:if test="${score ge 70 and score lt 80}" scope="page" var="f3">C</c:if>
    <c:if test="${score ge 60 and score lt 70}" scope="page" var="f4">D</c:if>
    <c:if test="${score lt 60}" scope="page" var="f5">E</c:if>
    <hr/>
    ${f1}
    ${f2}
    ${f3}
    ${f4}
    ${f5}
    <hr/>
    <%--输出分数是否及格--%>
    <c:if test="${score ge 60}" scope="page" var="flag">及格</c:if>
    <c:if test="${!pageScope.flag}">不及格</c:if>
    <hr/>
    <c:choose>
        <c:when test="${score ge 90}">A</c:when>
        <c:when test="${score ge 80}">B</c:when>
        <c:when test="${score ge 70}">C</c:when>
        <c:when test="${score ge 60}">D</c:when>
        <c:otherwise>E</c:otherwise>
    </c:choose>
```

#### 迭代标签c:foreach

##### 打印乘法表

c:forEach中的**属性**

- ==var==: 迭代变量, 存放在pageContext作用域
- ==begin==: 迭代起始值
- ==end==: 迭代结束值
- ==step==: 迭代变量变化的步长

```jsp
<%--
    for ( int i =1;i<=9 ;i+=2){
        pageContext.setAttribute("i",i)
    }
c:foreach 每次执时都会向page域中放入一个名为 i 值为当前值这样的一个操作
    --%>
<c:forEach var="i" begin="1" end="9" step="1">
    <c:forEach var="j" begin="1" end="${i}" step="1">
        ${j} * ${i} = ${i*j} &nbsp;
    </c:forEach>
    <br/>
</c:forEach>
```

##### 遍历对象数组

- ==items==: 要遍历的集合, 需要使用EL表达式取值
- ==varStatus==: 迭代变量的状态
- ==index:== 索引, 从0开始
- ==count:== 计数, 从1开始
- ==first==: boolean, 表示是否是第一个
- ==last==: boolean, 表示是否是最后一个
- ==current==: 对象, 当前迭代的对象值



```jsp
 <%--<%//原来的遍历
        List<Emp> emps = (List<Emp>) request.getAttribute("emps");
        for (Emp emp : emps) {
            pageContext.setAttribute("emp", emp);//将员工对象放入PageContext 域
        %>
        c:foreach
        --%>
        <c:forEach items="${emps}" var="emp" varStatus="empStatus">
```



### 格式化标签库fmt

#### 导入标签

```jsp
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
```

#### &lt;fmt:formatDate>日期格式标签

```jsp
<td>
    <fmt:formatDate value="${emp.hiredate}" pattern="yyyy年MM月dd日 HH:mm:ss"/>
</td>
```

#### 数字格式化标签

```jsp
<td>
    <%--
    0 代表必须有一位数字,如果对应的位置没有值怎么办?自动补充0
    # 代表有一位数字,开头和结尾的所有的0不保留
    --%>
    &yen;<fmt:formatNumber value="${emp.sal}" pattern="###,##0.00"/>
</td>
```



## showEmp.js页面最终优化

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<html>
    <head>
        <title>Title</title>
        <style>
            table{
                border: 3px solid blue;
                width: 80%;
                margin: 0px auto;
            }
            td,th{
                border: 2px solid green;
            }
        </style>
    </head>
    <body>
        <table cellspacing="0px" cellpadding="0px">
            <tr>
                <th>序号</th>
                <th>索引</th>
                <th>isFirst</th>
                <th>isLast</th>
                <th>编号</th>
                <th>姓名</th>
                <th>姓名</th>
                <th>上级编号</th>
                <th>职务</th>
                <th>入职日期</th>
                <th>薪资</th>
                <th>补助</th>
                <th>部门号</th>
                <th>薪资等级</th>
            </tr>
            <%-- <%
    List<Emp> emps = (List<Emp>) request.getAttribute("emps");
            for (Emp emp : emps) {
                pageContext.setAttribute("emp",emp);//将员工对象放入PageContext域
                %>
            c:foreach
            items 要遍历的数组/List  可以通过EL表达式取出集合之后给改属性赋值
            var   中间变量的名称
            varStatus 记录每一个对象状态的设置
            count 个数
            index 索引
            first 如果当前元素是迭代的第一个元素 true 否则为false
            last  如果当前元素是迭代的最后一个元素 true 否则为false
            current 当前迭代的元素本身
            --%>
            <c:forEach items="${emps}" var="emp" varStatus="empStatus">
                <tr>
                    <%--使用EL表达式来取出域对象里的对象属性值--%>
                    <%-- <td><%=emp.getEmpno()%></td>
            <td><%=emp.getEname()%></td>
            <td><%=emp.getMgr()%></td>
            <td><%=emp.getJob()%></td>
            <td><%=emp.getHiredate()%></td>
            <td><%=emp.getSal()%></td>
            <td><%=emp.getComm()%></td>
            <td><%=emp.getDeptno()%></td>--%>
            <td>${empStatus.count}</td>
            <td>${empStatus.index}</td>
            <td>${empStatus.first}</td>
            <td>${empStatus.last}</td>
            <td>${emp.empno}</td>
            <td>${emp.ename}</td>
            <td>${empStatus.current.ename}</td>
            <td>${emp.mgr}</td>
            <td>${emp.job}</td>
            <td>
                <fmt:formatDate value="${emp.hiredate}" pattern="yyyy年MM月dd日 HH:mm:ss"/>
            </td>
            <td>
                <%--
    0 代表必须有一位数字,如果对应的位置没有值怎么办?自动补充0
    # 代表有一位数字,开头和结尾的所有的0不保留
    --%>

                &yen;<fmt:formatNumber value="${emp.sal}" pattern="###,##0.00"/>
            </td>
            <td>${emp.comm}</td>
            <td>${emp.deptno}</td>
            <td><%--out.print("<td>")--%>
                <%--<%
    	Double sal = emp.getSal();
                if(sal<=500){
                    out.print("A");
                }else if( sal <=1000){
                    out.print("B");
                }else if( sal <=1500){
                    out.print("C");
                }else if( sal <=2000){
                    out.print("D");
                }else if( sal <=3000){
                    out.print("E");
                }else if( sal <=4000){
                    out.print("F");
                }else {
                    out.print("G");
                }
                %>--%>
                <%--使用EL算数表达式来判断等级
    ${emp.sal le 500?"A":""}
                ${emp.sal gt 500 and emp.sal le 1000?"B":""}
                ${emp.sal gt 1000 and emp.sal le 1500?"C":""}
                ${emp.sal gt 1500 and emp.sal le 2000?"D":""}
                ${emp.sal gt 2000 and emp.sal le 3000?"E":""}
                ${emp.sal gt 3000 and emp.sal le 4000?"F":""}
                ${emp.sal gt 4000?"G":""}--%>

                <%--使用JSTL标签--%>
                <c:choose>
                    <c:when test="${emp.sal le 500}">A</c:when>
                    <c:when test="${emp.sal le 1000}">B</c:when>
                    <c:when test="${emp.sal le 1500}">C</c:when>
                    <c:when test="${emp.sal le 2000}">D</c:when>
                    <c:when test="${emp.sal le 3000}">E</c:when>
                    <c:when test="${emp.sal le 4000}">F</c:when>
                    <c:when test="${emp.sal gt 4000}">G</c:when>
                </c:choose>
            </td>
            </tr>
        </c:forEach>
    </table>
</body>
</html>

```



# Filter

## 案例：通过过滤验证登录

需求：通过过滤器控制，只有登陆之后可以反复进入welcome.jsp欢迎页，如果没有登录，提示用户进入登录页进行登陆操作。

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210808090141.png" alt="image-20210808090117141" style="zoom:50%;" />

###### login.jsp

```jsp

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title%sSourceCode%lt;/title>
  </head>
  <body>
  <img src="static/img/logo.png">
  please login ... ... <br/>
  <form action="loginController.do" method="post">
    用户名:<input type="text" name="user"> <br/>
    密码:<input type="password" name="pwd"><br/>
    <input type="submit" value="提交">
  </form>
  </body>
</html>

```



###### welcome.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<img src="static/img/logo.png">
欢迎${user.username}登陆!!!
</body>
</html>

```

###### aaa.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
this is page aaa
</body>
</html>
```

准备Controller代码

```java
@WebServlet(urlPatterns = "/loginController.do")
public class LoginController extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取用户名和密码
        String username = req.getParameter("user");
        String password = req.getParameter("pwd");
        System.out.println(username);
        System.out.println(password);
        // 链接数据库校验登录
        // 登录成功,将用户信息放入Session域
        User user =new User(username,password);
        req.getSession().setAttribute("user", user);
        // 跳转到欢迎页
        resp.sendRedirect("welcome.jsp");
    }
}

```

准备登录控制过滤器

```java
@WebFilter(urlPatterns = "/*")// 任何资源都要进行过滤,
public class Filter1_LoginFilter  implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest req=(HttpServletRequest)servletRequest;
        HttpServletResponse resp=(HttpServletResponse) servletResponse;
        //无论是否登录过,都要放行的资源   登录页  登录校验Controller 和一些静态资源
        String requestURI = req.getRequestURI();
        System.out.println(requestURI);
        if(requestURI.contains("login.jsp")|| requestURI.contains("loginController.do")|| requestURI.contains("/static/")){
            // 直接放行
            filterChain.doFilter(req,resp);
            // 后续代码不再执行
            return;
        }
        // 需要登录之后才能访问的资源,如果没登录,重定向到login.jsp上,提示用户进行登录
        HttpSession session = req.getSession();
        Object user = session.getAttribute("user");
        if(null != user){// 如果登录过 放行
            filterChain.doFilter(req,resp);
        }else{// 没有登录过,跳转至登录页
            resp.sendRedirect("login.jsp");
        }
    }
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }
    @Override
    public void destroy() {
    }
}

```



# Listener



## 案例：记录请求日志

###### RequestLoginListener.java

```java
@WebListener
public class RequestLogListener implements ServletRequestListener {
    private SimpleDateFormat simpleDateFormat=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    @Override
    public void requestDestroyed(ServletRequestEvent sre) {
    }
    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        // 获得请求发出的IP
        // 获得请求的URL
        // 获得请求产生的时间
        HttpServletRequest request = (HttpServletRequest)sre.getServletRequest();
        String remoteHost = request.getRemoteHost();
        String requestURL = request.getRequestURL().toString();
        String reqquestDate = simpleDateFormat.format(new Date());
        // 准备输出流
        try {
            PrintWriter pw =new PrintWriter(new FileOutputStream(new File("d:/msb.txt"),true));
            pw.println(remoteHost+" "+requestURL+" "+reqquestDate );
            pw.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}

```



## 案例：显示在线人数

**需求**：开启一次会话session 在线人数加一 销毁会话以后在线人数减一

使用count来计数，然后**存在application域中**

###### OnlineNumberListener:

获取application域对象，存入数据count，如果一次session开启，++count；一次session关闭，--count

```java
@WebListener
public class OnlineNumberListener implements HttpSessionListener {
    @Override
    public void sessionCreated(HttpSessionEvent se) {
        //向application域中增加一个数字
        HttpSession session = se.getSession();
        ServletContext application = session.getServletContext();
        Object count = application.getAttribute("count");
        if (null == count) {
            //第一次放入数据
            application.setAttribute("count",1);
        }else{
            int c = (int) count;
            application.setAttribute("count", ++c);
        }
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        //向application域中减少一个数字
        HttpSession session = se.getSession();
        ServletContext application = session.getServletContext();
        Object count = application.getAttribute("count");
        int count1 = (int) count;
        application.setAttribute("count",--count1);

    }
}

```

## 案例：重启免登录

### Session序列化和反序列化

1、序列化与反序列

把对象转化为字节序列的过程称为序列化（保存到硬盘，持久化）

把字节序列转化为对象的过程称为反序列化（存放于内存）

 2、序列化的用途

把对象的字节序列永久保存到硬盘上，通常放到一个文件中。

把网络传输的对象通过字节序列化，方便传输本节作业

3、实现步骤

想实现序列化和反序列化需要手动配置

![image-20210808154350410](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210808154352.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>

<Context>

    <Manager className="org.apache.catalina.session.PersistentManager">

        <Store className="org.apache.catalina.session.FileStore" directory="d:/session"/>

    </Manager>

</Context>
```

==注意实体类必须实现serializable 接口==

### 开发过程

#### 1 准备实体类

```java

public class User  implements Serializable {
    private String username;
    private String pwd;
}
```



#### 2 开发登录信息输入页面

```jsp
  <form action="loginController.do" method="post">
    用户名:<input type="text" name="user"> <br/>
    密码:<input type="password" name="pwd"><br/>
    <input type="submit" value="提交">
  </form>
```



#### 3开发登录信息验证Servlet

```java
@WebServlet("/loginController.do")
public class LoginController extends HttpServlet {
@Override
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    String username = req.getParameter("user");
    String pwd = req.getParameter("pwd");
    // user
    User user =new User(username,pwd);
    // session
    HttpSession session = req.getSession();
    session.setAttribute("user", user);
}
}
```



#### 4 开发校验当前是否已经登录的Controller

loginCheckController.java

```java

@WebServlet(urlPatterns = "/loginCheckController.do")
public class LoginCheckController extends HttpServlet {
@Override
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 判断是否登录
    HttpSession session = req.getSession();
    Object user = session.getAttribute("user");
    Object listener = session.getAttribute("listener");// 获得对应的监听器
    String message ="";
    if(null != user){
        message="您已经登录过";
    }else{
        message="您还未登录";
    }
    resp.setCharacterEncoding("UTF-8");
    resp.setContentType("text/html;charset=UTF-8");
    resp.getWriter().println(message);
}
}
```






#### 5  测试

先登录,然后请求loginCheckController.do 校验是否登录过,然后重启项目,再起请求loginCheckController.do 校验是否登录过,发现重启后,仍然是登录过的

#### 6 监听钝化和活化

**MySessionActivationListener.java**

```java
public class MySessionActivationListener implements HttpSessionActivationListener, Serializable {
@Override
public void sessionWillPassivate(HttpSessionEvent se) {
    System.out.println(se.getSession().hashCode()+"即将钝化");
}
@Override
public void sessionDidActivate(HttpSessionEvent se) {
    System.out.println(se.getSession().hashCode()+"已经活化");
}
}
```




**LoginController登录时绑定监听器**

```java
 @WebServlet("/loginController.do")
public class LoginController extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("user");
        String pwd = req.getParameter("pwd");
        // user
        User user =new User(username,pwd);
        // session
        HttpSession session = req.getSession();
        session.setAttribute("user", user);
        // 绑定监听器
        session.setAttribute("listener", new MySessionActivationListener());
    }
}
```



重启项目 重复测试即可

![image-20210808161834961](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210808161851.png)

![image-20210808161823274](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210808161855.png)



