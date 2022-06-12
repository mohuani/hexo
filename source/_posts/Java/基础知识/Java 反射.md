---
title: "Java 反射"
date: 2016-06-15 21:22:30
draft: true
tags: [Java]
categories:
- [Java]
---

##### 参考视频
反射——Java高级开发必须懂的 https://www.imooc.com/learn/199

------
#### Class类
-  在面向对象的世界里，万事万物皆对象。（java语言中，静态的成员、普通数据类型除外)
   类是不是对象呢?类是(哪个类的对象呢?)谁的对象呢?
   类是对象，类是java.lang.Class类的实例对象
- 这个对象到底如何表示
-  Class.forName("类的全称")
       不仅表示了，类的类类型，还代表了动态加载类
       请大家区分编译、运行
       编译时刻加载类是静态加载类， new创建对象是静态加载类，在编译时刻就需要加载所有的可能使用到的类， 运行时刻加载类是动态加载类
   
- 基本的数据类型
      void关键字  都存在类类型 
- Class类的基本API操作

#### 方法的反射
- 如何获取某个方法
    方法的名称和方法的参数列表才能唯一决定某个方法
- 方法反射的操作
   method.invoke(对象，参数列表)
- 为什么要用方法的反射
    why?指定方法名称调用方法
    举个实际应用的案例  ---->通过标准JavaBean的属性名获取其属性值
    BeanUtil类
- 通过Class,Method来认识泛型的本质


##### 反射-Class类的使用
```java
package com.imooc.reflect;

public class ClassDemo1 {
	public static void main(String[] args) {
		//Foo的实例对象如何表示
		Foo foo1 = new Foo();//foo1就表示出来了.
		//Foo这个类 也是一个实例对象，Class类的实例对象,如何表示呢
		//任何一个类都是Class的实例对象，这个实例对象有三种表示方式
		
		//第一种表示方式--->实际在告诉我们任何一个类都有一个隐含的静态成员变量class
		Class c1 = Foo.class;
		
		//第二中表达方式  已经知道该类的对象通过getClass方法
		Class c2 = foo1.getClass();
		
		/*官网 c1 ,c2 表示了Foo类的类类型(class type)
		 * 万事万物皆对象，
		 * 类也是对象，是Class类的实例对象
		 * 这个对象我们称为该类的类类型
		 * 
		 */
		
		//不管c1  or c2都代表了Foo类的类类型，一个类只可能是Class类的一个实例对象
		System.out.println(c1 == c2);
		
		//第三种表达方式
		Class c3 = null;
		try {
			c3 = Class.forName("com.imooc.reflect.Foo");
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println(c2==c3);
		
		//我们完全可以通过类的类类型创建该类的对象实例---->通过c1 or c2 or c3创建Foo的实例对象
		try {
			Foo foo = (Foo)c1.newInstance();//需要有无参数的构造方法
			foo.print();
		} catch (InstantiationException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IllegalAccessException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	
		
	}
}
class Foo{
	
	void print(){
		System.out.println("foo");
	}
}
```
##### 动态加载类
```Java
package com.imooc.reflect;

public class ClassDemo2 {
	public static void main(String[] args) {
		
		Class c1 = int.class;//int 的类类型
		Class c2 = String.class;//String类的类类型   String类字节码（自己发明的)
		Class c3 = double.class;
		Class c4 = Double.class;
		Class c5 = void.class;
		
		System.out.println(c1.getName());
		System.out.println(c2.getName());
		System.out.println(c2.getSimpleName());//不包含包名的类的名称
		System.out.println(c5.getName());
	}
}
```
##### 获取 方法信息 和 成员变量构造函数信息
```Java
package com.imooc.reflect;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class ClassUtil {
	/**
	 * 打印类的信息，包括类的成员函数、成员变量(只获取成员函数)
	 * @param obj 该对象所属类的信息
	 */
	public static void printClassMethodMessage(Object obj){
		//要获取类的信息  首先要获取类的类类型
		Class c = obj.getClass();//传递的是哪个子类的对象  c就是该子类的类类型
		//获取类的名称
		System.out.println("类的名称是:"+c.getName());
		/*
		 * Method类，方法对象
		 * 一个成员方法就是一个Method对象
		 * getMethods()方法获取的是所有的public的函数，包括父类继承而来的
		 * getDeclaredMethods()获取的是所有该类自己声明的方法，不问访问权限
		 */
		Method[] ms = c.getMethods();//c.getDeclaredMethods()
		for(int i = 0; i < ms.length;i++){
			//得到方法的返回值类型的类类型
			Class returnType = ms[i].getReturnType();
			System.out.print(returnType.getName()+" ");
			//得到方法的名称
			System.out.print(ms[i].getName()+"(");
			//获取参数类型--->得到的是参数列表的类型的类类型
			Class[] paramTypes = ms[i].getParameterTypes();
			for (Class class1 : paramTypes) {
				System.out.print(class1.getName()+",");
			}
			System.out.println(")");
		}
	}
    /**
     * 获取成员变量的信息
     * @param obj
     */
	public static void printFieldMessage(Object obj) {
		Class c = obj.getClass();
		/*
		 * 成员变量也是对象
		 * java.lang.reflect.Field
		 * Field类封装了关于成员变量的操作
		 * getFields()方法获取的是所有的public的成员变量的信息
		 * getDeclaredFields获取的是该类自己声明的成员变量的信息
		 */
		//Field[] fs = c.getFields();
		Field[] fs = c.getDeclaredFields();
		for (Field field : fs) {
			//得到成员变量的类型的类类型
			Class fieldType = field.getType();
			String typeName = fieldType.getName();
			//得到成员变量的名称
			String fieldName = field.getName();
			System.out.println(typeName+" "+fieldName);
		}
	}
	/**
	 * 打印对象的构造函数的信息
	 * @param obj
	 */
	public static void printConMessage(Object obj){
		Class c = obj.getClass();
		/*
		 * 构造函数也是对象
		 * java.lang. Constructor中封装了构造函数的信息
		 * getConstructors获取所有的public的构造函数
		 * getDeclaredConstructors得到所有的构造函数
		 */
		//Constructor[] cs = c.getConstructors();
		Constructor[] cs = c.getDeclaredConstructors();
		for (Constructor constructor : cs) {
			System.out.print(constructor.getName()+"(");
			//获取构造函数的参数列表--->得到的是参数列表的类类型
			Class[] paramTypes = constructor.getParameterTypes();
			for (Class class1 : paramTypes) {
				System.out.print(class1.getName()+",");
			}
			System.out.println(")");
		}
	}
}
```
```Java
package com.imooc.reflect;

public class ClassDemo3 {
	public static void main(String[] args) {
		String s = "hello";
		ClassUtil.printClassMethodMessage(s);
		
	    Integer n1 = 1;
	    ClassUtil.printClassMethodMessage(n1);
	}
}
```
```Java
package com.imooc.reflect;

public class ClassDemo5 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		ClassUtil.printConMessage("hello");
		ClassUtil.printConMessage(new Integer(1));
	}
}
```
##### 方法反射的基本操作

```Java
package com.imooc.reflect;

import java.lang.reflect.Method;

public class MethodDemo1 {
	public static void main(String[] args) {
	   //要获取print(int ,int )方法  1.要获取一个方法就是获取类的信息，获取类的信息首先要获取类的类类型
		A a1 = new A();
		Class c = a1.getClass();
		/*
		 * 2.获取方法 名称和参数列表来决定  
		 * getMethod获取的是public的方法
		 * getDelcaredMethod自己声明的方法
		 */
	    try {
			//Method m =  c.getMethod("print", new Class[]{int.class,int.class});
	    	Method m = c.getMethod("print", int.class,int.class);
	    	
	    	//方法的反射操作  
	    	//a1.print(10, 20);方法的反射操作是用m对象来进行方法调用 和a1.print调用的效果完全相同
	        //方法如果没有返回值返回null,有返回值返回具体的返回值
	    	//Object o = m.invoke(a1,new Object[]{10,20});
	    	Object o = m.invoke(a1, 10,20);
	    	System.out.println("==================");
	    	//获取方法print(String,String)
            Method m1 = c.getMethod("print",String.class,String.class);
            //用方法进行反射操作
            //a1.print("hello", "WORLD");
            o = m1.invoke(a1, "hello","WORLD");
            System.out.println("===================");
            //Method m2 = c.getMethod("print", new Class[]{});
            Method m2 = c.getMethod("print");
            // m2.invoke(a1, new Object[]{});
            m2.invoke(a1);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} 
     
	}
}
class A{
	public void print(){
		System.out.println("helloworld");
	}
	public void print(int a,int b){
		System.out.println(a+b);
	}
	public void print(String a,String b){
		System.out.println(a.toUpperCase()+","+b.toLowerCase());
	}
}

```
通过反射了解集合泛型的本质
```Java
package com.imooc.reflect;

import java.lang.reflect.Method;
import java.util.ArrayList;

public class MethodDemo4 {
	public static void main(String[] args) {
		ArrayList list = new ArrayList();
		
		ArrayList<String> list1 = new ArrayList<String>();
		list1.add("hello");
		//list1.add(20);错误的
		Class c1 = list.getClass();
		Class c2 = list1.getClass();
		System.out.println(c1 == c2);
		//反射的操作都是编译之后的操作
		
		/*
		 * c1==c2结果返回true说明编译之后集合的泛型是去泛型化的
		 * Java中集合的泛型，是防止错误输入的，只在编译阶段有效，
		 * 绕过编译就无效了
		 * 验证：我们可以通过方法的反射来操作，绕过编译
		 */
		try {
			Method m = c2.getMethod("add", Object.class);
			m.invoke(list1, 20);//绕过编译操作就绕过了泛型
			System.out.println(list1.size());
			System.out.println(list1);
			/*for (String string : list1) {
				System.out.println(string);
			}*///现在不能这样遍历
		} catch (Exception e) {
		  e.printStackTrace();
		}
	}

}


```