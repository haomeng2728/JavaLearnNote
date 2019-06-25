# Realize The Database's Four Basic Operation based JDBC Connecting to The  MySQL Database 

[TOC]

## 1. Config MySQL for Your PC

Environment: Win10 + IDEA + MySQL8.0.16

Due to the network speed, I couldn't download the whole MySQL.msi totally with one request. Thus, I downloaded this software of non-install version and configed my environment according to [**INSTALL HELP PAGE**](https://blog.csdn.net/weixin_43209201/article/details/86158043).

Refered to [**IDEA CONFIGURATION HELP**](<https://blog.csdn.net/qq_36172505/article/details/84102468>), MySQL can be run on my computer.





## 2. Import Java Package of SQL

There are six packages we need to import which are as follows:

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
```



## 3. Set Basic Information of Database

```Java
String DBURL = "jdbc:mysql://localhost:3306/demo?&useSSL=false&serverTimezone=UTC";
String DBDRIVER = "com.mysql.cj.jdbc.Driver";
String USERNAME = "root";
String DBPWD = "123456";
```

Note: 

+ For DBURL: **localhost** is your ip address; **3306** is your port which can be adjusted by yourself; **demo** is your database name; and the rests is some option of your database setting
+ For DBDRIVER: it seems that for the MySQL version more than 6.0 should use "com.mysql.cj.jdbc.Driver" instead of "com.mysql.jdbc.Driver"
+ For USERNAME and DBPWD: they are the user name and user password of your MySQL 



## 4. Establish and Close The Connection of Database

### 4.1 establish connection

```java
public Connection getConnection() {
    Connection connection = null;
    try {
        // load database driver, Class.forName can be considered as an initialization of Class
        Class.forName(DBDRIVER);
        // get database connection
        connection = DriverManager.getConnection(DBURL, USERNAME, DBPWD);
        System.out.println("Connection Succeeded!");
    } catch (Exception e) {
        System.out.println("Connection Failed!");
    }
    return connection;
}

```

### 4.2 close the connection

```java
public void closeConnection(Connection connection) {
        try {
            connection.close();
            System.out.println("Close Succeeded");
        } catch (SQLException e) {
            System.out.println("Close Failed");
        }
    }
```

## 5. Realize Four Basic Database Operation

### 5.1 insert

```java
public void insert() {
	String sql = "insert into user (username, age, salary) values ('ameilia', 23, 2500.00)";
    Connection connection = getConnection();
    try {
    	//get database operation class
    	Statement statement = connection.createStatement();
    	//excute sql and return result
    	int result = statement.executeUpdate(sql);
    	if (result != 0) {
    		System.out.println("Insert Operation Succeeded" + result + "row");
    	}
    	statement.close();
    } catch (SQLException e) {
    	System.out.println("Insert Operation Failed");
    } finally {
    	closeConnection(connection);
    }
}
```



### 5.2 delete

```java
public void delete() {
    String sql = "delete from user where username='ameilia' ";
    Connection connection = getConnection();
    try {
        Statement statement = connection.createStatement();
        int result = statement.executeUpdate(sql);
        if (result != 0) {
        	System.out.println("Delete Operation Succeeded" + result + "row");
        }
    	statement.close();
    } catch (SQLException e) {
    	System.out.println("Delete Operation Failed");
    } finally {
    closeConnection(connection);
    }
}
```



### 5.3 update

```java
public void update() {
    String sql = "update user set salary = 4000.00 where username = 'tom'";
    Connection connection = getConnection();
    try {
        Statement statement = connection.createStatement();
        int result = statement.executeUpdate(sql);
        if (result != 0) {
            System.out.println("Update Operation Succeeded" + result + "row");
        }
        statement.close();
    } catch (SQLException e) {
        System.out.println("Update Operation Failed");
    } finally {
        closeConnection(connection);
    }
}
```



### 5.4 select

```java
public void select() {
    String sql = "select * from user where username = 'tom'";
    Connection connection = getConnection();
    try {
        //express sql
        PreparedStatement statement = connection.prepareStatement(sql);
        ResultSet resultSet = statement.executeQuery();
        while (resultSet.next()) {
            System.out.println("UserName = " + resultSet.getString("username"));
            System.out.println("UserAge = " + resultSet.getInt("age"));
            System.out.println("UserSalary = " + resultSet.getDouble("salary"));
        }
        resultSet.close();
        statement.close();
    } catch (SQLException e) {
        System.out.println("Select Operation Failed");
    } finally {
        closeConnection(connection);
    }
}
```



## 6. Test Procedure

```java 
public static void main(String[] args) {
        JDBC_DS_Practice testdemo = new JDBC_DS_Practice();
        System.out.println("=======query begin======");
        testdemo.select();
        System.out.println("=======query end======");

        System.out.println("=======insert begin======");
        testdemo.insert();
        System.out.println("=======insert end======");

        System.out.println("=======delete begin======");
        testdemo.delete();
        System.out.println("=======delete end======");

        System.out.println("=======update begin======");
        testdemo.update();
        System.out.println("=======update end======");

        System.out.println("=======query begin======");
        testdemo.select();
        System.out.println("=======query end======");
}
```

## 7. SQL Design

I designed my sql through the mysql terminal. I added three records into the user table of demo database.

![SQL Design](D:\JavaWorkspace\PracticeNote\SQL_Design.png)



## 8. Subsequent Work

+ Learn MyBatis to optimize the problems in JDBC connection. For example, the hard coding of connection parameter and sql sentence; the frequent connection and close of database; the hard coding of querying result set to get data. 
+ Learn the core of Java, JVM, especially for the heap and stack of Java.