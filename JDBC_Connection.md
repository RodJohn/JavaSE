# Connection


	代表数据库的链接
	

# 创建语句

	Statement createStatement()  
		静态语句
  	PreparedStatement prepareStatement(String sql)
		预编译语句
	CallableStatement prepareCall(String sql)
		存储过程
   
   
# 事务控制

控制语句

	void setAutoCommit(boolean autoCommit)
		设置自动提交
	void commit() 
		提交语句
	void rollback() 
		回滚语句


常用模式

	try {
		con.setAutoCommit(false);
		….
		…
		con.commit();
	} catch() {
		con.rollback();
	}


