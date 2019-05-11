# DriverManager

# 注册驱动

## 作用

 	注册驱动其实就是加载驱动类文件
	将其用作JDBC接口的实现
	
## 方式

静态方法注册

	注册底层驱动
	DriverManager.registerDriver(new Driver());
	
Class.forName
	
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


# 建立连接

## 作用

	建立和数据库的连接

## 方式

	Connection con = DriverManager.getConnection(url, username, password);
