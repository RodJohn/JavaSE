# JDBC

JDBC(Java DataBase Connectivity)

JDBC本身是java连接数据库的一个标准,是进行数据库连接的抽象层,由java编写的一组类和接口组成，接口的实现由各个数据库厂商来完成


？？具体的原理呢 怎么调用的







# 步骤
	
	事务
    1):加载注册驱动.
    2):获取连接对象.
    3):创建/获取语句对象
    4):执行SQL语句
    5):释放资源
	

加载数据库驱动

	Class.forName("com.mysql.jdbc.Driver");

	
获取连接

	Connection conn = DriverManager.getConnection(url,user,pass); 
	
	URL用于标识数据库的位置，程序员通过URL地址告诉JDBC程序连接哪个数据库，URL的写法为
	MySql：jdbc:mysql://localhost:3306/shen ?参数名：参数值
	
创建Statement

	PreperedStatement st =  conn.preparedStatement()
	
执行语句

	ResultSet rs = st.executeQuery()
	
处理结果

	结果集或者数据库变更数
	
	
资源释放
	
	
	

	

# 重要对象

## DriverManager

注册

	Class.forName("com.mysql.jdbc.Driver");
	原因
	Class.forName() 方法要求JVM查找并加载指定的类到内存中；
	由于JVM加载类文件时会执行其中的静态代码块，从Driver类的源码中可以看到该静态代码块执行的操作是：将mysql的driver注册到系统的DriverManager中。


public class Driver extends NonRegisteringDriver implements java.sql.Driver {
    public Driver() throws SQLException {
    }

    static {
        try {
        	//1. 新建一个mysql的driver对象
        	//2. 将这个对象注册到DriverManager中
            DriverManager.registerDriver(new Driver());
        } catch (SQLException var1) {
            throw new RuntimeException("Can't register driver!");
        }
    }
}


获取连接

Connection con = DriverManager.getConnection(url, username, password);



## Connection

	它用于代表数据库的链接
	    Statement createStatement()  创建一个 Statement 对象来将 SQL 语句发送到数据库。
    PreparedStatement prepareStatement(String sql) :获取预编译语句对象。
       参数:sql,并不是一个静态SQL,而是带有占位符的SQL(?).
    void close():关闭连接对象 


	
## Statement	

int executeUpdate(String sql)：用来执行更新操作，包括DML（insert、update、delete）和DDL（create、drop、alter）,返回影响的行数；
ResultSet executeQuery(String sql):用来执行select查询，返回包含查询的结果集
    用于执行静态 SQL (写死的SQL,可以执行运行的SQL)语句并返回它所生成结果的对象。
    int executeUpdate(String sql):可以执行DML(增删改)和DDL语句,如果是执行DDL什么都不返回,执行DML返回受影响的行数。
    ResultSet executeQuery(String sql) :执行给定的 DQL语句，该语句执行之后返回一个 ResultSet 对象。 


## PreperedStatement

    是Statement的子接口,表示预编译的 SQL 语句的对象，设置占位符参数(告诉SQL中的?到底表示哪一个值):

 void  setXxx(int parameterIndex, Xxx value):  xxx表示数据类型,比如:String,int,Long等，parameterIndex:设置第几个占位符?(从1开始)，value:需要设置的参数值。
    int executeUpdate():可以执行DML(增删改)和DDL语句,如果是执行DDL什么都不返回,执行DML返回受影响的行数.
    ResultSet executeQuery() :执行给定的 DQL语句，该语句执行之后返回一个 ResultSet 对象。
                注意:此时不需要传递SQL参数
好处

二、Statement和PreparedStatement的区别

3).PreparedStatement 能保证安全性，可以防止SQL注入危机
？？？

PreparedStatement 的优点:
1).PreparedStatement 代码的可读性和可维护性. (SQL模板,使用占位符表示参数)
2).PreparedStatement 能最大可能提高性能(预编译),MySQL不支持PreparedStatement的性能优化.

选择:使用PreparedStatement。

https://cs-css.iteye.com/blog/1847772



## ResultSet

  表示数据库结果集的数据表，通常通过执行查询数据库的语句生成.
   ResultSet 对象具有指向其当前数据行的光标。最初，光标被置于第一行之前。next 方法将光标移动到下一行；因为该方法在 ResultSet 对象没有下一行时返回 false，所以可以在 while 循环中使用它来迭代结果集。(类似迭代器操作)

ResultSet是一个二维表

    得到元数据：rs.getMetaData()，返回值为ResultSetMetaData；
    获取结果集列数：int getColumnCount()
    获取指定列的列名：String getColumnName(int colIndex)


# 事务


Connection的三个方法与事务相关：

    setAutoCommit(boolean)：设置是否为自动提交事务，如果true（默认值就是true）表示自动提交，也就是每条执行的SQL语句都是一个单独的事务，如果设置false，那么就相当于开启了事务了；con.setAutoCommit(false)表示开启事务！
    commit()：提交结束事务；con.commit();表示提交事务
    rollback()：回滚结束事务。con.rollback();表示回滚事务
--------------------- 


try {
 
  con.setAutoCommit(false);//开启事务…
 
  ….
 
  …
 
  con.commit();//try的最后提交事务
 
} catch() {
 
  con.rollback();//回滚事务
 
}

# java和DB数据类型




# 批处理

批量操作(batch)

    当需要成批插入或者更新记录时，可以采用Java的批量更新机制，这一机制允许多条语句一次性提交给数据库批量处理。通常情况下比单独提交处理更有效率.
	批处理就是一批一批的处理，只针对更新（增，删，改）语句，不包括查询。
	
	
JDBC的批量处理语句包括下面两个方法：
addBatch(String sql)：添加需要批量处理的SQL语句或是参数；
executeBatch（）；执行批量处理语句；

-------------------------------------------------------------------------
Statement 批处理 ： 一次性可以执行多条sql语句，需要编译多次。
应用场景：系统初始化 (创建表，创建数据等)
添加sql语句，st.addBatch(sql)   --添加sql语句
批量处理sql语句，int[] st.executeBatch()
清除缓存： st.clearBatch();
 String sql1 = "insert into user(name,password,email,birthday)

  values('kkk','123','abc@sina.com','1978-08-08')";

String sql2 = "update user set password='123456' where id=3";

st = conn.createStatement();
st.addBatch(sql1);  //把SQL语句加入到批命令中
st.addBatch(sql2);  //把SQL语句加入到批命令中
st.executeBatch();



-------------------------------------------------------------------------
PreparedStatement 批处理 ： 执行一条sql语句，编译一次，执行sql语句的参数不同。
应用场景：表数据初始化
添加批量参数：psmt.addBatch()    --添加实际参数，执行之前，需要执行psmt.setXxx()设置实际参数
执行批处理：int[] psmt.executeBatch()
清除缓存：pstm.clearBatch();

PreparedStatement.addBatch()

conn = JdbcUtil.getConnection();

String sql = "insert into user(name,password,email,birthday) values(?,?,?,?)";

st = conn.prepareStatement(sql);

for(int i=0;i<50000;i++){

st.setString(1, "aaa" + i);

st.setString(2, "123" + i);

st.setString(3, "aaa" + i + "@sina.com");

st.setDate(4,new Date(1980, 10, 10));

st.addBatch();

if(i%1000==0){

st.executeBatch();

st.clearBatch();

}

}

st.executeBatch();







MySQL服务器既不支持PreparedStatement的性能优化,也不支持JDBC中的批量操作.但是,在新的JDBC驱动中,我们可以通过设置参数来优化。

url=jdbc:mysql://localhost:3306/test?rewriteBatchedStatements=true

	
	
	

# 自增结果

在ResultSet中获取自动生成的主键


Statement:

	st.executeUpdate(sql, Statement.RETURN_GENERATED_KEYS);	//设置需要返回自动生成的主键
	ResultSet rs = st.getGeneratedKeys();		        //获取返回的含主键的结果集
	int executeUpdate(String sql,int autoGeneratedKeys):
	执行SQL
	参数:autoGeneratedKeys,是否需要返回自动生成的主键.常量值:Statement.RETURN_GENERATED_KEYS
	
	ResultSet getGeneratedKeys():获取自动生成的主键
	

			String sql = "insert into t_student (name,age) values('a',22)";
			st = conn.createStatement();
			st.executeUpdate(sql, Statement.RETURN_GENERATED_KEYS);	//设置需要返回自动生成的主键
			ResultSet rs = st.getGeneratedKeys();		        //获取返回的含主键的结果集
			while(rs.next()){
				int id = rs.getInt(1);
				System.out.println(id);
			}


PreparedStatement:

PreparedStatement prepareStatement(String sql, int autoGeneratedKeys)  :创建PreparedStatement对象,并且指定是否需要返回生成的主键. 参数的常量值:Statement.RETURN_GENERATED_KEYS









# 注意点

SQL注入
批量SQL
事务
获取自增值
数据类型


# 参考

https://blog.csdn.net/qq_38741971/article/list/3?t=1&


https://blog.csdn.net/qq_22172133/article/details/81266048
https://blog.csdn.net/zdb292034/article/details/81705876


