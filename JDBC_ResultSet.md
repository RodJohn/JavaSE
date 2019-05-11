
# ResultSet

	ResultSet表示数据库结果集，是一个二维表
	使用游标标记当前行位置


# 类型

ResultSet类型

	ResultSet类型由创建连接提供的参数确定

	createStatement(int RSType, int RSConcurrency);
	prepareStatement(String SQL, int RSType, int RSConcurrency);
	prepareCall(String sql, int RSType, int RSConcurrency);

RSType

	ResultSet.TYPE_FORWARD_ONLY	(默认)
		光标只能在结果集中向前移动
	ResultSet.TYPE_SCROLL_INSENSITIVE
		光标可以向前和向后滚动，结果集对创建结果集后发生的数据库所做的更改不敏感
	ResultSet.TYPE_SCROLL_SENSITIVE
		光标可以向前和向后滚动，结果集对创建结果集之后发生的其他数据库的更改敏感

RSConcurrency

	ResultSet.CONCUR_READ_ONLY(默认)
		创建只读结果集
	ResultSet.CONCUR_UPDATABLE
		创建可更新的结果集

说明

	RSType，RSConcurrency只是JDBC的定义，
	一般数据库只支持ResultSet.TYPE_FORWARD_ONLY和ResultSet.CONCUR_READ_ONLY


# 方法

## 获得元数据

	ResultSetMetaData getMetaData()
	ResultSetMetaData包括表名，列名，列数据类型获取
	
## 光标移动

next()

	下一行

previous()

	上一行

	
## 查看结果集

	ResultSet接口包含数十种获取当前行数据的方法。
	每个可能的数据类型都有一个get方法，
	每个get方法有两个版本
	一个是采用列名称。另一个采用列索引。

示例

	String getString(String columnLabel)
	String getString(int columnIndex)

## 更新结果集

	更新结果集中的一行会更改ResultSet对象中当前行的列，但不会更改底层数据库中的列的值。 
	要更新数据库中的行，需要调用updateRow


	方法类似于查看结果集

# 常用数据类型转换表

