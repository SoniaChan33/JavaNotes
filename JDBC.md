# 引入

## 1. 概念

**JDBC**（Java DataBase Connectivity,  Java数据库连接) ,是一种用于执行SQL语句的Java API，为多种关系数据库提供统一访问,它由一组用Java语言编写的类和接口组成

## 2. 依赖

### 导入依赖

src下创建一个文件夹命名lib，然后把依赖包复制到里面，右键create library

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210704112134.png" alt="image-20210704112134768" style="zoom:50%;" />

### 移除依赖

setting ——>project structure

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210704112049.png" alt="image-20210704112049391" style="zoom: 33%;" />

## 3. 实例

#### 1：加载一个Driver驱动

```java
//1.加载驱动 Driver
Driver driver = new com.mysql.cj.jdbc.Driver();
```

#### 2：创建数据库连接（Connection）

* url

  格式——协议**://**ip**:**端口/资源路径**?**参数名**=**参数值**&**参数名=参数值**&....**

  * 协议         jdbc:mysql
  * IP          127.0.0.1/localhost
  *  端口号       3306
     * 数据库名字   mydb
     * 参数  useSSL(是否使用SSL)、useUnicode(是否使用unicod)、characterEncoding(使用unicode类型)、serverTimezone(时区)

* user

* password

```java
//3.获得链接
/*
* 传入三个参数：url，user，password
* */
String url = "jdbc:mysql://127.0.0.1:3306/mytestdb?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/ShangHai";
String user = "root";
String password = "root";
Connection connection = DriverManager.getConnection(url,user,password);

```

#### 3：创建SQL命令发送器Statement

```java
//4.获得语句对象statement
Statement statement = connection.createStatement();
```

#### 4：通过Statement发送SQL命令并得到结果

**statement.executeUpdate** 在sql操作insert、delete、update时使用

```java
//5.执行SQL语句，返回结果
String sql = "insert into dept values(50,'教学部','BeiJing')";
int row = statement.executeUpdate(sql); //insert delete update
System.out.println("影响行数为"+ row);
```

#### 5：关闭数据库资源ResultSet  Statement  Connection

```java
//6.释放资源
statement.close();
connection.close();
```

结果

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210704123012.png" alt="image-20210704123012385" style="zoom:50%;" />

## 4. 常见的异常分析

MySQL8中数据库连接的四个参数有两个发生了变化

String driver = "com.mysql.cj.jdbc.Driver";

String url = "jdbc:mysql://127.0.0.1:3306/mydb?useSSL=false&useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai";

或者String url = ".......serverTimezone=GMT%2B8";

- **错误1：Exception in thread "main" java.lang.ClassNotFoundException: com.mysql.jdbc2.Driver**

原因：没有添加jar包或者com.mysql.jdbc2.Driver路径错误

- **错误2：Exception in thread "main" java.sql.SQLException:No suitable driver found for jbdc:mysql://127.0.0.1:3306/stumgr**

原因：url错误

- **错误3：Exception in thread "main" java.sql.SQLException:**

Access denied for user 'root'@'localhost' (using password: YES)

原因：用户名或者密码错误

- **错误4：Exception in thread "main" com.mysql.jdbc.exceptions**
- ![image-20210704153100091](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210705111903.png)

.jdbc4.MySQLIntegrityConstraintViolationException:Duplicate entry '90' for key 'PRIMARY'

原因：主键冲突

- **错误5：Public Key Retrieval is not allowed**

  如果用户使用 sha256_password 认证，密码在传输过程中必须使用 TLS 协议保护，但是如果 RSA 公钥不可用，可以使用服务器提供的公钥；可以在连接中通过 ServerRSAPublicKeyFile 指定服务器的 RSA 公钥，或者AllowPublicKeyRetrieval=True参数以允许客户端从服务器获取公钥；但是需要注意的是 AllowPublicKeyRetrieval=True可能会导致恶意的代理通过中间人攻击(MITM)获取到明文密码，所以默认是关闭的，必须显式开启
  在jdbc连接添加上参数**allowPublicKeyRetrieval=true**即可，注意参数间用&



加参数allowPublicKeyRetrieval=true

# 1. 驱动的加载

* 在查看Driver的源代码时我们发现,该类内部有一个静态代码块,在代码块中就是在实例化一个驱动并在驱动中心注册.静态代码块会在类进入内存时执行,也就是说,我们只要让该类字节码进入内存,就会自动完成注册,不需要我们手动去new

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210704145529.png" alt="image-20210704145529583" style="zoom:50%;" />

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210704145550.png" alt="image-20210704145550511" style="zoom: 50%;" />

* 可以通过反射来加载驱动

  ```java
  //可以通过反射来加载驱动
  Class.forName("com.mysql.cj.jdbc.Driver");
  ```

  因为Driver类里面的静态代码块已经注册了驱动，就**不用再单独注册驱动了**

* 查看jar包发现,jar包中已经默认配置了驱动类的加载，其实不需要手动加载，程序运行的时候也会自动加载jar包里的类

  ![image-20210704150456396](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210704150456.png)


# 2. 添加异常处理

![image-20210704153100091](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210704153100.png)

将需要catch的异常部分 用快捷键**ctrl+alt+T**选择

<img src="C:/Users/Dell/AppData/Roaming/Typora/typora-user-images/image-20210705110755222.png" alt="image-20210705110755222" style="zoom:50%;" />

把最后处理放在finally里面，分别加上try catch

```java
public class TestJDBC2 {
    private static String driver="com.mysql.cj.jdbc.Driver";//
    private static String url = "jdbc:mysql://127.0.0.1:3306/mytestdb?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true";//
    private static String user= "root";
    private static String password = "root";

    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        try {
            Class.forName(driver);
            connection = DriverManager.getConnection(url,user,password);
            statement = connection.createStatement();
            String sql = "insert into dept values(50,'教学部','BeiJing')";
            int row = statement.executeUpdate(sql);
            System.out.println("影响行数为"+ row);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            if(null != statement){ //statement如果创建失败就不需要关了
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(null != connection){// 同上
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}
```



# 3. JDBC完成CURD

### 删除和修改操作

和上面实例一样，只需要改sql语句

```java
String sql = "delete from emp where deptno = 20 ";
```

```java
String sql = "update dept set dname = '总部',loc='BeiJing' where deptno = 30 ";
```

### 查询操作

- 使用的方法是executeQuery()，返回的是一个结果集即表格被抽象成的对象 **resultSet**

```java
String sql = "select * from emp";
ResultSet resultSet = statement.executeQuery(sql); 
```

- 循环获得每一列的数据：

  **resultset.next()**：返回当前游标所指的行数据是否为空，如果为不为空，返回true，游标向下移动；为空则返回false，游标不移动

```java
while (resultSet.next()){//判断游标是否为空
    int empno = resultSet.getInt("empno");
    String ename = resultSet.getString("ename");
    String job = resultSet.getString("job");
    int MGR = resultSet.getInt("MGR");
    Date date = resultSet.getDate("hiredate");
    double sal = resultSet.getDouble("SAL");
    double comm = resultSet.getDouble("comm");
    int deptno = resultSet.getInt("deptno");
    System.out.println(empno+" "+ename+" "+job+" "+MGR+" "+" "+date+" "+sal+" "+comm+" "+deptno);
}
```

ps: 获得的date对象不是util包的，而是sql包的对象

- 关闭resultSet：

  虽然 Statement 关闭、重新执行或用于从多结果序列中获取下一个结果时，ResultSet将被自动关闭，但是一个好的编程习惯很重要嘛，该关的还是要关。

```java
  if(null != resultSet){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
```

#### 完整代码

```java 
package com.chs.test01;
import java.sql.*;
import static java.lang.Class.forName;
/**
 * @author: ChanHuiShan
 * @date: 2021-07-05 - 07 - 05 - 16:28
 * @description: com.chs.test01
 * @version: 1.0
 */
public class TestJDBC4 {
    private static String driver = "com.mysql.cj.jdbc.Driver";
    private  static String url ="jdbc:mysql://127.0.0.1:3306/mytestdb?" +
            "useSSL=false&useUnicode=true&characterEncoding=UTF-8&" +
            "serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true";
    private static String user = "root";
    private static String password = "root";
    public static void main(String[] args) {
        testQuery();
    }
    public static void testQuery(){
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;
        try {
            //1.加载驱动
            forName(driver);
            //2.建立连接
            connection = DriverManager.getConnection(url,user,password);
            //3.通过连接搭建statement
            statement = connection.createStatement();
            //4.操作sql
            String sql = "select * from emp";
            resultSet = statement.executeQuery(sql);
            while (resultSet.next()){
                int empno = resultSet.getInt("empno");
                String ename = resultSet.getString("ename");
                String job = resultSet.getString("job");
                int MGR = resultSet.getInt("MGR");
                Date date = resultSet.getDate("hiredate");
                double sal = resultSet.getDouble("SAL");
                double comm = resultSet.getDouble("comm");
                int deptno = resultSet.getInt("deptno");
                System.out.println(empno+" "+ename+" "+job+" "+MGR+" "+" "+date+" "+sal+" "+comm+" "+deptno);

            }

        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }finally{
            //关闭resultSet
            if(null != resultSet){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(null != statement){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(null != connection){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}


```



结果：

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210705182201.png" alt="image-20210705182200681" style="zoom:50%;" />

### 创建实体类封装结果集

​	该实体类是和数据表格名称和字段是一一对应的类，该类主要用处是存储重数据库中查询出来的数据，除此之外该类无其他功能。

#### Emp.java

要求：

* 类名和表名保持一致
* 属性个数和表的列数保持一致
* 属性的数据类型和列的数据类型保持一致
* 属性名和表格列名保持一致
* 所有的属性都是私有的 （出于安全考虑）
* 实体类的属性推荐写成包装类 （例如int类型不能赋值为null）
* 日期类型建议使用java.util.Date(因为是父类,比较方便使用多态）
* 所有的属性都要有get和set方法
* 必须具备空参构造器
* 实体类需要实现序列化接口（mybatis 分布式）
* 实体类中其他构造方法可选
* 添加toString方法

```java
package com.chs.entity;
import com.sun.scenario.effect.impl.prism.PrRenderInfo;
import java.io.Serializable;
import java.util.Date;//是java.sql.Date的父类

/**
 * @author: ChanHuiShan
 * @date: 2021-07-05 - 07 - 05 - 19:07
 * @description: com.chs.entity
 * @version: 1.0
 */
public class Emp implements Serializable {//实现序列化接口
    private Integer empno;
    private String ename;
    private String job;
    private Integer mgr;
    private Date hiredate;
    private Double sal;
    private Double comm;
    private Integer deptno;
    //可以有其他构造方法
    public Emp(Integer empno, String ename, String job, Integer mgr, Date hiredate, Double sal, Double comm, Integer deptno) {
        this.empno = empno;
        this.ename = ename;
        this.job = job;
        this.mgr = mgr;
        this.hiredate = hiredate;
        this.sal = sal;
        this.comm = comm;
        this.deptno = deptno;
    }
    //必须有空参构造器
    public Emp() {
    }
    //set和get方法省略
    //toSting省略
}

```

#### TestJDBC.java

修改---返回的是Emp的List集合对象：

```java
public static List<Emp> testQuery(){}
```

main（）方法里面遍历输出：

```java
public static void main(String[] args) {
    //获得方法里返回的集合
    List<Emp> emps = testQuery();
    //遍历集合
    for(Emp emp:emps){
        System.out.println(emp);
    }
}
```



##### 完整代码

```java
/**
 * @author: ChanHuiShan
 * @date: 2021-07-05 - 07 - 05 - 16:28
 * @description: com.chs.test01
 * @version: 1.0
 */
public class TestJDBC4 {
    private static String driver = "com.mysql.cj.jdbc.Driver";
    private  static String url ="jdbc:mysql://127.0.0.1:3306/mytestdb?" +
            "useSSL=false&useUnicode=true&characterEncoding=UTF-8&" +
            "serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true";
    private static String user = "root";
    private static String password = "root";
    public static void main(String[] args) {
        //获得方法里返回的集合
        List<Emp> emps = testQuery();
        //遍历集合
        for(Emp emp:emps){
            System.out.println(emp);
        }
    }
    public static List<Emp> testQuery(){
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;
        List<Emp> list = null;//emp对象的集合初始化

        try {
            forName(driver);
            connection = DriverManager.getConnection(url,user,password);
            statement = connection.createStatement();
            String sql = "select * from emp";
            resultSet = statement.executeQuery(sql);
            list= new ArrayList<>();
            while (resultSet.next()){
                int empno = resultSet.getInt("empno");
                String ename = resultSet.getString("ename");
                String job = resultSet.getString("job");
                int MGR = resultSet.getInt("MGR");
                Date date = resultSet.getDate("hiredate");
                double sal = resultSet.getDouble("SAL");
                double comm = resultSet.getDouble("comm");
                int deptno = resultSet.getInt("deptno");
                Emp emp = new Emp(empno, ename, job, MGR, date, sal, comm, deptno);//构造每一行数据对象
                list.add(emp);//将每一行都放入集合中
            }

        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }finally{
            if(null != resultSet){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

            if(null != statement){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(null != connection){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return list;//返回集合
    }
}
```

# 4. 预编译PreparedStatement

## 使用预编译语句来预防SQL注入攻击

​		SQL注入攻击指的是通过构建**特殊的输入**作为**参数**传入**Web应用程序**，而这些输入大都是SQL语法里的一些组合，通过执行SQL语句进而执行攻击者所要的操作，其主要原因是程序没有细致地过滤用户输入的数据，致使非法数据侵入系统。

#### 模拟登录

在前台输入用户名和密码，后台判断信息是否正确，并给出前台反馈信息，前台输出反馈信息。

具体实现步骤：

* 创建数据库表

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210706143739.png" style="zoom:50%;" />

* 创建实体类

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210706143810.png" alt="image-20210706143810882" style="zoom:50%;" />

* 测试类：

  ```java
  /**
   * @author: ChanHuiShan
   * @date: 2021-07-06 - 07 - 06 - 14:13
   * @description: com.chs.test02
   * @version: 1.0
   */
  public class TestInjection {
      private static String driver = "com.mysql.cj.jdbc.Driver";
      private  static String url ="jdbc:mysql://127.0.0.1:3306/mytestdb?" +
              "useSSL=false&useUnicode=true&characterEncoding=UTF-8&" +
              "serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true";
      private static String user = "root";
      private static String password = "root";
      public static void main(String[] args) {
          Scanner sc = new Scanner(System.in);
          System.out.println("请输入用户名：");
          String username = sc.next();
          System.out.println("请输入密码：");
          String pwd = sc.next();
          Account account = getAccount(username, pwd);
          System.out.println(null!=account?("登录成功"+"用户信息"+account):"登录失败");
      }
  
      public static Account getAccount(String username,String pwd){
          Connection connection = null;
          Statement statement = null;
          ResultSet resultSet = null;
          Account account = null;
          try {
              //1.加载驱动
              forName(driver);
              //2.建立连接
              connection = DriverManager.getConnection(url,user,password);
              //3.通过连接搭建statement
              statement = connection.createStatement();
              //4.操作sql
              String sql = "select * from account where username = '"+username+"'and password = '"+pwd+"'";
              resultSet = statement.executeQuery(sql);
              while (resultSet.next()){
                  int aid = resultSet.getInt("aid");
                  String usernamea = resultSet.getString("username");
                  String pwda = resultSet.getString("password");
                  Double balance = resultSet.getDouble("balance");
                  account = new Account(aid,usernamea,pwda,balance);
              }
  
          } catch (ClassNotFoundException | SQLException e) {
              e.printStackTrace();
          }finally{
              if(null != resultSet){
                  try {
                      resultSet.close();
                  } catch (SQLException e) {
                      e.printStackTrace();
                  }
              }
  
              if(null != statement){
                  try {
                      statement.close();
                  } catch (SQLException e) {
                      e.printStackTrace();
                  }
              }
              if(null != connection){
                  try {
                      connection.close();
                  } catch (SQLException e) {
                      e.printStackTrace();
                  }
              }
              return account;
          }
      }
  }
  
  ```

  结果：

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210706152659.png" alt="image-20210706152659400" style="zoom:50%;" />

#### 注入攻击

构建特殊的输入:

例如输入密码 

```mysql
asdf'or'a'='a
```

整个sql语句就会变为

```mysql
select * from account where username = 'asdf' and password = 'asdf'or'a'='a';
```

导致含义改变。



#### 通过预编译语句来预防

* 使用PreparedStatement语句对象防止注入攻击
   * PreparedStatement 可以使用 **?** 作为参数的占位符,即使是字符串和日期类型,也不使用单独再添加 **' '**
   * connection.createStatement();获得的是普通语句对象 Statement
        ction.prepareStatement(sql);可以获得一个预编译语句对象PreparedStatement
   * 如果SQL语句中有?作为参数占位符号,那么要在执行CURD之前先设置参数
        ***(问号的编号,数据) 方法设置参数

```java

String sql="select * from account where username = ? and password = ?";
preparedStatement = connection.prepareStatement(sql);//这里已经传入SQL语句
//设置参数
preparedStatement.setString(1,username );
preparedStatement.setString(2,pwd );
//执行CURD
resultSet = preparedStatement.executeQuery();// 这里不需要再传入SQL语句，传了会报错，亲身经历man
```

效果：

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210707200554.png" alt="image-20210707200553352" style="zoom:50%;" />

原理：

说白了就是把值当中的所有单引号给**转义**了!这就达到了防止sql注入的目的，说白了mysql驱动的PreparedStatement实现类的**setString()**;方法内部做了单引号的转义，而Statement不能防止sql注入，就是因为它没有把单引号做转义，而是简单粗暴的直接拼接字符串，所以达不到防止sql注入的目的。

##  预编译的原理

### 预编译的开启

值得注意的是,我们的Connector/J 5.0.5及之后useServerPrepStmts默认false,就是默认没有开启预编译,之前默认为true, cachePrepStmts 一直默认为false,需要我们手动设置才可以启用预编译,在开启预编译的同时要同时开启预编译缓存才能带来些许的性能提升.

```java
"jdbc:mysql://localhost:3306/mydb?*****&useServerPrepStmts=true&cachePrepStmts=true"; 
```

### 优点

- 安全性高,可以避免SQL注入

- 简单不繁琐,不用进行字符串拼接

- 性能高，用在执行多个相同数据库DML操作时,可以减少sql语句的编译次数


## 预编译实现CURD

#### TestPreparedStatement.java

```java
/**
 * @author: ChanHuiShan
 * @date: 2021-07-06 - 07 - 06 - 19:31
 * @description: com.chs.test03
 * @version: 1.0
 */
public class TestPreparedStatement {
    private static String driver = "com.mysql.cj.jdbc.Driver";
    private static String url = "jdbc:mysql://127.0.0.1:3306/mytestdb?" +
            "useSSL=false&useUnicode=true&characterEncoding=UTF-8&" +
            "serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true";
    private static String user = "root";
    private static String password = "root";

    public static void main(String[] args) {
        testAdd(); //向emp表中增加一条数据
        testUpdate();//根据工号修改员工表中的数据
        testDelete();//根据工号删除员工表中的数据
        testQuery();//查询名字中包含A的员工信息
    }
    public static void testAdd() {
        //向emp表中增加一条数据
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            String sql = "insert into emp values(DEFAULT,?,?,?,?,?,?,?)";
            preparedStatement = connection.prepareStatement(sql);//这个时候就要传入语句对象
            //设置参数
            preparedStatement.setString(1, "Mark");
            preparedStatement.setString(2, "MANAGER");
            preparedStatement.setInt(3, 7839);
            preparedStatement.setDate(4, new Date(System.currentTimeMillis()));
            preparedStatement.setDouble(5, 3000.12);
            preparedStatement.setDouble(6, 0.0);
            preparedStatement.setDouble(7, 30);
            //执行CURD
            int rows = preparedStatement.executeUpdate();
            System.out.println("影响行数" + rows);
        } 
        catch(){}finally{} 
       }
    }

    public static void testUpdate() {
        //根据工号修改员工表中的数据
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            String sql = "update emp set ename =?,job= ? where empno = ?";
            preparedStatement = connection.prepareStatement(sql);//这个时候就要传入语句对象
            //设置参数
            preparedStatement.setString(1, "John");
            preparedStatement.setString(2, "ANALYST");
            preparedStatement.setInt(3, 7777);
            //执行CURD
            int rows = preparedStatement.executeUpdate();
            System.out.println("影响行数" + rows);
        }catch(){}finally{}//这部分都省略，同下
        }
    }

    public static void testDelete() {
        //根据工号删除员工表中的数据
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            String sql = "delete from emp where empno = ?";
            preparedStatement = connection.prepareStatement(sql);//这个时候就要传入语句对象
            //设置参数
            preparedStatement.setInt(1, 7777);
            //执行CURD
            int rows = preparedStatement.executeUpdate();
            System.out.println("影响行数" + rows);
        } catch(){}finally{}
        }
    }

    public static void testQuery() {
        //查询名字中包含A的员工信息
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        List<Emp> list = null;
        try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            String sql = "select * from emp where ename like ?";
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setString(1,"%A%");
            resultSet = preparedStatement.executeQuery();
            list= new ArrayList<>();
            while (resultSet.next()) {
                int empno = resultSet.getInt("empno");
                String ename = resultSet.getString("ename");
                String job = resultSet.getString("job");
                int MGR = resultSet.getInt("MGR");
                Date date = resultSet.getDate("hiredate");
                double sal = resultSet.getDouble("SAL");
                double comm = resultSet.getDouble("comm");
                int deptno = resultSet.getInt("deptno");
                Emp emp = new Emp(empno, ename, job, MGR, date, sal, comm, deptno);
                list.add(emp);
            }
        } catch () {} finally {}
        for(Emp e:list){
            System.out.println(e);
        }

    }
}


```

# 5. 批处理

## 概念

​		当我们有多条sql语句需要发送到数据库执行的时候，有两种发送方式，一种是执行一条发送一条sql语句给数据库,另一个种是发送一个sql集合给数据库，也就是发送一个批sql到数据库。普通的执行过程是：每处理一条数据，就访问一次数据库；而批处理是：累积到一定数量，再一次性提交到数据库，减少了与数据库的交互次数，所以效率会大大提高,很显然两者的数据库执行效率是不同的，我们发送批处理sql的时候数据库执行效率要高

## Statement和preparedStatement的批处理区别

- statement语句对象实现批处理有如下问题
  - 缺点：采用硬编码效率低，安全性较差。
  - 原理：硬编码，每次执行时相似SQL都会进行编译  
- PreparedStatement+批处理
  - 优点：语句只编译一次，减少编译次数。提高了安全性（阻止了SQL注入）
  - 原理：相似SQL只编译一次，减少编译次数

## 批处理开启

```java
"&rewriteBatchedStatements=true"
```

## 实例

向部门表中增加多条数据

利用循环添加，并每1000为一批，放入之后清除

```java
String sql = "insert into dept values(default ,?,?)";
preparedStatement = connection.prepareStatement(sql);
for (int i = 0; i < 10036 i++) {
    preparedStatement.setString(1,"name");
    preparedStatement.setString(2,"location");
    //放进一批里
    preparedStatement.addBatch();
    if(i%1000==0){
        //每1000为一批，放入后清除
        preparedStatement.executeBatch();
        preparedStatement.clearBatch();
    }

}
//没有整除完的部分
preparedStatement.executeBatch();
preparedStatement.addBatch();
```

# 6.JDBC控制事务

主要是在学习如何让多个数据库操作成为一个整体，实现要么全部执行成功，要么全部都不执行

在JDBC中，事务操作是自动提交，一条对数据库的DML代表一项事务操作

转账的操作：

```java
/**
 * @author: ChanHuiShan
 * @date: 2021-07-08 - 07 - 08 - 16:02
 * @description: com.chs.test05
 * @version: 1.0
 */
public class TestTransaction {
    private static String driver = "com.mysql.cj.jdbc.Driver";
    private static String url = "jdbc:mysql://127.0.0.1:3306/mytestdb?" +
            "useSSL=false&useUnicode=true&characterEncoding=UTF-8&" +    "serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true&useServerPrepStmts=true&cachePrepStmts=true&rewriteBatchedStatements=true";
    private static String user = "root";
    private static String password = "root";
    public static void main(String[] args) {
        testTransaction();
    }
    //定义一个方法，向部门表增加多条数据
    public static void testTransaction(){
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            String sql = "update account set balance = balance-? where aid =?";
            preparedStatement = connection.prepareStatement(sql);
            //转出
            preparedStatement.setDouble(1,100);
            preparedStatement.setInt(2,1);
            //执行
            preparedStatement.executeUpdate();
            //转入
            preparedStatement.setDouble(1,-100);
            preparedStatement.setInt(2,2);
            //执行
            preparedStatement.executeUpdate();

        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {
            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

        }
    }
}

```

为了保证多个sql语句一起执行，使用事务。

在JDBC中，事务操作是**自动提交**。一条对数据库的DML(insert、update、delete)代表一项事务操作,操作成功后，系统将自动调用commit()提交，否则自动调用rollback()回滚。

### 设置事务手动提交

```java
try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            //设置事务手动提交
            connection.setAutoCommit(false);
            String sql = "update account set balance = balance-? where aid =?";
            preparedStatement = connection.prepareStatement(sql);
            //转出
            preparedStatement.setDouble(1,100);
            preparedStatement.setInt(2,1);
            preparedStatement.executeUpdate();
   		   //出现异常
            int i =1/0;
            //转入
            preparedStatement.setDouble(1,-100);
            preparedStatement.setInt(2,2);
            preparedStatement.executeUpdate();

        } catch (Exception e) {
            //回滚操作
            if(null!=connection){
                try {
                    connection.rollback();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
        } finally {
            //提交操作
            if(null!=connection){
                try {
                    connection.commit();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

        }
```

### 设置回滚点

作用是保证回滚点之前的数据更改生效，回滚点以后的数据更改操作就会无效。

并且回滚点的操作值可以进行更改和设置。

- 在增加数据操作里面，每一千设置一个回滚点：

```java
connection = DriverManager.getConnection(url, user, password);
connection.setAutoCommit(false);//设置手动提交
String sql = "insert into dept values(default ,?,?)";
preparedStatement = connection.prepareStatement(sql);
for (int i = 0; i < 10036; i++) {
    preparedStatement.setString(1,"name");
    preparedStatement.setString(2,"location");
    preparedStatement.addBatch();
    if(i%1000==0){
        preparedStatement.executeBatch();
        preparedStatement.clearBatch();
        //每一千条设置回滚点
        Savepoint savepoint = connection.setSavepoint();
        savepoints.addLast(savepoint);
    }
    //期间出现异常
    if(i==10001){
        int x =1/0;
    }

}
preparedStatement.executeBatch();
preparedStatement.addBatch();
```

- 出现异常是10000以后，在最后一个回滚点回滚，剩下36则不保留

```java
catch (Exception e) {
    if (null != connection) {
        try {
            //获得回滚点集合最后一个点
            Savepoint sp = savepoints.getLast();
            if(null != sp){
                connection.rollback(sp);}//回滚
        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }
    e.printStackTrace();
} 
```

* 设置特定的回滚点

```java
 //获得特定的回滚点
 Savepoint sp = savepoints.get(4);
```

# 7. DAO模式

项目结构

- **实体类**：和数据库表格一一对应的类,单独放入一个包中,包名往往是 pojo/entity/bean,要操作的每个表格都应该有对应的实体类

  - emp > class Emp  

    dept > class Dept  

    account > class Account 

- **DAO层**：定义了对数据要执行那些操作的接口和实现类,包名往往是 dao/mapper,要操作的每个表格都应该有对应的接口和实现类

  - emp > interface EmpDao >EmpDaoImpl

    dept > interface DeptDao> DeptDaoImpl

- Mybatis/Spring-JDBCTemplate 中,对DAO层代码进行了封装,代码编写方式会有其他变化

项目截图

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210709153202.png" alt="image-20210709153201647" style="zoom: 50%;" />

## 接口类（DAO层）

### Emp.java

```java
/**
 * @author: ChanHuiShan
 * @date: 2021-07-09 - 07 - 09 - 15:26
 * @description: com.chs.dao
 * @version: 1.0
 */
public interface EmpDao {
    /**
     * 向数据库Emp表中增加一条数据的方法
     * @param emp 要增加的数据封装成的Emp类的对象
     * @return 增加成功返回大于0的整数，增加失败返回0
     */
    int addEmp(Emp emp);

    /**
     * 根据员工编号来删除员工信息的方法
     * @param empno 要删除的员工的编号
     * @return 删除成功返回大于0的整数，删除失败返回0
     */
    int deleteByEmpno(int empno);

    /**
     * 查询所有员工的信息
     * @return 所有员工的信息集合
     */
    List<Emp> findAll();

    /**
     * 根据工号修改员工信息
     * @param emp 传入修改的信息
     * @return 修改成功返回大于0的整数，删除失败返回0
     */
    int updateEmp(Emp emp);

}

```

### Dept.java

```java
/**
 * @author: ChanHuiShan
 * @date: 2021-07-09 - 07 - 09 - 15:26
 * @description: com.chs.dao
 * @version: 1.0
 */
public interface DeptDao {
    /**
     * 查询所有部门信息
     * @return 所有部门信息的集合
     */
    List<Dept> findAll();

    /**
     * 添加部门信息
     * @param dept 需要添加的部门信息
     * @return 增加成功返回大于0的整数，增加失败返回0
     */
    int addDept(Dept dept);
}
```

## 实现类（未被抽取的）

### EmpDaoImpl.java

```java
/**
 * @author: ChanHuiShan
 * @date: 2021-07-09 - 07 - 09 - 15:28
 * @description: com.chs.dao.impl
 * @version: 1.0
 */
public class EmpDaoImpl implements EmpDao {
    private static String driver = "com.mysql.cj.jdbc.Driver";
    private static String url = "jdbc:mysql://127.0.0.1:3306/mytestdb?" +
            "useSSL=false&useUnicode=true&characterEncoding=UTF-8&" +
            "serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true";
    private static String user = "root";
    private static String password = "root";

    @Override
    public int addEmp(Emp emp) {
        //向emp表中增加一条数据
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        int rows= 0;
        try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            String sql = "insert into emp values(DEFAULT,?,?,?,?,?,?,?)";
            preparedStatement = connection.prepareStatement(sql);//这个时候就要传入语句对象
            //设置参数
            preparedStatement.setObject(1, emp.getEname());
            preparedStatement.setObject(2, emp.getJob());
            preparedStatement.setObject(3, emp.getMgr());
            preparedStatement.setObject(4, emp.getHiredate());
            preparedStatement.setObject(5, emp.getSal());
            preparedStatement.setObject(6, emp.getComm());
            preparedStatement.setObject(7, emp.getDeptno());
            //执行CURD
            rows = preparedStatement.executeUpdate();
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {//释放资源
            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return rows;
    }

    @Override
    public int deleteByEmpno(int empno) {
        //根据工号删除员工表中的数据
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        int rows= 0;
        try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            String sql = "delete from emp where empno = ?";
            preparedStatement = connection.prepareStatement(sql);//这个时候就要传入语句对象
            //设置参数
            preparedStatement.setObject(1, empno);
            //执行CURD
            rows = preparedStatement.executeUpdate();
            //设置参数然后今次那
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {//释放资源
            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return rows;
    }

    @Override
    public List<Emp> findAll() {
        //查询名字中包含A的员工信息
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        List<Emp> list = null;
        try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            String sql = "select * from emp ";
            preparedStatement = connection.prepareStatement(sql);
            resultSet = preparedStatement.executeQuery();
            list= new ArrayList<>();
            while (resultSet.next()) {
                int empno = resultSet.getInt("empno");
                String ename = resultSet.getString("ename");
                String job = resultSet.getString("job");
                int MGR = resultSet.getInt("MGR");
                Date date = resultSet.getDate("hiredate");
                double sal = resultSet.getDouble("SAL");
                double comm = resultSet.getDouble("comm");
                int deptno = resultSet.getInt("deptno");
                Emp emp = new Emp(empno, ename, job, MGR, date, sal, comm, deptno);
                list.add(emp);
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {
            if (null != resultSet) {
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

        }
        return list;
    }

    @Override
    public int updateEmp(Emp emp) {
        //向emp表中增加一条数据
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        int rows= 0;
        try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            String sql = "update emp set ename = ?,job = ?,mgr = ?,hiredate=?,sal=?,comm=? ,deptno = ? where empno = ?";
            preparedStatement = connection.prepareStatement(sql);//这个时候就要传入语句对象
            //设置参数
            preparedStatement.setObject(1, emp.getEname());
            preparedStatement.setObject(2, emp.getJob());
            preparedStatement.setObject(3, emp.getMgr());
            preparedStatement.setObject(4, emp.getHiredate());
            preparedStatement.setObject(5, emp.getSal());
            preparedStatement.setObject(6, emp.getComm());
            preparedStatement.setObject(7, emp.getDeptno());
            preparedStatement.setObject(8,emp.getEmpno());
            //执行CURD
            rows = preparedStatement.executeUpdate();
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {//释放资源
            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return rows;
    }
}

```

这些操作都很多相似的地方，可以被抽取成为一个总的类，所以可以啊看待

### DeptDaoImpl.java

```java
/**
 * @author: ChanHuiShan
 * @date: 2021-07-09 - 07 - 09 - 15:29
 * @description: com.chs.dao.impl
 * @version: 1.0
 */
public class DeptDaoImpl implements DeptDao {
    private static String driver = "com.mysql.cj.jdbc.Driver";
    private static String url = "jdbc:mysql://127.0.0.1:3306/mytestdb?" +
            "useSSL=false&useUnicode=true&characterEncoding=UTF-8&" +
            "serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true";
    private static String user = "root";
    private static String password = "root";

    @Override
    public List<Dept> findAll() {
        //查询名字中包含A的员工信息
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        List<Dept> list = null;
        try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            String sql = "select * from dept ";
            preparedStatement = connection.prepareStatement(sql);
            resultSet = preparedStatement.executeQuery();
            list= new ArrayList<>();
            while (resultSet.next()) {
                int deptno = resultSet.getInt("deptno");
                String dname = resultSet.getString("dname");
                String loc = resultSet.getString("loc");
                Dept dept = new Dept(deptno,dname,loc);
                list.add(dept);
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {
            if (null != resultSet) {
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

        }
        return list;
    }

    @Override
    public int addDept(Dept dept) {
        //向emp表中增加一条数据
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        int rows= 0;
        try {
            forName(driver);
            connection = DriverManager.getConnection(url, user, password);
            String sql = "insert into dept values(?,?,?)";
            preparedStatement = connection.prepareStatement(sql);//这个时候就要传入语句对象
            //设置参数
            preparedStatement.setObject(1,dept.getDeptno());
            preparedStatement.setObject(2,dept.getDname());
            preparedStatement.setObject(3,dept.getLoc());
            //执行CURD
            rows = preparedStatement.executeUpdate();
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {//释放资源
            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return rows;
    }
}

```

## 员工管理系统

### EmpManageSystem.java

```java
/**
 * @author: ChanHuiShan
 * @date: 2021-07-09 - 07 - 09 - 20:11
 * @description: com.chs.view
 * @version: 1.0
 */
public class EmpManageSystem {
    private static Scanner sc = new Scanner(System.in);
    private static EmpDao empDao = new EmpDaoImpl();
    private static DeptDao deptDao= new DeptDaoImpl();
    private static SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");

    public static void main(String[] args) {
        while (true){//控制台循环打印
            showMenu();
            System.out.println("请用户录入选项");
            int i = sc.nextInt();
            switch (i){
                case 1:
                    case1();
                    break;
                case 2:
                    case2();
                    break;
                case 3:
                    case3();
                    break;
                case 4:
                    case4();
                    break;
                case 5:
                    case5();
                    break;
                case 6:
                    case6();
                    break;
                default:
                    System.out.println("请输入1-7的数字！！！");
                    break;
            }

        }
    }
    public static void showMenu(){
        System.out.println("----------------------------");
        System.out.println("1.查看所有员工的信息");
        System.out.println("2.查看所有部门信息");
        System.out.println("3.根据工号删除员工信息");
        System.out.println("4.根据工号修改员工信息");
        System.out.println("5.增加员工信息");
        System.out.println("6.增加部门信息");
        System.out.println("----------------------------");

    }
    public static void case1(){
        //查看所有员工的信息
        List<Emp> list = empDao.findAll();
        /*for (Emp e: list){
            System.out.println(e);
        }*/
        list.forEach(System.out::println);
    }
    public static void case2(){
        //查看所有部门的信息
        List<Dept> list = deptDao.findAll();
        list.forEach(System.out::println);
    }
    public static void case3(){
        //根据工号来删除员工信息
        System.out.println("请输入要删除员工的工号：");
        int i = sc.nextInt();
        int i1 = empDao.deleteByEmpno(i);
        if (i1!=0){
            System.out.println("删除成功！");
        }else
        {
            System.out.println("删除失败！");
        }
    }
    public static void case4(){
        //根据员工工号修改员工信息
        System.out.println("*请输入员工的编号");
        int empno = sc.nextInt();
        System.out.println("*请输入员工的姓名");
        String ename = sc.next();
        System.out.println("*请输入员工的工作");
        String job = sc.next();
        System.out.println("*请输入员工的上级编号");
        int mgr = sc.nextInt();
        System.out.println("*请输入员工的聘用日期（格式为yyyy-mm-dd）");
        Date hiredate = null;//把参数从try代码块里拿出来初始化，因为创建对象的时候会访问不到这个值
        try {
            hiredate = simpleDateFormat.parse(sc.next());
        } catch (ParseException e) {
            System.out.println("请按照格式输入!!!");
            e.printStackTrace();
        }
        System.out.println("*请输入员工的薪资");
        double sal = sc.nextDouble();
        System.out.println("*请输入员工的奖金");
        double comm = sc.nextDouble();
        System.out.println("*请输入员工的部门编号");
        int deptno = sc.nextInt();
        Emp emp = new Emp(empno,ename,job,mgr,hiredate,sal,comm,deptno);
        empDao.updateEmp(emp);

    }
    public static void case5(){
        //添加员工信息
        System.out.println("*请输入员工的姓名");
        String ename = sc.next();
        System.out.println("*请输入员工的工作");
        String job = sc.next();
        System.out.println("*请输入员工的上级编号");
        int mgr = sc.nextInt();
        System.out.println("*请输入员工的聘用日期（格式为yyyy-mm-dd）");
        Date hiredate = null;//把参数从try代码块里拿出来初始化，因为创建对象的时候会访问不到这个值
        try {
            hiredate = simpleDateFormat.parse(sc.next());
        } catch (ParseException e) {
            System.out.println("请按照格式输入!!!");
            e.printStackTrace();
        }
        System.out.println("*请输入员工的薪资");
        double sal = sc.nextDouble();
        System.out.println("*请输入员工的奖金");
        double comm = sc.nextDouble();
        System.out.println("*请输入员工的部门编号");
        int deptno = sc.nextInt();
        Emp emp = new Emp(null,ename,job,mgr,hiredate,sal,comm,deptno);
        int i = empDao.addEmp(emp);
        if (i>0){
            System.out.println("添加成功");
        }else{
            System.out.println("添加失败！！！");
        }
    }
    public static void case6(){
        //添加部门信息
        System.out.println("请输入部门号");
        int deptno = sc.nextInt();
        System.out.println("*请输入部门名称");
        String dname = sc.next();
        System.out.println("*请输入部门地址");
        String loc = sc.next();
        Dept dept = new Dept(deptno, dname, loc);
        int i = deptDao.addDept(dept);
        if (i>0){
            System.out.println("添加成功");
        }else{
            System.out.println("添加失败！！！");
        }
    }
}

```

## BaseDao抽取

由上面实现类的代码可以看到增删改查方法的代码都很相似，其实可以把相似的部分抽取出来称为一个BaseDao的类

<img src="C:/Users/Dell/AppData/Roaming/Typora/typora-user-images/image-20210710152134796.png" alt="image-20210710152134796" style="zoom: 50%;" />

### 增删改的抽取--baseUpdate

```java
/**
     * 增删改方法的抽取
     * @param sql 传入的sql语句对象
     * @param args 传入的可变参数
     * @return 修改成功返回大于0的数，返回失败返回0
     */
public static int baseUpdate(String sql,Object ... args){
    Connection connection = null;
    PreparedStatement preparedStatement = null;
    int rows= 0;
    try {
        forName(driver);
        connection = DriverManager.getConnection(url, user, password);
        preparedStatement = connection.prepareStatement(sql);//这个时候就要传入语句对象
        //设置参数
        for (int i = 0; i < args.length; i++) {
            preparedStatement.setObject(i+1,args[i]);
        }
        //执行CURD
        rows = preparedStatement.executeUpdate();
    } catch (ClassNotFoundException | SQLException e) {
        e.printStackTrace();
    } finally {//释放资源
        if (null != preparedStatement) {
            try {
                preparedStatement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (null != connection) {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
    return rows;
}

```

### 查询的抽取--baseQuery

通过**反射**获取传入字节码的对象属性

关于**反射是否破坏封装类**（面试）：https://www.cnblogs.com/wk-missQ1/p/13304329.html

```java
/**
     * 查询操作的抽取
     * @param clazz 传入字节码以便通过反射获得对象的属性，
     *              因为不知道具体会传入什么对象里面是什么属性。
     * @param sql 传入的sql语句对象
     * @param args 一些可变的参数
     * @return 返回一个查询结果集合
     */
public static List baseQuery(Class clazz,String sql,Object ... args){
    Connection connection = null;
    PreparedStatement preparedStatement = null;
    ResultSet resultSet = null;
    List list = null;
    try {
        forName(driver);
        connection = DriverManager.getConnection(url, user, password);
        preparedStatement = connection.prepareStatement(sql);
        list= new ArrayList<>();
        //设置参数 -- 这里要注意呀
        for (int i = 0; i < args.length; i++) {
            preparedStatement.setObject(i+1,args[i]);
        }
        //执行CURD
        resultSet = preparedStatement.executeQuery();
        //通过字节码获取对象的属性
        Field[] fields = clazz.getDeclaredFields();
        //获取访问封装类属性的权限--设置为true可以访问
        for (Field field : fields) {
            field.setAccessible(true);
        }
        while (resultSet.next()) {
            //通过反射创建对象
            Object obj = clazz.newInstance();//默认通过反射调用对象的空参构造方法
            for (Field field : fields) {
                String name = field.getName();//先将属性名字获得
                Object data = resultSet.getObject(name);//通过属性名从结果集中获得
                field.set(data,obj);//将数据放入对象中
            }
            list.add(obj);//将对象加入到链表集合里
        }
    } catch (ClassNotFoundException | SQLException e) {
        e.printStackTrace();
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    } catch (InstantiationException e) {
        e.printStackTrace();
    } finally {
        if (null != resultSet) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if (null != preparedStatement) {
            try {
                preparedStatement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (null != connection) {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

    }
    return list;
}
//设置参数
for (int i = 0; i < args.length; i++) {
    preparedStatement.setObject(i+1,args[i]);
}
//执行CURD
resultSet = preparedStatement.executeQuery();
//通过字节码获取对象的属性
Field[] fields = clazz.getDeclaredFields();
//获取访问封装类属性的权限--设置为true可以访问
for (Field field : fields) {
    field.setAccessible(true);
}
while (resultSet.next()) {
    //通过反射创建对象
    Object obj = clazz.newInstance();//默认通过反射调用对象的空参构造方法
    for (Field field : fields) {
        String name = field.getName();//先将属性名字获得
        Object data = resultSet.getObject(name);//通过属性名从结果集中获得
        field.set(data,obj);//将数据放入对象中
    }
    list.add(obj);//将对象放入list表中
}
```

这样一来，两个实现类中间的增删改操作就可以被简化

### 实现类简化结果

#### DeptDaoImpl.java

```java
public class DeptDaoImpl extends BaseDao implements DeptDao {//注意继承BaseDao
    @Override
    public List<Dept> findAll() {
            String sql = "select * from dept ";
            return baseQuery(Dept.class,sql);
    }
    @Override
    public int addDept(Dept dept) {
            String sql = "insert into dept values(?,?,?)";
            return baseUpdate(sql,dept.getDeptno(),dept.getDname(),
                    dept.getLoc());
    }
}
```

#### EmpDaoImpl.java

```java
public class EmpDaoImpl extends BaseDao implements EmpDao {
    @Override
    public int addEmp(Emp emp) {
            String sql = "insert into emp values(DEFAULT,?,?,?,?,?,?,?)";
            return baseUpdate(sql,emp.getEname(),emp.getJob(),emp.getMgr(),
                    emp.getHiredate(),emp.getSal(),emp.getComm(),emp.getDeptno());
    }

    @Override
    public int deleteByEmpno(int empno) {
            String sql = "delete from emp where empno = ?";
            return baseUpdate(sql,empno);
    }

    @Override
    public List<Emp> findAll() {
            String sql = "select * from emp ";
            return baseQuery(Emp.class,sql);
    }

    @Override
    public int updateEmp(Emp emp) {
        String sql = "update emp set ename = ?,job = ?,mgr = ?,hiredate=?,sal=?,comm=? ,deptno = ? where empno = ?";
        return baseUpdate(sql,emp.getEname(),emp.getJob(),emp.getMgr(),
                emp.getHiredate(),emp.getSal(),emp.getComm(),emp.getDeptno(),emp.getEmpno());
    }
}

```

#### 错误总结：

**java.lang.IllegalArgumentException**：传入参数传错了

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210711103201.png" alt="image-20210711103159768" style="zoom:50%;" />

```java
field.set(obj,data);//将数据放入对象中,不要传反了
```

## 连接池

* 传统的连接：

  ​	首先调用Class.forName()方法加载数据库驱动，然后调用DriverManager.getConnection()方法建立连接.

  * 缺点：

    ​	Connection对象在每次执行DML和DQL的过程中都要创建一次,DML和DQL执行完毕后,connection对象都会被销毁. connection对象是可以反复使用的,没有必要每次都创建新的.该对象的创建和销毁都是比较消耗系统资源的,如何实现connection对象的反复使用呢?使用连接池技术实现.   

* 使用连接池：

  - 预先准备一些链接对象,放入连接池中,当多个线程并发执行时,可以避免短时间内一次性大量创建链接对象,减少计算机单位时间内的运算压力,提高程序的响应速度

  - 实现链接对象的反复使用,可以大大减少链接对象的创建次数,减少资源的消耗

MyConnectionPool.java

* 创建一个连接对象

  ```java
  //准备一个池子装连接对象
  private static LinkedList<Connection> pool;
  ```

* 静态代码块

  * 加载驱动
  * 初始化pool
  * 创建5个链接对象

  ```java
  //使用静态代码块加载驱动，创建连接
  static{
      //加载驱动
      try {
          Class.forName(driver);
      } catch (ClassNotFoundException e) {
          e.printStackTrace();
      }
      //初始化pool
      pool = new LinkedList<>();
      //创建五个链接对象
      for (int i = 0; i < initSize; i++) {
          Connection connection = initConnection();
          if(null != connection){
              pool.add(connection);
              System.out.println("初始化连接："+connection.hashCode()+"放入连接池");
          }
      }
  }
  ```

* 私有的初始化一个链接对象的方法

  ```java
  /**
       * 私有的初始化一个链接对象的方法
       * @return 初始化的连接对象
       */
  private static Connection initConnection(){
      try {
          return DriverManager.getConnection(url,user,password);
      } catch (SQLException e) {
          e.printStackTrace();
      }
      return null;
  }
  ```

* 公有的向外界提供链接对象

  * 如果池子里有对象
    * remove（）
  * 如果池子没了：创建新的对象
  * 最后返回对象

  ```java
  /**
       * 公有的向外界提供链接对象的方法
       * @return
       */
  public static Connection getConnection(){
      Connection connection = null;
      if (pool.size()>0){//如果连接池里面有元素
          connection = pool.remove();//移出集合中的第一个元素
          System.out.println("连接池中还有链接，获得链接："+connection.hashCode());
      }else{
          connection = initConnection();
          System.out.println("连接池为空，创建新链接："+connection);
      }
      return connection;
  }
  ```

* 公有的向链接对象归还链接对象的方法

  * 若链接没有被关闭
    * 调整事务状态，设置为手动提交
    * 容量小于最大容量
      * 池子最后加上connection
    * 容量达到最大容量
      * 关闭连接，打印池子满了，关闭连接
  * 若已经被关闭，打印：无需关闭，不用归还

  ```java
  /**
       * 公有的向链接对象归还链接对象的方法
       * @param connection
       */
  public static void returnConnection(Connection connection){
      if (null != connection){
          try {
              if (!connection.isClosed())
                  if (pool.size()< maxSize){
                      connection.setAutoCommit(true);//调整事务状态
                      pool.addLast(connection);//归还链接对象给池子
                      System.out.println("连接池未满，归还链接对象："+connection.hashCode());
                  }else{
                      connection.close();
                      System.out.println("连接池已满，关闭连接池");
                  }
              }else{
                  System.out.println("连接已经关闭无需归还");//连接被关了下次是没法用的
              }
          } catch (SQLException e) {
              e.printStackTrace();
          }
      }else{
          System.out.println("传入的连接为null，无需归还");
  
  ```



### 测试

1. 创建多个连接：

   ```java
   Connection connection1 = MyConnectionPool.getConnection();
   ...
   Connection connection11 = MyConnectionPool.getConnection();
   ```

   结果：前面小于initSize的部分个从连接池里取，后面超过的部分直接初始化。

   <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210711185528.png" style="zoom:50%;" />

2. 归还连接

   1. 传入null值

      ```java
      MyConnectionPool.returnConnection(null);
      ```

      结果：

      <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210711190848.png" alt="image-20210711190848341" style="zoom:50%;" />

   2. 传入已关闭的连接

      ```java
      connection1.close();
      MyConnectionPool.returnConnection(connection1);
      ```

      结果：

      <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210711190953.png" alt="image-20210711190953769" style="zoom: 67%;" />

   3. 传入超过maxSize的连接

      ```java
      MyConnectionPool.returnConnection(connection11);//maxSize=10---11>10
      ```

      <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210711191448.png" alt="image-20210711191448052" style="zoom:50%;" />

### BaseDao改进

* 原来的参数可以删除

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210711192110.png" alt="image-20210711192110111" style="zoom: 50%;" />

* 加载驱动和连接部分改进

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210711192336.png" alt="image-20210711192335963" style="zoom: 67%;" />

* 连接最后归还

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210711192550.png" alt="image-20210711192550600" style="zoom:50%;" />

#### BaseDao.java

```java
public abstract class BaseDao {
    //原来的参数全部删除
    /**
     * 增删改方法的抽取
     * @param sql 传入的sql语句对象
     * @param args 传入的可变参数
     * @return 修改成功返回大于0的数，返回失败返回0
     */
    public static int baseUpdate(String sql,Object ... args){
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        int rows= 0;
        try {
            connection = MyConnectionPool.getConnection();//从连接池里获得连接
            preparedStatement = connection.prepareStatement(sql);//这个时候就要传入语句对象
            //设置参数
            for (int i = 0; i < args.length; i++) {
                preparedStatement.setObject(i+1,args[i]);
            }
            //执行CURD
            rows = preparedStatement.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {//释放资源
            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            MyConnectionPool.returnConnection(connection);//归还到连接池里
        }
        return rows;
    }

    /**
     * 查询操作的抽取
     * @param clazz 传入字节码以便通过反射获得对象的属性，
     *              因为不知道具体会传入什么对象里面是什么属性。
     * @param sql 传入的sql语句对象
     * @param args 一些可变的参数
     * @return 返回一个查询结果集合
     */
    public static List baseQuery(Class clazz,String sql,Object ... args){
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        List list = null;
        try {
            connection = MyConnectionPool.getConnection();
            preparedStatement = connection.prepareStatement(sql);
            list= new ArrayList<>();
            //设置参数
            for (int i = 0; i < args.length; i++) {
                preparedStatement.setObject(i+1,args[i]);
            }
            //执行CURD
            resultSet = preparedStatement.executeQuery();
            //通过字节码获取对象的属性
            Field[] fields = clazz.getDeclaredFields();
            //获取访问封装类属性的权限--设置为true可以访问
            for (Field field : fields) {
               field.setAccessible(true);
            }
            while (resultSet.next()) {
                //通过反射创建对象
                Object obj = clazz.newInstance();//默认通过反射调用对象的空参构造方法
                for (Field field : fields) {
                    String name = field.getName();//先将属性名字获得
                    Object data = resultSet.getObject(name);//通过属性名从结果集中获得
                    field.set(obj,data);//将数据放入对象中
                }
                list.add(obj);//将对象加入到链表集合里

            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (null != resultSet) {
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            MyConnectionPool.returnConnection(connection);

        }
        return list;
    }

}

```

## 配置文件优化连接池

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210711193622.png" alt="image-20210711193622546" style="zoom: 50%;" />

注意格式：空格不能随便加

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210711193523.png" alt="image-20210711193523624" style="zoom:50%;" />

### PropertiesUtil工具类

* 创建工具类
  * 定义构造方法
    * 传进配置文件的路径
    * 通过字节码获取文件路径
    * 加载IO流
  * 定义一个获得参数的方法

```java
public class PropertiesUtil {
    private Properties properties;//定义配置文件参数
    /**
     * 定义配置文件构造器
     * @param path 传入配置文件路径
     */
    public PropertiesUtil(String path){
        properties = new Properties();
        //通过字节码获取IO流
        InputStream resourceAsStream = this.getClass().getResourceAsStream(path);
        try {
            //加载IO流
            properties.load(resourceAsStream);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
    /**
     * 返回配置文件参数值的方法
     * @param key 参数名
     * @return 参数的字符串
     */
    public String getProperties(String key){
        return properties.getProperty(key);
    }
}

```

### 连接池优化

* 删除参数值 

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712101026.png" alt="image-20210712101026751" style="zoom:50%;" />

* 在静态代码块里配置参数

  ```java
  //创建配置文件对象
  PropertiesUtil propertiesUtil = new PropertiesUtil("/jdbc.properties");
  driver = propertiesUtil.getProperties("driver");
  url = propertiesUtil.getProperties("url");
  user = propertiesUtil.getProperties("user");
  password = propertiesUtil.getProperties("password");
  initSize = Integer.parseInt(propertiesUtil.getProperties("initSize"));
  maxSize = Integer.parseInt(propertiesUtil.getProperties("maxSize"));
  ```

  

### 错误总结

连接池的定义好像出现了某种问题

Exception in thread "main" java.lang.NoClassDefFoundError: Could not initialize class com.chs.dao.MyConnectionPool

发现是构造器中没有用到本身的参数，而是重新定义了一个参数：

<img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712110521.png" alt="image-20210712110521532" style="zoom: 67%;" />



# 8. log4j日志

1.  什么是日志log

   ​		异常信息  登录成功失败的信息  其他重要操作的信息，日志可以记录程序的运行状态,运行信息,用户的一些常用操作.日志可以帮助我们分析程序的运行状态,帮我们分析用户的操作习惯,进而对程序进行改

2.  如何记录日志

   - 方式1：System.out.println(.....)    e.printStackTrace();

     缺点：不是保存到文件，不能长久存储

   - 方式2：IO流 将System.out.println(.....)  e.printStackTrace();写入文件

     缺点：操作繁琐,IO流操作容易阻塞线程,日志没有等级,日志的格式不能很好的定制,要想实行编程复杂

   - 方式3：使用现成的日志框架，比如log4j

     优点：1长久保存 2有等级3格式可以很好的定制 4代码编写简单

3. log4j日志的级别

       FATAL：  指出现非常严重的错误事件，这些错误可能导致应用程序异常中止
       ERROR： 指虽有错误，但仍允许应用程序继续运行
       WARN：  指运行环境潜藏着危害
       INFO：    指报告信息，这些信息在粗粒度级别上突出显示应用程序的进程
       DEBUG： 指细粒度信息事件，对于应用程序的调试是最有用的

4. 使用log4j记录日志

   - 加入jar包   **log4j-1.2.8.jar**

     <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712120555.png" alt="image-20210712120555879" style="zoom:50%;" />

   - 加入属性文件 src 下 **log4j.properties**

     <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712120643.png" alt="image-20210712120643129" style="zoom:50%;" />

     ```properties
     log4j.rootLogger=error,logfile
     ## 方式一：打印到控制台
     log4j.appender.stdout=org.apache.log4j.ConsoleAppender
     log4j.appender.stdout.Target=System.err
     log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout
     ## 方式二：打印到logfile文件
     log4j.appender.logfile=org.apache.log4j.FileAppender
     log4j.appender.logfile.File=d:/msb.log
     log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
     log4j.appender.logfile.layout.ConversionPattern=%d{yyyy-MM-dd   HH:mm:ss} %l %F %p %m%n
     ```

     <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712124734.png" alt="image-20210712124734548" style="zoom: 67%;" />

     <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712125136.png" alt="image-20210712125136115" style="zoom:50%;" />

   -  通过属性文件理解log4j的主要API

     Appender 日志目的地 :ConsoleAppender  FileAppender

     Layout 日志格式化器 ：SimpleLayout  PatternLayout

### 以连接池为例：

* 添加日志对象

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712122737.png" alt="image-20210712122736926" style="zoom:50%;" />

* 初始化日志

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712123200.png" alt="image-20210712123159994" style="zoom:50%;" />

* 将sout语句和错误打印改为日志输出：

  * 错误大多使用fatal级别

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712124134.png" alt="image-20210712124134859" style="zoom:50%;" />

  * sout提示语句一般使用info级别

  <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712124306.png" alt="image-20210712124306011" style="zoom:50%;" />

  运行结果：

  ![image-20210712124525739](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712124525.png)

### 补充

```
%p：输出日志信息的优先级，即DEBUG，INFO，WARN，ERROR，FATAL。
%d：输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，如：%d{yyyy/MM/dd HH:mm:ss,SSS}。
%r：输出自应用程序启动到输出该log信息耗费的毫秒数。
%t：输出产生该日志事件的线程名。
%l：输出日志事件的发生位置，相当于%c.%M(%F:%L)的组合，包括类全名、方法、文件名以及在代码中的行数。例如
test.TestLog4j.main(TestLog4j.java:10)。
%c：输出日志信息所属的类目，通常就是所在类的全名。
%M：输出产生日志信息的方法名。
%F：输出日志消息产生时所在的文件名称。
%L:：输出代码中的行号。
%m:：输出代码中指定的具体日志信息。
%n：输出一个回车换行符，Windows平台为"rn"，Unix平台为"n"。
%x：输出和当前线程相关联的NDC(嵌套诊断环境)，尤其用到像java servlets这样的多客户多线程的应用中。
%%：输出一个"%"字符。
```



# 9. 三大范式

## **第一范式（1NF）**

**要求数据库表的每一列都是不可分割的原子数据项。**

举例说明：

![img](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712153337.png)

在上面的表中，“家庭信息”和“学校信息”列均不满足原子性的要求，故不满足第一范式，调整如下：

![img](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712153335.png)

可见，调整后的每一列都是不可再分的，因此满足第一范式（1NF）；

 

## **第二范式（2NF）**

**在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖）**

**第二范式需要确保数据库表中的每一列都和主键相关，而不能只与主键的某一部分相关（主要针对联合主键而言）。**

举例说明：

![img](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712153333.png)

在上图所示的情况中，同一个订单中可能包含不同的产品，因此主键必须是“订单号”和“产品号”联合组成，

但可以发现，产品数量、产品折扣、产品价格与“订单号”和“产品号”都相关，但是订单金额和订单时间仅与“订单号”相关，与“产品号”无关，

这样就不满足第二范式的要求，调整如下，需分成两个表：

 ![img](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712153328.png) ![img](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712153331.png)



## **第三范式（3NF）**

**在2NF基础上，任何非主[属性](https://baike.baidu.com/item/属性)不依赖于其它非主属性（在2NF基础上消除传递依赖）**

**第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。**

举例说明：

![img](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712153325.png)

上表中，所有属性都完全依赖于学号，所以满足第二范式，但是“班主任性别”和“班主任年龄”直接依赖的是“班主任姓名”，

而不是主键“学号”，所以需做如下调整：

![img](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712153312.png) ![img](https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712153317.png)

这样以来，就满足了第三范式的要求。

ps:如果把上表中的班主任姓名改成班主任教工号可能更确切，更符合实际情况，不过只要能理解就行。

## 总结

- 优点：结构合理、冗余较小、尽量避免插入删除修改异常


- 缺点：性能降低、多表查询比单表查询速度慢。

- 范式是指导数据设计的规范化理论，可以保证数据库设计质量
  - 第一范式：字段不能再分
  - 第二范式：不存在局部依赖
  - 第三范式：不含传递依赖（间接依赖）
- 在实际设计中，要整体遵循范式理论，但某些特定的情况下不能死板遵循，这样会降低数据库的效率，一般单表查询效率会比多表查询高，特定表的的设计可以违反第三范式，增加冗余提高性能
  - 例如：经常购物车条目的中除了条目编号，商品编号，商品数量外，可以增加经常使用的商品名称，商品价格等
  - <img src="https://gitee.com/TeaSea33/typora-picgo/raw/master/img/20210712154207.png" alt="image-20210712154207006" style="zoom:50%;" />
