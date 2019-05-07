



# 步骤

加载数据库驱动

	Class.forName("com.mysql.jdbc.Driver");
	原因
	
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

DriverManager


Connection

	它用于代表数据库的链接
	
	
Statement	
	
PreperedStatement

好处

ResultSet



# 注意点

SQL注入
批量SQL
事务
获取自增值
数据类型




https://blog.csdn.net/qq_22172133/article/details/81266048
https://blog.csdn.net/zdb292034/article/details/81705876


