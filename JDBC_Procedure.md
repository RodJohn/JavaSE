

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
	



