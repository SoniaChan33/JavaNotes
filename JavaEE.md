# 引入

​                                                           

![image-20210731155304853](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210731155304.png)



# Tomcat

## 结构

## 部署项目

## 常见配置

## 主要组件

## HTTP协议

协议：Protocol

应用层

传输层

网络层

数据链路成



超文本协议

超级文本窗户上西医



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

### ServletContext

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
