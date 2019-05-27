	
# 反射获取注解元数据

	反射可以提供类名、方法和实例变量对象。
	所有这些对象都有getAnnotation()这个方法用来返回注解信息

	Class businessLogicClass = BusinessLogic.class;

	for(Method method : businessLogicClass.getMethods()) {

	 Todo todoAnnotation = (Todo)method.getAnnotation(Todo.class);

	 if(todoAnnotation != null) {

	  System.out.println(" Method Name : " + method.getName());

	  System.out.println(" Author : " + todoAnnotation.author());

	  System.out.println(" Priority : " + todoAnnotation.priority());

	  System.out.println(" Status : " + todoAnnotation.status());

	 }

	}

# 
