---
layout: post
title: JDBC的ResultSet接口学习笔记(一)
date: 2018-05-23 23:03:50
tags: JDBC
---
### 一、ResultSet接口的介绍：
对数据库的查询操作，一般需要返回查询结果，在程序中，JDBC为我们提供了ResultSet接口来专门处理查询结果集。

Statement通过以下方法执行一个查询操作：
``` 
ResultSet executeQuery(String sql) throws SQLException 
```
单词Query就是查询的意思。函数的返回类型是ResultSet，实际上查询的数据并不在ResultSet里面，依然是在数据库里，ResultSet中的next()方法类似于一个指针，指向查询的结果，然后不断遍历。所以这就要求连接不能断开。

ResultSet接口常用方法：
<!--more-->
boolean next()     遍历时，判断是否有下一个结果
int getInt(String columnLabel)
int getInt(int columnIndex)
Date getDate(String columnLabel)
Date getDate(int columnIndex)
String getString(String columnLabel)
String getString(int columnIndex)

###二、ResultSet接口实现查询操作：
步骤如下：（和上一篇博文中的增删改的步骤类似哦）

1、加载数据库驱动程序：Class.forName(驱动程序类)
2、通过用户名密码和连接地址获取数据库连接对象：DriverManager.getConnection(连接地址,用户名,密码)
3、构造查询SQL语句
4、创建Statement实例：Statement stmt = conn.createStatement()
5、执行查询SQL语句，并返回结果：ResultSet rs = stmt.executeQuery(sql)
6、处理结果
7、关闭连接：rs.close()、stmt.close()、conn.close()

### Code block
``` 
package com.vae.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class JdbcQuey {


    //数据库连接地址
    private final static String URL = "jdbc:mysql://localhost:3306/JDBCdb";
    //用户名
    public final static String USERNAME = "root";
    //密码
    public final static String PASSWORD = "smyh";
    //加载的驱动程序类（这个类就在我们导入的jar包中）
    public final static String DRIVER = "com.mysql.jdbc.Driver";
    
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        query();

    }
    
    
    //方法：查询操作
    public static void query(){
        try {
            Class.forName(DRIVER);
            Connection conn = DriverManager.getConnection(URL, USERNAME, PASSWORD);
            String sql = "select id,name,age,description from person";
            Statement state = conn.createStatement();
            //执行查询并返回结果集
            ResultSet rs = state.executeQuery(sql);
            while(rs.next()){  //通过next来索引：判断是否有下一个记录
                //rs.getInt("id"); //方法：int java.sql.ResultSet.getInt(String columnLabel) throws SQLException
                int id = rs.getInt(1);  //方法：int java.sql.ResultSet.getInt(int columnIndex) throws SQLException

                String name = rs.getString(2);
                int age = rs.getInt(3);
                String description = rs.getString(4);
                System.out.println("id="+id+",name="+name+",age="+age+",description="+description);
            }
            rs.close();
            state.close();
            conn.close();            
            
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```


### 三、ResultSet中rs.getInt(),rs.getSting()和rs.getDouble()的区别和用法
rs.getXxx(参数)中Xxx代表哪个，参数都可以是1，2，3，4，表示取第1，2，3，4列的值，或者参数也可以直接是 id name address salary

问题是Xxx什么时候应该用Int，什么时候应该用String或者Double呢？这取决于数据库中每列所处处数据的类型。

如，最好这样获取各列

rs.getInt(1) = rs.getInt(id)

rs.getString(2) = rs.getString(name)

rs.getDouble(4) = rs.getDouble(salary)



