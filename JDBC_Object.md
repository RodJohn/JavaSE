# 主要对象

	DriverManager，Connection对象的定义和行为


# DriverManager

## 注册驱动

注册底层驱动
	
	DriverManager.registerDriver(new Driver());
	
无耦合注册
	
	Class.forName("com.mysql.jdbc.Driver");

	JVM加载类文件时会执行其中的静态代码块
	将mysql的driver注册到系统的DriverManager中。

	public class Driver extends NonRegisteringDriver implements java.sql.Driver {
		public Driver() throws SQLException {
		}

		static {
			try {
				DriverManager.registerDriver(new Driver());
			} catch (SQLException var1) {
				throw new RuntimeException("Can't register driver!");
			}
		}
	}


## 建立连接

建立和数据库的连接

	Connection con = DriverManager.getConnection(url, username, password);


# Connection

定义

	代表数据库的链接
	

## 创建语句

	Statement createStatement()  
		L静态语句
    PreparedStatement prepareStatement(String sql)
		预编译语句
	CallableStatement prepareCall(String sql)
		存储过程
   
## 事务控制

	void setAutoCommit(boolean autoCommit)
		设置自动提交
	void commit() 
		提交语句
	void rollback() 
		回滚语句


# Statement

## Statement	

定义

    封装静态SQL(写死的SQL)

执行语句

    int executeUpdate(String sql):
		执行DML(增删改)和DDL语句
		如果是执行DDL什么都不返回,执行DML返回受影响的行数。
    ResultSet executeQuery(String sql) :
		执行DQL语句，
		返回一个 ResultSet 对象。 


## PreperedStatement

定义

    预编译语句（带有占位符）

参数设置

	void  setXxx(int parameterIndex, Xxx value):  
		xxx表示数据类型,比如:String,int,Long等，
		parameterIndex:设置第几个占位符?(从1开始)，
		value:需要设置的参数值。

语句执行

	int executeUpdate()
	ResultSet executeQuery() 
	作用同于Statement此时不需要传递SQL参数
				

## 区别

好处

	PreparedStatement 
	可以防止SQL注入危机
	可能提高性能(预编译)



# ResultSet

定义

	表示数据库结果集的数据表，是一个二维表

## 获得元数据

	ResultSetMetaData getMetaData()
	ResultSetMetaData包括表名，列名，列数据类型获取
	
## 下一行

	next()
	
	
## 获取指定列数据

	String getString(String columnLabel)
	String getString(int columnIndex)





