
# JDKProxy


面向接口

	1.要求目标对象实现了某个接口；
	2.会为接口中的声明的所有方法进行代理

动态代理

	不会像静态代理会产生许多重复代码
	


# 示例

代理接口

	public interface Helloworld {
	  void sayHello();
	}

目标对象

	public class HelloworldImpl implements HelloWorld {
		public void sayHello() {
			System.out.print("hello world");
		}
	}

代理回调类

	public class MyInvocationHandler implements InvocationHandler{
		private Object target;
		public MyInvocationHandler(Object target) {
			this.target=target;
		}
		public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
			System.out.println("method :"+ method.getName()+" is invoked!");
			return method.invoke(target,args);
		}
	}

生成代理对象

	public class JDKProxyTest {
		public static void main(String[]args) throws Exception {

			HelloWorld helloWorld=(HelloWorld)Proxy.newProxyInstance(
									JDKProxyTest.class.getClassLoader(),
									new Class<?>[]{HelloWorld.class},
									new MyInvocationHandler(new HelloworldImpl()));
			helloWorld.sayHello();

		}
	}













# 原理

https://rejoy.iteye.com/blog/1627405

http://www.importnew.com/23168.html







