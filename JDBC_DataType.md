


# NULL

	SQL使用NULL值和Java使用null是不同的概念。

	对原始数据类型使用包装类，并使用ResultSet对象的wasNull()方法来测试接收getXXX()方法的返回值的包装器类变量是否应设置为null。


	Statement stmt = conn.createStatement( );
	String sql = "SELECT id, first, last, age FROM Employees";
	ResultSet rs = stmt.executeQuery(sql);

	int id = rs.getInt(1);
	if( rs.wasNull( ) ) {
	 id = 0;
	}

# 时间类型

	java.sql.Date类映射到SQL DATE类型，
	java.sql.Time和java.sql.Timestamp类分别映射到SQL TIME和SQL TIMESTAMP数据类型。

