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

## 控制语句

提交/回滚

	void setAutoCommit(boolean autoCommit)
		设置是否自动提交（默认自动提交）
	void commit() 
		提交语句
	void rollback() 
		回滚语句

保存点

	Savepoint setSavepoint(String savepointName)
		定义保存点
    void releaseSavepoint(Savepoint savepointName)
		删除保存点	
	void rollback (String savepointName)
		回滚到指定的保存点

## 示例

提交/回滚

	try{	
	   conn.setAutoCommit(false);
	   
	   Statement stmt = conn.createStatement();
	   String SQL = "INSERT ...";
	   stmt.executeUpdate(SQL);  
	   String SQL = "INSERT ...";
	   stmt.executeUpdate(SQL);	 
	   
	   conn.commit();
	}catch(SQLException se){
	   conn.rollback();
	}

保存点

	try{
	   conn.setAutoCommit(false);
	   
	   Statement stmt = conn.createStatement();
	   Savepoint savepoint1 = conn.setSavepoint("Savepoint1");
	   String SQL = "INSERT ...";
	   stmt.executeUpdate(SQL);  
	   String SQL = "INSERT ...";
	   stmt.executeUpdate(SQL);	 
	   
	   conn.commit();
	}catch(SQLException se){
	   conn.rollback(savepoint1);
	}


