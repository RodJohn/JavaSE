# DriverManager

# 注册驱动

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


# 建立连接

建立和数据库的连接

	Connection con = DriverManager.getConnection(url, username, password);
