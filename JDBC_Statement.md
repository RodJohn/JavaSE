# Statement


# 类型

Statement 

	静态SQL
	(写死的SQL)
	
PreperedStatement

	预编译语句
	所有参数都由 ? 符号作为占位符，在执行SQL语句之前，必须为每个参数(占位符)提供值。
	
CallableStatement

	调用存储过程
	在执行语句之前，必须将值绑定到所有参数


区别
		
	PreparedStatement 
		可以防止SQL注入危机
		可能提高性能(预编译)


# 创建

	参考Connection


# 参数设置

	PreparedStatement、CallableStatement需要设置参数

setXxx
	
	void  setXxx(int parameterIndex, Xxx value)
	
	xxx表示数据类型,比如:String,int,Long等，
	parameterIndex:设置第几个占位符?(从1开始)，
	value:需要设置的参数值。
		
		


# 执行语句	


## Statement

execute

	boolean execute (String SQL)
	如果可以检索到ResultSet对象，则返回一个布尔值true; 否则返回false。
	使用此方法执行SQLDDL语句或需要使用真正的动态SQL，可使用于执行创建数据库，创建表的SQL语句等等。


executeUpdate

    int executeUpdate(String sql):
	
	执行DML(增删改)和DDL语句
	如果是执行DDL什么都不返回,执行DML返回受影响的行数。

executeQuery

    ResultSet executeQuery(String sql) :
	执行DQL语句，
	返回一个 ResultSet 对象。 


## PreparedStatement

	类似于Statement，只是不需要传递SQL参数
	int executeUpdate()
	ResultSet executeQuery() 
	
				

# 获取自动生成的主键

	在ResultSet中获取自动生成的主键

## Statement:

流程

	设置需要返回自动生成的主键
		st.executeUpdate(sql, Statement.RETURN_GENERATED_KEYS);	
		参数:autoGeneratedKeys,是否需要返回自动生成的主键
		常量值:Statement.RETURN_GENERATED_KEYS
		
	获取返回的含主键的结果集
		ResultSet rs = st.getGeneratedKeys();		       
	
示例

	String sql = "insert ...";
	st = conn.createStatement();
	st.executeUpdate(sql, Statement.RETURN_GENERATED_KEYS);	
	ResultSet rs = st.getGeneratedKeys();		        
	while(rs.next()){
		int id = rs.getInt(1);
		System.out.println(id);
	}

## PreparedStatement

	操作类似statement
	不同在于创建PreparedStatement对象,并且指定是否需要返回生成的主键
	PreparedStatement prepareStatement(String sql, int autoGeneratedKeys) 
	




# 批处理


    一次性提交多条语句给数据库批量处理
	通常情况下比单独提交处理更有效率.
	只针对更新（增，删，改）语句，不包括查询。

## Statement

特点

	一次性可以执行多条sql语句，需要编译多次。

流程

	添加sql语句，st.addBatch(sql)  
	批量处理sql语句，int[] st.executeBatch()
	清除缓存： st.clearBatch();

示例

	st = conn.createStatement();
	st.addBatch(sql1);  
	st.addBatch(sql2);  
	st.executeBatch();

## PreparedStatement

特点

	执行一条sql语句，编译一次，执行sql语句的参数不同。

流程

	添加批量参数：psmt.addBatch()    --添加实际参数，执行之前，需要执行psmt.setXxx()设置实际参数
	执行批处理：int[] psmt.executeBatch()
	清除缓存：pstm.clearBatch();

示例

	st = conn.prepareStatement(sql);
	st.setString(1, "aaa");
	st.addBatch();
	st.setString(1, "bbb");
	st.addBatch();
	st.executeBatch();
	st.clearBatch();







