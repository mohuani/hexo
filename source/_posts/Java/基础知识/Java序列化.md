---
title: "Java序列化"
date: 2016-06-15 21:22:30
draft: true
tags: [Java]
categories:
- [Java]
---

#### 序列化
Java 中提供了一种对象持久化保存的技术。常规的对象，在程序结束后就会被回收处理，如果想要对象能被持久化保存下来方便下次使用，就需要使用到对象的序列化和反序列化。
序列化成功的条件

```
1、该类必须实现java.io.Serializable接口
2、类的所有字段都必须是可序列化的（属性类型是实现类java.io.Serializable接口的）；
```

```
序列化是排除某个字段
只需要用transient关键字修饰这个字段。
```

##### 序列化

```java
package pers.pwz.cmdemo;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

public class ProgramMain {

	public static void main(String[] args)  {
		Persons p=new Persons();
		p.Age=10;
		p.Name="pwz";
		p.Height=170;
		p.Weight=60;
		try {
			File myFile=new File(".\\temp");
			if(!myFile.exists())

			{
				myFile.mkdirs();
			}
					
			FileOutputStream fo=new FileOutputStream(".\\temp\\persons.ser");
			ObjectOutputStream oo = new ObjectOutputStream(fo);
			oo.writeObject(p);
			oo.close();
			fo.close();
			
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	
	}
}

class Persons implements java.io.Serializable//这是个标记接口
{
	public int Age;
	public String Name;
	public double Height;
	public double Weight;
	public void sayHello()
	{
		System.out.println(Name+"你好");
	}
}
```

##### 反序列化

```java
package pers.pwz.cmdemo;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.ObjectInputStream;

public class ProgramMain {

	public static void main(String[] args)  {
		try {	
			FileInputStream fo=new FileInputStream(".\\temp\\persons.ser");
			ObjectInputStream oo = new ObjectInputStream(fo);
			Persons p=(Persons) oo.readObject();
			p.sayHello();
			oo.close();
			fo.close();
			
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	
	}
}

class Persons implements java.io.Serializable//这是个标记接口
{
	public int Age;
	public String Name;
	public double Height;
	public double Weight;
	public void sayHello()
	{
		System.out.println(Name+"你好");
	}
}

```