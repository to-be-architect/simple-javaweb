# [ 成为架构师系列  ] 1. 第一个 Java Web 程序
---

# 1.新建 maven webapp 工程

打开 idea, New Project, 选择 Maven, 从 maven-archetype 创建, 找到 maven-archetype-webapp:

![](https://upload-images.jianshu.io/upload_images/1233356-48b524772239dd66.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

创建 maven 项目


` -DgroupId=com.light.sword -DartifactId=simple-javaweb -Dversion=1.0-SNAPSHOT -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=RELEASE org.apache.maven.plugins:maven-archetype-plugin:RELEASE:generate`

得到项目文件目录如下

```
$ tree
.
├── pom.xml
├── simplejavaweb.iml
└── src
    └── main
        └── webapp
            ├── WEB-INF
            │   └── web.xml
            └── index.jsp

4 directories, 4 files

```

# 2. maven-archetype-webapp 是什么?

Maven Webapp Archetype:
[http://maven.apache.org/archetypes/maven-archetype-webapp/](http://maven.apache.org/archetypes/maven-archetype-webapp/)

### Maven Webapp Archetype
maven-archetype-webapp is an archetype which generates a sample Maven webapp project:

```
project
|-- pom.xml
`-- src
    `-- main
        `-- webapp
            |-- WEB-INF
            |   `-- web.xml
            `-- index.jsp
```

### Usage
To generate a new project from this archetype, type:
```
mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=1.4
 ```
# 3. Servlet 是什么?

Servlet 本质上就是一个java类:

* A servlet is a small Java program that runs within a Web server. 
* Servlets receive and respond to requests from Web clients, usually across HTTP, the
HyperText Transfer Protocol.

![](https://upload-images.jianshu.io/upload_images/1233356-a5944447b771629a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
/**
 * Defines methods that all servlets must implement.
 *
 * <p>
 * A servlet is a small Java program that runs within a Web server. Servlets
 * receive and respond to requests from Web clients, usually across HTTP, the
 * HyperText Transfer Protocol.
 *
 * <p>
 * To implement this interface, you can write a generic servlet that extends
 * <code>javax.servlet.GenericServlet</code> or an HTTP servlet that extends
 * <code>javax.servlet.http.HttpServlet</code>.
 *
 * <p>
 * This interface defines methods to initialize a servlet, to service requests,
 * and to remove a servlet from the server. These are known as life-cycle
 * methods and are called in the following sequence:
 * <ol>
 * <li>The servlet is constructed, then initialized with the <code>init</code>
 * method.
 * <li>Any calls from clients to the <code>service</code> method are handled.
 * <li>The servlet is taken out of service, then destroyed with the
 * <code>destroy</code> method, then garbage collected and finalized.
 * </ol>
 *
 * <p>
 * In addition to the life-cycle methods, this interface provides the
 * <code>getServletConfig</code> method, which the servlet can use to get any
 * startup information, and the <code>getServletInfo</code> method, which allows
 * the servlet to return basic information about itself, such as author,
 * version, and copyright.
 *
 * @see GenericServlet
 * @see javax.servlet.http.HttpServlet
 */

public interface Servlet {

    /**
     * Called by the servlet container to indicate to a servlet that the servlet
     * is being placed into service.
     *
     * <p>
     * The servlet container calls the <code>init</code> method exactly once
     * after instantiating the servlet. The <code>init</code> method must
     * complete successfully before the servlet can receive any requests.
     *
     * <p>
     * The servlet container cannot place the servlet into service if the
     * <code>init</code> method
     * <ol>
     * <li>Throws a <code>ServletException</code>
     * <li>Does not return within a time period defined by the Web server
     * </ol>
     *
     *
     * @param config
     *            a <code>ServletConfig</code> object containing the servlet's
     *            configuration and initialization parameters
     *
     * @exception ServletException
     *                if an exception has occurred that interferes with the
     *                servlet's normal operation
     *
     * @see UnavailableException
     * @see #getServletConfig
     */
    public void init(ServletConfig config) throws ServletException;

    /**
     *
     * Returns a {@link ServletConfig} object, which contains initialization and
     * startup parameters for this servlet. The <code>ServletConfig</code>
     * object returned is the one passed to the <code>init</code> method.
     *
     * <p>
     * Implementations of this interface are responsible for storing the
     * <code>ServletConfig</code> object so that this method can return it. The
     * {@link GenericServlet} class, which implements this interface, already
     * does this.
     *
     * @return the <code>ServletConfig</code> object that initializes this
     *         servlet
     *
     * @see #init
     */
    public ServletConfig getServletConfig();

    /**
     * Called by the servlet container to allow the servlet to respond to a
     * request.
     *
     * <p>
     * This method is only called after the servlet's <code>init()</code> method
     * has completed successfully.
     *
     * <p>
     * The status code of the response always should be set for a servlet that
     * throws or sends an error.
     *
     *
     * <p>
     * Servlets typically run inside multithreaded servlet containers that can
     * handle multiple requests concurrently. Developers must be aware to
     * synchronize access to any shared resources such as files, network
     * connections, and as well as the servlet's class and instance variables.
     * More information on multithreaded programming in Java is available in <a
     * href
     * ="http://java.sun.com/Series/Tutorial/java/threads/multithreaded.html">
     * the Java tutorial on multi-threaded programming</a>.
     *
     *
     * @param req
     *            the <code>ServletRequest</code> object that contains the
     *            client's request
     *
     * @param res
     *            the <code>ServletResponse</code> object that contains the
     *            servlet's response
     *
     * @exception ServletException
     *                if an exception occurs that interferes with the servlet's
     *                normal operation
     *
     * @exception IOException
     *                if an input or output exception occurs
     */
    public void service(ServletRequest req, ServletResponse res)
            throws ServletException, IOException;

    /**
     * Returns information about the servlet, such as author, version, and
     * copyright.
     *
     * <p>
     * The string that this method returns should be plain text and not markup
     * of any kind (such as HTML, XML, etc.).
     *
     * @return a <code>String</code> containing servlet information
     */
    public String getServletInfo();

    /**
     * Called by the servlet container to indicate to a servlet that the servlet
     * is being taken out of service. This method is only called once all
     * threads within the servlet's <code>service</code> method have exited or
     * after a timeout period has passed. After the servlet container calls this
     * method, it will not call the <code>service</code> method again on this
     * servlet.
     *
     * <p>
     * This method gives the servlet an opportunity to clean up any resources
     * that are being held (for example, memory, file handles, threads) and make
     * sure that any persistent state is synchronized with the servlet's current
     * state in memory.
     */
    public void destroy();
}
```

## Tomcat与Servlet的关系

Servlet运行在Tomcat中

### Servlet与普通的Java程序的区别

Servlet本质上就是一个Java类
Servlet类必须实现接口javax.servlet.Servlet接口
运行在Web容器中，tomcat就是一个Web容器。
能够接收浏览器发送的请求，并且做出响应给浏览器。

# 4. 编写自己的 Servlet 

## pom.xml中添加 servlet-api 依赖

```
<dependency>
    <groupId>org.apache.tomcat</groupId>
    <artifactId>servlet-api</artifactId>
    <version>6.0.53</version>
</dependency>
```

## 写一个类继承 HttpServlet

HttpServlet是个抽象类它已经实现了Servlet接口。

![](https://upload-images.jianshu.io/upload_images/1233356-dbcd8329810d925e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们写一个 HelloServlet 类继承 HttpServlet:

```
package com.light.sword;

import javax.servlet.http.HttpServlet;

/**
 * @author: Jack
 * 2019-11-23 22:50
 */
public class HelloServlet extends HttpServlet {
    
}

```

重写 `doGet` 或 `doPost` 方法，分别处理表单的get或post请求。
```
package com.light.sword;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Date;

/**
 * @author: Jack
 * 2019-11-23 22:50
 */
public class HelloServlet extends HttpServlet {

    @Override
    public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        resp.setCharacterEncoding("UTF8");
        PrintWriter writer = resp.getWriter();
        writer.println(new Date() + ", Hello World, by Servlet.");
    }
}

```

如果直接在浏览器输入地址访问，使用的是get方法。



> Note: Get 方法是幂等的,无状态,无副作用的.
The GET method should be safe, that is, without  any side effects

# 5. 编写web.xml配置文件

完成 Servlet 的编写, 还要配置一下 web.xml 文件，对Servlet进行配置，才能通过浏览器来访问。

配置 web.xml

```
<!DOCTYPE web-app PUBLIC
        "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
        "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <display-name>Hello World, Archetype Created Web Application</display-name>
    <description>
        This is a simple web application with a source code organization
        based on the recommendations of the Application Developer's Guide.
    </description>

    <servlet>
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>com.light.sword.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>

```



# 6. war 打包配置

[http://maven.apache.org/plugins/maven-war-plugin/examples/adding-filtering-webresources.html](http://maven.apache.org/plugins/maven-war-plugin/examples/adding-filtering-webresources.html)

maven 打 war包配置:

```
   <build>
        <finalName>simple-javaweb</finalName>
        <sourceDirectory>${project.basedir}/src/main/java</sourceDirectory>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <!-- 指定JDK版本 -->
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>2.4</version>
            </plugin>
        </plugins>
    </build>
```

工程目录规范:

```
/simplejavaweb$ tree
.
├── README.md
├── package.sh
├── pom.xml
├── simplejavaweb.iml
└── src
    └── main
        ├── java
        │   └── com
        │       └── light
        │           └── sword
        │               └── HelloServlet.java
        └── webapp
            ├── WEB-INF
            │   └── web.xml
            ├── hello.jsp
            └── index.html

8 directories, 8 files
```

![](https://upload-images.jianshu.io/upload_images/1233356-6542a017a15b0ded.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> Including and Excluding Files From the WAR: 
[http://maven.apache.org/plugins/maven-war-plugin/examples/including-excluding-files-from-war.html](http://maven.apache.org/plugins/maven-war-plugin/examples/including-excluding-files-from-war.html)


# 7. Tomcat 启动部署

```
apache-tomcat-7.0.82$ ./bin/startup.sh 
Using CATALINA_BASE:   /Users/jack/soft/apache-tomcat-7.0.82
Using CATALINA_HOME:   /Users/jack/soft/apache-tomcat-7.0.82
Using CATALINA_TMPDIR: /Users/jack/soft/apache-tomcat-7.0.82/temp
Using JRE_HOME:        /Library/Java/JavaVirtualMachines/jdk1.8.0_40/Contents/Home
Using CLASSPATH:       /Users/jack/soft/apache-tomcat-7.0.82/bin/bootstrap.jar:/Users/jack/soft/apache-tomcat-7.0.82/bin/tomcat-juli.jar
Tomcat started.

```

![](https://upload-images.jianshu.io/upload_images/1233356-ac56764806a566f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上传 war 包, 然后 deploy .

# 8. 运行测试

访问 [http://localhost:8080/simple-javaweb/](http://localhost:8080/simple-javaweb/)

![](https://upload-images.jianshu.io/upload_images/1233356-4ee1aa7c464eeefe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


点击 To a [JSP page](http://localhost:8080/simple-javaweb/hello.jsp).
http://localhost:8080/simple-javaweb/hello.jsp

```
<%@ page import="java.util.Date" %>
<%@ page session="false" pageEncoding="UTF-8" contentType="text/html; charset=UTF-8" %>
<!DOCTYPE html>
<html>
<body>
<h2>
    <%= new Date() + ", Hello World, by JSP." %>
</h2>
</body>
</html>

```

![](https://upload-images.jianshu.io/upload_images/1233356-0ad1c10ffacbc5ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



点击  To a [Servlet](http://localhost:8080/simple-javaweb/hello).
http://localhost:8080/simple-javaweb/hello
    
```
package com.light.sword;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Date;

/**
 * @author: Jack
 * 2019-11-23 22:50
 */
public class HelloServlet extends HttpServlet {

    @Override
    public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        resp.setCharacterEncoding("UTF8");
        PrintWriter writer = resp.getWriter();
        writer.println(new Date() + ", Hello World, by Servlet.");
    }
}

```

![](https://upload-images.jianshu.io/upload_images/1233356-2bc646ba1301e426.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



# 9. FAQ 问题

## HTTP Status 500 - Error instantiating servlet class


用maven打了war包之后部署到tomcat下居然无法执行,看了一下原来没有任何编译的.class文件.
这个问题通常是打 war 包,没有把编译好的类文件打进去.

配置 build 标签下面的sourceDirectory:

```
<build>
        <finalName>simple-javaweb</finalName>
        <sourceDirectory>${project.basedir}/src/main/java</sourceDirectory>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <!-- 指定JDK版本 -->
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>2.4</version>
            </plugin>
        </plugins>
    </build>
```

```
#!/usr/bin/env bash
mvn clean compile package
```

## .gitignore

```
*.iml
.idea
```

# 工程源码

[https://github.com/to-be-architect/simple-javaweb](https://github.com/to-be-architect/simple-javaweb)


# 参考资料

[https://blog.csdn.net/RookiexiaoMu_a/article/details/89254719](https://blog.csdn.net/RookiexiaoMu_a/article/details/89254719)



---
# Kotlin 开发者社区

![](https://upload-images.jianshu.io/upload_images/1233356-4cc10b922a41aa80?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


国内第一Kotlin 开发者社区公众号，主要分享、交流 Kotlin 编程语言、Spring Boot、Android、React.js/Node.js、函数式编程、编程思想等相关主题。

越是喧嚣的世界，越需要宁静的思考。
