---
title: JDBC的代码结构
date: 2022-05-03 21:42:00.0
updated: 2022-07-04 20:29:36.143
url: /archives/jdbc的代码结构
categories: 
- JDBC
tags: 
---



#### 内容简介：JDBC基本的代码结构，一定要记住！！！

<!--more-->

JDBC 要处理的事情，一共有七步：

- 连接数据库
- 注册驱动
- 获取连接
- 获取数据库操作对象
- 执行 SQL
- 处理结果集
- 释放资源

这个是没有对输入结果进行预先处理的代码，会发生 sql 注入。

~~~java
package Demo000.demo001;
import java.sql.*;
import java.util.*;

public class JDBCTest001 {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");

        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;

        try{
            Class.forName(driver);
            conn = DriverManager.getConnection(url,user,password);
            stmt = conn.createStatement();
            rs = stmt.executeQuery("select deptno,ename,sal from emp");
            while(rs.next()){
                String deptno = rs.getString("deptno");
                String ename = rs.getString("ename");
                String sal = rs.getString("sal");
                System.out.println(deptno + " " + ename + " " + sal);
            }
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            if(rs != null){
                try{
                    rs.close();
                }catch(SQLException e){
                    e.printStackTrace();
                }
            }
            if(stmt != null){
                try{
                    stmt.close();
                }catch(SQLException e){
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try{
                    conn.close();
                }catch(SQLException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
~~~



为了避免 sql 注入的发生，可以使用下面的代码

~~~java
package JDBCTest07;
import java.sql.*;
import java.util.ResourceBundle;

public class JDBCTest07 {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try{
            ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
            String driver = bundle.getString("driver");
            String url = bundle.getString("url");
            String user = bundle.getString("user");
            String password = bundle.getString("password");
            Class.forName(driver);
            conn = DriverManager.getConnection(url,user,password);
            String sql = "select * from t_student";
            ps = conn.prepareStatement(sql);
            rs = ps.executeQuery();
            while(rs.next()){
                String no = rs.getString("no");
                String name = rs.getString("name");
                System.out.println(no + "  " + name);
            }
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            if(rs != null){
                try{
                    rs.close();
                }catch(Exception e){
                    e.printStackTrace();
                }
            }
            if(ps != null){
                try{
                    ps.close();
                }catch(Exception e){
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try{
                    conn.close();
                }catch(Exception e){
                    e.printStackTrace();
                }
            }
        }
    }
}
~~~

---

参考资料：[JDBC从入门到精通视频教程-JDBC实战精讲_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Bt41137iB?p=29&spm_id_from=333.880.my_history.page.click)