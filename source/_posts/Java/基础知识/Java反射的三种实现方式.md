---
title: "Java反射的三种实现方式"
date: 2016-06-15 21:22:30
draft: true
tags: [Java]
categories:
- [Java]
---

Java反射的三种实现方式

Foo foo = new Foo();


```java
第一种：通过Object类的getClass方法
Class cla = foo.getClass();

第二种：通过对象实例方法获取对象
Class cla = foo.class;

第三种：通过Class.forName方式
Class cla = Class.forName("xx.xx.Foo");

对于有空构造函数的类 可以直接用字节码文件获取实例：
Object o = clazz.newInstance();　　// 会调用空参构造器 如果没有则会报错

对于没有空的构造函数的类则需要先获取到他的构造对象 在通过该构造方法类获取实例：
Constroctor constroctor = clazz.getConstructor(String.class, int.class); // 获取构造函数 
Object obj = constroctor.newInstance("jack", 18); // 通过构造器对象的newInstance方法进行对象的初始化

Constroctor constroctor = clazz.getConstructors(); // 获取所有的构造函数 

```

```java
public class ProgramMain {
    public static void main(String[] args) throws Exception {
        Persons person = getInstance("english");
        person.say();

        Persons person2 = getInstanceByReflector(Chinese.class);
        person2.say();
    }

    /**
     *
     * @param name
     * @return
     * @throws Exception
     */
    public static Persons getInstance(String name) throws Exception {
        Persons person = null;

        if (name.equalsIgnoreCase("Chinese")) {
            person = new Chinese();
        } else if (name.equalsIgnoreCase("English")) {
            person = new English();
        } else {
            throw new Exception("请传递正确的参数");
        }

        return person;
    }

	/**
     * 通过反射
     * @param personClass
     * @return
     * @throws Exception
     */
    public static Persons getInstanceByReflector(Class personClass) throws Exception {
        Persons person = null;
        try {
            person = (Persons) personClass.getConstructor().newInstance();
        } catch (Exception e) {
            throw new Exception("请传入正确的参数");
        }

        return person;
    }
}
```

#####        获取构造函数
1、知道构造函数精确的参数类型的
```java
Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
Constructor c=c1.getConstructor(String.class,Integer.class);//参数可以为空，根据知道的参数类型和个数来写
```

2、不知道构造函数精确的参数类型
```java
Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
	Constructor[] constructors=c1.getConstructors();
	for (int i = 0; i < constructors.length; i++) {
		 Class [] parameters= constructors[i].getParameterTypes();//获得构造函数的参数列表
		 if(parameters.length<=0)
		 {
			 
			 System.out.println("一个无参的构造函数");
		 }
		 else
		 {
			 String [] typeName=new String[parameters.length];
			 for(int j=0;j<typeName.length;j++)
			 {
				 typeName[j]= parameters[j].getSimpleName();//获取参数的类型名	 
			 }
			 System.out.println(String.join(",", typeName));
		 }
	}
```

##### 创建实例对象
通过构造函数去创建，调用Constructor对象的newInstance()方法创建对象。
		

```java
Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
Chinese chinese=(Chinese) c1.getConstructor().newInstance();
chinese.say();
```


##### 获取方法并执行	

```java
1、获取所有的非private方法
		Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
		Object chinese= c1.getConstructor().newInstance();
		Method[] methods1= c1.getMethods();//获取所有的非private方法，并且父类的方法也会获取
		for(int i=0;i<methods1.length;i++)
		{
			Class[] parameters= methods1[i].getParameterTypes();
			if(parameters.length<=0)
			{
				 System.out.println(methods1[i].getName()+"()");
			}
			else
			{
				String [] typeName=new String[parameters.length];
				for(int j=0;j<parameters.length;j++)
				{
					
					typeName[j]=parameters[j].getSimpleName();
				}
				System.out.println(methods1[i].getName()+"("+String.join(",",typeName)+")");
			}

		}

2、获取所有的当前类定义的所有方法，包括私有方法
		Method[] methods1= c1.getDeclaredMethods();//获取当前类定义的所有，包括私有方法
3、获取指定方法名的方法
	    Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
		Object chinese= c1.getConstructor().newInstance();
	    Method setNumber=	c1.getDeclaredMethod("setNumber", Integer.class);//如果方法没有参数就可以不用写，另外需要注意参数类型和定义方法的参数类型保持一致

4、获取父类的私有方法
		Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
	    Class c2=	c1.getSuperclass();
	    //通过c2去获取父类中的私有方法

5、执行方法
		Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
		Object chinese= c1.getConstructor().newInstance();
	    Method setNumber=	c1.getDeclaredMethod("setNumber", Integer.class);//如果方法没有参数就可以不用写，另外需要注意参数类型和定义方法的参数类型保持一致
	    setNumber.invoke(chinese, 1);	//第一个参数是实例对象，后面的参数是调用方法所需的参数
```

##### 获取字段
```java

1、获取所有非private类型的字段，包括父类的
	 Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
	 Object object=	c1.getConstructor().newInstance();
	 Field [] fields=  c1.getFields();//或者所有的非private类型的字段，包括父类的
	 for(int i=0;i<fields.length;i++)
	 {
		 System.out.println(fields[i].getName());
	 }
	 
2、获取当前类所有字段，包括私有的
	 Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
	 Object object=	c1.getConstructor().newInstance();
	 Field [] fields=  c1.getDeclaredFields();//获取当前类中所有的字段
	 for(int i=0;i<fields.length;i++)
	 {
		 System.out.println(fields[i].getName());
	 }
	 
3、获取父类的所有字段，包括私有的

	    Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
	    Class c2=	c1.getSuperclass();
	    //通过c2去获取父类中的私有字段
4、获取指定的字段
	 Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
	 Object object=	c1.getConstructor().newInstance();
	 Field field=  c1.getDeclaredField("Age");

5、使用
	 Class c1=Class.forName("pers.pwz.cmdemo.Chinese");
	 Object object1=	c1.getConstructor().newInstance();
	 Field field=  c1.getDeclaredField("Age");
	 field.setAccessible(true);//如果字段是私有的，需要先这是允许访问
	 //取值
	 Object  object2=field.get(object1);
	 System.out.println(object2);
	 //赋值
	 field.set(object1, 21);
	 //取值
	 Object  object3=field.get(object1);
	 System.out.println(object3);


```