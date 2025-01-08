# JavaEE

JavaEE：Java Enterprise Edition, Java企业版。指Java企业级开发的技术规范总和。包含13项技术规范：JDBC、JNDI、EJB、RMI、JSP、Servlet、XML、JMS、Java IDL、JTS、JTA、JavaMail、JAF

# Servlet继承结构

Servlet(ServletRequest,ServletResponse)

GenericServlet

HttpServlet(HttpServletRequest,HttpServletResponse)

# urlPattern配置规则

优先级：精确路径>目录路径>扩展名路径> /* > /
- 精确匹配：@WebServlet("/user/select")
- 目录匹配：@WebServlet("/user/*")
- 扩展名匹配(不能加/)：@WebServlet("*.do")
- 任意匹配(不建议使用)：@WebServlet("/") 或 @WebServlet("/*") 
  - 当我们的项目中的Servlet配置了"/"，会覆盖tomcat中DefaultServlet(处理所有的静态资源,当其他的urlPattern匹配不上时会走这个Servlet)
  - 当我们的项目中配置了"/*"，意味着访问任意路径
  

# Request获取请求数据

## 请求行

```
GET /request-demo/req1?username=zhangshan HTTP/1.1
```

```
String getMethod(): GET
String getContextPath()：/request-demo
StringBuffer getRequestURL()：http://localhost:8080/request-demo/req1
String getReqeustURI()：/request-demo/req1
String getQueryString()：username=zhangshan (GET方式)
```

## 请求头

```
Connection: keep-alive
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36
```

```
String getHeader(String name)
```

## 请求体

```
username=abc&password=123
```

```
ServletInputStream getInputStream()
BufferedReader getReader()
```

## 通用方式获取请求参数

```
# 获取所有参数Map集合
Map<String,String[]> getParameterMap()

# 根据名称获取参数值(数组)
String[] getParameterValues(String name)

# 根据名称获取参数值(单个)
String getParameter(String name)
```

## 请求转发

```
request.getRequestDispatcher("/path").forward(request,response)
```

## 中文乱码

```
# POST请求
request.setCharacterEncoding("UTF-8"); // 设置字符输入流的编码

# GET请求
String username = req.getParameter("username");
username = new String(username.getBytes(StandardCharsets.ISO_8859_1), StandardCharsets.UTF_8);
```

# Response设置响应数据

## 响应行

```
HTTP/1.1 200 OK
```

```
void setStatus(int sc)
```

## 响应头

```
Content-Tyle:text/html
```

```
void setHeader(String name,String value)
```

## 响应体

```
PrintWriter getWriter()
ServletOutputStream getOutputStream()
```

## 重定向

```
# 方式一
response.setStatus(302)
response.setHeader("localtion","资源路径")

# 方式二
response.sendRedirect("资源路径")
```

## 响应编码

```
response.setCharacterEncoding("UTF-8");
response.setContentType("application/json");
```

# JSP

```
jsp本质就是一个Servlet，间接继承自HttpServlet
```

# EL表达式

```
${brands}	获取域中存储的key为brands的数据
```

JavaWeb中四大域对象：

- page：当前页面有效
- request：当前请求有效
- session：当前会话有效
- applicaiton：当前应用有效

##  JSTL

Jsp Standarded Tag Library：使用标签取代JSP页面上的Java代码

# 三层架构

- 表现层(Servlet、JSP)：SpringMVC
- 业务逻辑层(JavaBean)：Spring
- 数据访问层(JavaBean)：MyBatis

# 客户端会话跟踪技术(Cookie)

```
# 发送Cookie
# 响应头：set-cookie
Cookie cookie = new Cookie("key","value")
response.addCookie(cookie)

# 获取Cookie
# 请求头：cookie
Cookie[] cookies = request.getCookies()
cookie.getName()
cookie.getValue()

默认情况下，Cookie存储在浏览器内存中，当浏览器关闭，内存释放，则Cookie被销毁
cookie.setMaxAge(int seconds)
	正数：持久化存储，到期自动删除
	负数：默认值,浏览器自动关闭，则Cookie销毁
	0：删除对应Cookie
Cookie不支持存储中午，可以将中文转换成URL编码的形式
```

# 服务端会话跟踪技术(Session)

```
HttpSession session = request.getSesssion()
void setAttribute(String name,OBject o)
Object getAttribute(String name)
void removeAttribute(String name)

Session的实现基于Cookie，会把SessionId通过Cookie的方式发送到客户端
set-cookie:JSESSIONID=10
cookie:JSESSIONID=10

Session钝化：在服务器正常关闭后，Tomcat会自动将Session数据写入硬盘文件中
Session活化：再次启动服务器后，从文件中加载数据到Session中

默认情况下，无操作，30分钟Session自动销毁,也可以调用session.invalidate()手动销毁
<session-config>
	<session-timeout>100</session-timeout>
</session-config>
```

Cookie最大3KB，Session大小无限制

# Filter

Filter执行顺序：

- 注解配置的Filter，优先级按照过滤器类名(字符串)的自然排序

# Listener

监听器可以监听在application、session、request三个对象创建、销毁或者往其中添加修改删除属性时自动执行代码的功能主键。

ServletContext(application)监听

- ServletContextListener：对象创建和销毁进行监听
- ServletContextAttributeListener：对象增删改属性监听

Session(session)监听

- HttpSessionListener：对象创建和销毁进行监听
- HttpSessionAttributeListener：对象增删改属性监听
- HttpSessionBindingListener：监听对象于Session的绑定和解除
- HttpSessionActivationListener：对象钝化和活化监听

Request(request)监听

- ServletRequestListener：对象创建和销毁进行监听
- ServletRequestAttributeListener：对象增删改属性监听
