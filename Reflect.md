# 反射

作用

  获取 和 调用

JAVA反射机制是在运行状态中，
对于任意一个类，都能够知道这个类的所有属性和方法；
对于任意一个对象，都能够调用它的任意一个方法和属性；
这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。

要想解剖一个类,必须先要获取到该类的字节码文件对象。而解剖使用的就是Class类中的方法.所以先要获取到每一个字节码文件对应的Class类型的对象.

反射就是把java类中的各种成分映射成一个个的Java对象
例如：一个类有：成员变量、方法、构造方法、包等等信息，利用反射技术可以对一个类进行解剖，把个个组成部分映射成一个个对象。
     （其实：一个类中这些成员方法、构造方法、在加入类中都有一个类来描述）
     
# Class对象
     
     
如图是类的正常加载过程：反射的原理在与class对象。
熟悉一下加载的时候：Class对象的由来是将class文件读入内存，并为之创建一个Class对象。



https://www.jianshu.com/p/9be58ee20dee

# 获取Class对象

/**
 * 获取Class对象的三种方式
 * 1 Object ——> getClass();
 * 2 任何数据类型（包括基本数据类型）都有一个“静态”的class属性
 * 3 通过Class类的静态方法：forName（String  className）(常用)
 *
 */
 
 
 在运行期间，一个类，只有一个Class对象产生
 
public class Fanshe {
    public static void main(String[] args) {
        //第一种方式获取Class对象  
        Student stu1 = new Student();//这一new 产生一个Student对象，一个Class对象。
        Class stuClass = stu1.getClass();//获取Class对象
        System.out.println(stuClass.getName());

        //第二种方式获取Class对象
        Class stuClass2 = Student.class;
        System.out.println(stuClass == stuClass2);//判断第一种方式获取的Class对象和第二种方式获取的是否是同一个

        //第三种方式获取Class对象
        try {
            Class stuClass3 = Class.forName("fanshe.Student");//注意此字符串必须是真实路径，就是带包名的类路径，包名.类名
            System.out.println(stuClass3 == stuClass2);//判断三种方式是否获取的是同一个Class对象
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

    }
}

# 构造方法



1.获取构造方法：
  1).批量的方法：
public Constructor[] getConstructors()：所有”公有的”构造方法
            public Constructor[] getDeclaredConstructors()：获取所有的构造方法(包括私有、受保护、默认、公有)
     
  2).获取单个的方法，并调用：
public Constructor getConstructor(Class… parameterTypes):获取单个的”公有的”构造方法：
public Constructor getDeclaredConstructor(Class… parameterTypes):获取”某个构造方法”可以是私有的，或受保护、默认、公有；

  调用构造方法：
Constructor–>newInstance(Object… initargs)

2、newInstance是 Constructor类的方法（管理构造函数的类）
api的解释为：
newInstance(Object… initargs)
           使用此 Constructor 对象表示的构造方法来创建该构造方法的声明类的新实例，并用指定的初始化参数初始化该实例。
它的返回值是T类型，所以newInstance是创建了一个构造方法的声明类的新实例对象。并为之调用


/* 
 * 通过Class对象可以获取某个类中的：构造方法、成员变量、成员方法；并访问成员； 
 *  
 * 1.获取构造方法： 
 *      1).批量的方法： 
 *          public Constructor[] getConstructors()：所有”公有的”构造方法 
            public Constructor[] getDeclaredConstructors()：获取所有的构造方法(包括私有、受保护、默认、公有) 
      
 *      2).获取单个的方法，并调用： 
 *          public Constructor getConstructor(Class… parameterTypes):获取单个的”公有的”构造方法： 
 *          public Constructor getDeclaredConstructor(Class… parameterTypes):获取”某个构造方法”可以是私有的，或受保护、默认、公有； 
 *       
 *          调用构造方法： 
 *          Constructor–>newInstance(Object… initargs) 
 */  
 
 
 public class Constructors {  
  
    public static void main(String[] args) throws Exception {  
        //1.加载Class对象  
        Class clazz = Class.forName(”fanshe.Student”);  
          
         
          
        System.out.println(”*****************获取公有、无参的构造方法*******************************”);  
        Constructor con = clazz.getConstructor(null);  
        //1>、因为是无参的构造方法所以类型是一个null,不写也可以：这里需要的是一个参数的类型，切记是类型  
        //2>、返回的是描述这个无参构造函数的类对象。  
      
        System.out.println(”con = ” + con);  
        //调用构造方法  
        Object obj = con.newInstance();  
    //  System.out.println(“obj = ” + obj);  
    //  Student stu = (Student)obj;  
          
        System.out.println(”******************获取私有构造方法，并调用*******************************”);  
        con = clazz.getDeclaredConstructor(char.class);  
        System.out.println(con);  
        //调用构造方法  
        con.setAccessible(true);//暴力访问(忽略掉访问修饰符)  
        obj = con.newInstance(’男’);  
    }  
      
} 





# Field


/* 
 * 获取成员变量并调用： 
 *  
 * 1.批量的 
 *      1).Field[] getFields():获取所有的”公有字段” 
 *      2).Field[] getDeclaredFields():获取所有字段，包括：私有、受保护、默认、公有； 
 * 2.获取单个的： 
 *      1).public Field getField(String fieldName):获取某个”公有的”字段； 
 *      2).public Field getDeclaredField(String fieldName):获取某个字段(可以是私有的) 
 *  
 *   设置字段的值： 
 *      Field –> public void set(Object obj,Object value): 
 *                  参数说明： 
 *                  1.obj:要设置的字段所在的对象； 
 *                  2.value:要为字段设置的值； 
 *  
 */  
 
 # Method
 
 /* 
 * 获取成员方法并调用： 
 *  
 * 1.批量的： 
 *      public Method[] getMethods():获取所有”公有方法”；（包含了父类的方法也包含Object类） 
 *      public Method[] getDeclaredMethods():获取所有的成员方法，包括私有的(不包括继承的) 
 * 2.获取单个的： 
 *      public Method getMethod(String name,Class<?>… parameterTypes): 
 *                  参数： 
 *                      name : 方法名； 
 *                      Class … : 形参的Class类型对象 
 *      public Method getDeclaredMethod(String name,Class<?>… parameterTypes) 
 *  
 *   调用方法： 
 *      Method –> public Object invoke(Object obj,Object… args): 
 *                  参数说明： 
 *                  obj : 要调用方法的对象； 
 *                  args:调用方式时所传递的实参； 
 
): 
 */  
 
 # 
 
 public class SampleInterface {  
public static void main(String[] args) throws Exception {  
A raf = new A();  
printInterfaceNames(raf);  
}  
  
public static void printInterfaceNames(Object o) {  
Class c = o.getClass();  
//获取反射类的接口  
Class[] theInterfaces = c.getInterfaces();  
for(int i=0; i<theInterfaces.length; i++)  
System.out.println(theInterfaces[i].getName());  
//获取反射类的父类（超类）  
Class theSuperclass = c.getSuperclass();  
System.out.println(theSuperclass.getName());  
}  
}

 
 # Annotation
 
 
 # 反射方法的其它使用之—通过反射越过泛型检查
 
 
 
 /*
 * 通过反射越过泛型检查
 * 
 * 例如：有一个String泛型的集合，怎样能向这个集合中添加一个Integer类型的值？
 */
public class Demo {
    public static void main(String[] args) throws Exception{
        ArrayList<String> strList = new ArrayList<>();
        strList.add("aaa");
        strList.add("bbb");

    //  strList.add(100);
        //获取ArrayList的Class对象，反向的调用add()方法，添加数据
        Class listClass = strList.getClass(); //得到 strList 对象的字节码 对象
        //获取add()方法
        Method m = listClass.getMethod("add", Object.class);
        //调用add()方法
        m.invoke(strList, 100);

        //遍历集合
        for(Object obj : strList){
            System.out.println(obj);
        }
    }
}
