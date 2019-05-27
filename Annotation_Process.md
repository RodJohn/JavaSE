
# 原理


	Java提供的javac.exe工具有一个-processor选项，该选项可指定一个Annotation处理器，
	如果在编译java源文件的时候通过该选项指定了Annotation处理器，
	那么这个Annotation处理器，将会在编译时提取并处理Java源文件中的Annotation。


Processor

	每个Annotation处理器都需要实现javax.annotation.processing包下的Processor接口。
	不过实现该接口必须实现它里面所有方法，因此通常采用继承AbstractProcessor的方式来实现Annotation处理器
	，一个Annotation处理器可以处理一种或多种Annotation类型。



# 示例


	定义注解

	定义解析器

	注册解析器

	使用解析器编译

	示例
	
	javac操作
		https://www.cnblogs.com/xiazdong/p/3216220.html
