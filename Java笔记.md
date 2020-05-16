----

# 【1】Cookie 和 Session

## 会话跟踪技术

### 什么是会话跟踪技术

HTTP是一种无状态协议，每当用户发出请求时，服务器就会做出响应，客户端与服务器之间的联系是离散的、非连续的。当用户在同一网站的多个页面之间转换时，根本无法确定是否是同一个客户，会话跟踪技术就可以解决这个问题。当一个客户在多个页面间切换时，服务器会保存该用户的信息。

### 4种方法实现

**1.隐藏表单域**

```html
<input type="hidden" name ="session" value="..."/>
```

**2.URL 重写**

如果浏览器禁用了cookie，URL 可以在后面附加参数，和服务器的请求一起发送，这些参数为名字/值对

```html
<a href='/day06_5/index.jsp;jsessionid=<%=session.getId() %>'  >主页</a>
```

**3.Cookie**

**4.Session**

## Cookie

![img](\img\clip_image002.jpg)

#### 规范

- Cookie大小上限为 4 KB；

- 一个服务器最多在客户端浏览器上保存 20 个 Cookie；

- 一个浏览器最多保存 300 个 Cookie；

#### 方法

- setMaxAge

  - -1：没有时效
  - 60：代表60秒
  - 0：表示cookie作废

- setPath

  - `setPath("/")`服务器下所有应用共享cookie，`setPath("/AAA")`是指设置的cookie只能在AAA应用下的获得
  - 如果不设置，访问`http://localhost/cookietest/AAA`时添加的Cookie默认路径为`/cookietest`

- setDomain

  - 如设置为`.baidu.com`，不论前缀如何，都可以共享Cookie 

```java
   //示例代码
	Cookie cookie = new Cookie("id", "baidu");
	cookie.setPath("/");
	cookie.setDomain(".baidu.com");
	cookie.setMaxAge(60*60);
	response.addCookie(cookie);
```

## Session

#### ![img](img\clip_image002-1588511232807.jpg)概述

- `javax.servlet.http.HttpSession`接口表示一个会话，我们可以把一个会话内需要共享的数据保存到HttSession 对象中

- ` HttpSession request.getSesssion()`：如果当前会话已经有了 session 对象那么直接返回，如果当前会话还不存在会话，那么创建 session 并返回

#### 方法

- void setAttribute(String name, Object value)
- Object getAttribute(String name)
- void removeAttribute(String name)
- void removeAttribute(String name)
- void invalidate()
- String getId()
- int getMaxInactiveInterval()：获取session可以的最大不活动时间（秒），默认为30分钟。当session在30分钟内没有使用，那么Tomcat会在session池中移除这个session
- void setMaxInactiveInterval(int interval)
- long getCreationTime()：返回session的创建时间，返回值为当前时间的毫秒值；
- long getLastAccessedTime()：返回session的最后活动时间，返回值为当前时间的毫秒值
- boolean isNew()：查看session是否为新。当客户端第一次请求时，服务器为客户端创建session，但这时服务器还没有响应客户端，也就是还没有把sessionId响应给客户端时，这时session的状态为新



#### 浏览器中 JSESSIONID 的创建时间

如果客户端请求的cookie中不包含 JSESSIONID，服务端调用 request.getSession() 时就会生成并传递给客户端，此次响应头会包含`Set-Cookie: JSESSIONID=XXX `的信息



----



# 【2】四大域对象

## 域方法

- void setAttribute(String name, Object value)
- Object getAttribute(String name)
- void removeAttribute(String name)
- void removeAttribute(String name)

## Request

HttpServletRequest：：一个请求创建一个request对象，所以在同一个请求中可以共享request，例如一个请求从AServlet转发到BServlet，那么AServlet和BServlet可以共享request域中的数据；

### 方法

- 域方法略
- String getHeader(String name)：获取指定名称的请求头；
- lString getContentType()：获取请求类型，如果请求是GET，那么这个方法返回null；如果是POST请求，那么默认为application/x-www-form-urlencoded，表示请求体内容使用了URL编码；
- String getMethod()：返回请求方法，例如：GET
- Locale getLocale()：返回当前客户端浏览器的Locale。java.util.Locale表示国家和言语，这个东西在国际化中很有用；

![](img\image-20200503212956137.png)

### 请求转发和请求包含

```java
RequestDispatcher rd = request.getRequestDispatcher("/BServlet"); 
rd.forward(request, response) ;//转发
rd.include(request, response);//包含
```

![img](img\clip_image002-1588512893899.jpg)

### **请求转发与重定向对比**

- 请求转发是一个请求，而重定向是两个请求；
- 请求转发后浏览器地址栏不会有变化，而重定向会有变化，因为重定向是两个请求；
- 请求转发的目标只能是本应用中的资源，重定向的目标可以是其他应用；
- 请求转发对 AServlet 和 BServlet 的请求方法是相同的，即要么都是GET，要么都是POST，因为请求转发是一个请求；
- 重定向的第二个请求一定是GET；

ServletContext：//TODO

HttpSession：略

PageContext：JSP 略

# 【3】Response

response对象的功能分为以下四种：

- 设置响应头信息；
- 设置状态码；
- 设置响应正文；
- 重定向；

## 重定向

当你访问 http://www.sun.com 时，你会发现浏览器地址栏中的URL会变成 http://www.oracle.com/us/sun/index.htm，这就是重定向了。

重定向是服务器通知浏览器去访问另一个地址，即再发出另一个请求。

![img](img\20190628175233843.png)

# 【4】Model和 ModelAndView

- **Model**

  - Model 或者 ModelMap 通过 addAttribute 方法向页面传递参数.

  - Model是一个接口，其实现类为ExtendedModelMap，继承了ModelMap类

```java
model.addAttribute("username","张三");
return "success";
```

- **ModelAndView**

```java
ModelAndView mv = new ModelAndView();
mv.addObject("username", "张三");
mv.setViewName("success");
return mv;
```

- **Model 与 ModelAndView 的最大区别**
  - Model 只是用来传输数据的，并不会进行业务的寻址。ModelAndView 则可以进行业务寻址，即可以设置对应的要请求的静态文件（jsp等）。
  - Model 是每次请求可以自动创建的，而 ModelAndView 是需要自行创建的。

