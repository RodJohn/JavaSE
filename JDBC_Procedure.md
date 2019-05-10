

# 步骤
	
    1):加载注册驱动.
    2):获取连接对象.
    3):创建/获取语句对象
	4):设置参数
    5):执行SQL语句
    6):解析结果
	7):释放资源
	8):按需控制事务
	
	
# 示例

	public class Jdbc {
	    public static void main(String[] args) {
		Connection conn = null;
		Statement stmt = null;
		try {
		    Class.forName("com.mysql.jdbc.Driver");
		    conn = DriverManager.getConnection("url", "user", "password");
		    stmt = conn.createStatement();
		    String sql = "SELECT ...";
		    ResultSet rs = stmt.executeQuery(sql);
		    while (rs.next()) {
				System.out.print(rs.getObject(0));
		    }
		    rs.close();
		} catch (Exception e) {
		    e.printStackTrace();
		} finally {
		    try {
			if (stmt != null)
			    stmt.close();
		    } catch (SQLException e) {
				e.printStackTrace();
		    }
		    try {
			if (conn != null)
			    conn.close();
		    } catch (SQLException se) {
				se.printStackTrace();
		    }
		}
	    }
	}





