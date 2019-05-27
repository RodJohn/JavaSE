
https://www.jianshu.com/p/28edf5352b63

# 重复注解

# Type Annotation


# 示例

定义

	@Target(ElementType.TYPE_USE)
	@Retention(RetentionPolicy.RUNTIME)
	@Documented
	public @interface NotNull {
		String value() default "";
	}

使用


	//implements实现接口中使用Type Annotation
	public class Test implements @NotNull(value="Serializable") Serializable{

			//泛型中使用Type Annotation  、   抛出异常中使用Type Annotation
		public  void foo(List<@NotNull String> list) throws @NotNull(value="ClassNotFoundException") ClassNotFoundException {
			//创建对象中使用Type Annotation
			Object obj =new @NotNull String("annotation.Test");
			//强制类型转换中使用Type Annotation
			String str = (@NotNull String) obj;
		}
	}






