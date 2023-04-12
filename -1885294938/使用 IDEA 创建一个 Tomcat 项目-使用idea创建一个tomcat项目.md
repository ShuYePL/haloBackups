---
title: 使用 IDEA 创建一个 Tomcat 项目
date: 2022-05-07 22:37:00.0
updated: 2022-07-04 20:17:54.093
url: /archives/使用idea创建一个tomcat项目
categories: 
- JavaWeb
tags: 
---



#### 内容简介：使用 IDEA 创建 TOMCAT 的过程还是挺复杂的，中间还因为一些软件层面的原因走了一段弯路，这里用作示例的 IDEA 是专业版的。

<!--more-->

搭建的步骤如下：

- 先创建一个空的工程文件，并在里面创建一个Module
- 将这个 Module 设置为 Servlet 项目的模块
- 导入 Servlet 的 jar 包
- 添加 JDBC 连接数据库的 jar 包
- 编写 Servlet 的实现类
- 在 xml 文件中注册编写的 Servlet 的实现类
- 在 WEB-INF 同级目录下新建一个 html 页面，插入上面那个类的超链接
- IDEA 关联 Tomcat 服务器，并将项目部署到 Tomcat 服务器中
- 启动服务
- 写代码，连接 Tomcat ，启动服务

## 创建空工程文件，并在里面创建一个 Module

首先，先创建一个空的工程文件

![202205001](http://img.shuyepl.com/202207042013544.jpg)

点击 next 之后在出现的的界面中输入项目的名字，并选择相应的路径之后，点击 Finish 完成项目的创建

![202205002](http://img.shuyepl.com/202207042013517.jpg)

接下来在 IDE 界面的左上角找到 File 菜单选项，依次找到 File -> new -> Module... 在出现的界面中点击 Java （一般会自动选择这个的），然后点击 Next 选项

![202205003](http://img.shuyepl.com/202207042013011.jpg)

在出现的界面中输入 Module 的名字就行了，填好名字（这里我将名字改为了 servlet01 ，后面图片中 Module 中显示的名字是 servlet01）之后点击下方的 Finish 按钮，这样一个 Module 就创建完成了

![202205004](http://img.shuyepl.com/202207042013813.jpg)

## 将这个 Module 设置为 Servlet 项目的模块

在刚刚创建的 Module 模块上右键，选择 Add Framework Support... 鼠标左键点击

![202205005](http://img.shuyepl.com/202207042013772.jpg)

在弹出来的界面中将 Web Application 前面的 √ 给打上，然后点击 ok 按钮

![202205006](http://img.shuyepl.com/202207042014600.jpg)

完成以上的步骤之后就可以在 IDE 的左侧看到一个基本的 webapp 的框架，如图所示

![202205007](http://img.shuyepl.com/202207042014917.jpg)

在这里，我们可以看到有一个文件夹的名字是 web 的，它的图标上有个小蓝点，这个代表这个文件夹类似于是 Servlet 项目中 webapps 里面的文件夹的意思

## 导入 Servlet 的 jar 包

因为我们编写的 Servlet 程序是需要实现 Servlet 接口的，在我们原本的 IDEA 环境中没有导进除 java JKD 以外 jar 包，我们需要手动将Servlet 的 jar 包导进来（这里说的 jar 包也就是 Tomcat 根目录里面 lib 文件夹里面的 jsp-api.jar 和 servlet-api.jar 这两个包，我们目前只需要用到这两个包）

先 File -> Project Structure... ，接着在出现的界面中 Module -> Dependencies -> 1 JARs or Directories... 

![202205008](http://img.shuyepl.com/202207042014010.jpg)

在弹出的界面中按住 Ctrl 键将刚才上面说到的两个 jar 包选中，点击 OK 就可以看到两个 jar 包被加载进去了

![202205009](http://img.shuyepl.com/202207042014407.jpg)

最后点击 Apply 和 OK 这个两个按钮就完成 jar 包的导入了

![202205010](http://img.shuyepl.com/202207042014993.jpg)

然后我们可以在 IDE 中编写一个类实现 Servlet 接口，将其要实现的方法都实现一下，如果没有报错，说明我们导入 jar 包已经成功了

## 添加 JDBC 连接数据库的 jar 包

因为我们后期的代码要用 JDBC 连接 mysql 数据库，所以要导入相关的 jar 包，注意，这里导入这个 jar 包的方式和上面的不一样

我们要在 web 文件夹下的 WEB-INF 文件夹中创建一个 lib 的子文件夹，然后将相关的 jar 包复制粘贴到这个包下，这个 jar 包可以在 mysql 的官网下载到

## 编写 Servlet 的实现类

这里编写的 Servlet 的实现类使用了 JDBC 连接数据库了，代码如下，因为较为简单，就没有写相关的注释了

~~~ java
package com.bjpowernode.javaweb.servlet;

import jakarta.servlet.*;

import java.io.PrintWriter;
import java.sql.*;
import java.io.IOException;

public class StudentServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        String driver = "com.mysql.cj.jdbc.Driver";
        String url = "jdbc:mysql://localhost:3306/bjpowernode";
        String user = "root";
        String password = "**************";

        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;

        servletResponse.setContentType("text/html");
        PrintWriter out = servletResponse.getWriter();
        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(url,user,password);
            String sql = "select no,name from t_student";
            ps = conn.prepareStatement(sql);
            rs = ps.executeQuery();
            while(rs.next()){
                String no = rs.getString("no");
                String name = rs.getString("name");
                out.print(no + "  " + name);
            }
        } catch(Exception e) {
            e.printStackTrace();
        } finally {
            if(rs != null){
                try {
                    rs.close();
                } catch(Exception e) {
                    e.printStackTrace();
                }
            }
            if(ps != null){
                try {
                    ps.close();
                } catch(Exception e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch(Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
~~~



## 在 xml 文件中注册编写的 Servlet 的实现类

xml 文件中的内容如下所示

~~~ xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>StudentServlet</servlet-name>
        <servlet-class>com.bjpowernode.javaweb.servlet.StudentServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>StudentServlet</servlet-name>
        <url-pattern>/servlet/student</url-pattern>
    </servlet-mapping>
</web-app>
~~~

## 在 WEB-INF 同级目录下新建一个 html 页面，插入上面那个类的超链接

内容如下

~~~ html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>student page</title>
</head>
<body>
    <a href="/webProject/servlet/student">student list</a>
</body>
</html>
~~~

在上面的超链接标签中，我的地址中最前面的 webProject 这个就是我的这个应用的名字（可以自行选取的），要记住，待会要用到

## IDEA 关联 Tomcat 服务器，并将项目部署到 Tomcat 服务器中

点击 IDEA 界面中右上角绿色小锤子旁边的 Add Configuration，在出现的界面中点击左上角的 + 号，选择 Tomcat Server 中的 local 选项，点击

![202205011](http://img.shuyepl.com/202207042015285.jpg)

然后，就会出现一个非常复杂的界面，里面要设置和填写的东西有点多，别慌，我们一个一个慢慢来

![202205012](http://img.shuyepl.com/202207042015179.jpg)

首先，我们来看第一个要设置的地方，点击图中 1 号标记的 Configure 选项，然后，就会出现下图所示的界面，这里明晃晃的写着 Tomcat Home 了，所以，是要我们配置 Tomcat 的根目录，配置好了之后，点击 ok 选项就可以了（这里我是出现这个界面的时候，里面的内容就直接自动填好的了）

![202205013](http://img.shuyepl.com/202207042015620.jpg)

接下来是第二个箭头的 name 那个地方，那个是服务器的名字，随便起一个就可以了

第三个箭头的地方是设置是否开启服务器之后自动打开浏览器，这个看个人爱好自行选择

然后其他地方的话，现在暂且默认就可以了

到目前未知，我们设置的都是服务器 Server 参数，接下来要配置 Deployment 参数部署项目了，首先点击第四个箭头的地方，出现的界面就是配置 Deployment 参数的界面了。在出现的界面中找到一个 + 号，这个 + 有可能出现在图中箭头所示的地方，也有可能在上面一些的位置，要自己找一下，点击 + 号之后选择 Artifact.. 选项

![202205014](http://img.shuyepl.com/202207042015013.jpg)

完成上面的步骤之后，就会自动添加好相关的内容了，但是，这个时候要注意，如下图所示，红色方框内的内容是你这个应用的名字，我们之前在配置 html 界面的时候记住的应用名字这个时候就有用了，这里的名字要和上面的应用的名字一样

![202205015](http://img.shuyepl.com/202207042015851.jpg)

改完名字之后点击下方的 Apply ，然后点击 OK 就可以了

## 启动服务器

终于完成所有配置了，接下来我们点击右上角的绿色小三角运行就可以了，在浏览器中输入网址，就可以访问了。

---

参考资料：[动力节点最新JavaWeb视频教程,javaweb零基础入门到精通IDEA版-【已完结】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Z3411C7NZ?spm_id_from=333.999.header_right.fav_list.click)