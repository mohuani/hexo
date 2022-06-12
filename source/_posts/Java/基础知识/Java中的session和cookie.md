---
title: "Java中的session和cookie"
date: 2016-06-15 21:22:30
draft: true
tags: [Java]
categories:
- [Java]
---

##### cookie
cookie是小段文本信息，在网络服务器上生成，并发给浏览器，通过使用cookie可以记录一些请求数据。流览器将cookie以key/value的形式保存到客户端的某个指定目录下。
在使用cookie时，应确保客户端上允许使用cookie，如果客户端禁止使用cookie的话，将导致cookie失效。

```java
读取cookie
Cookie [] cookies=request.getCookies();
for(Cookie item:cookies)
{
   System.out.println(item.getName()+”:”+URLDecoder.decode(item.getValue()));
\\System.out.println(item.getName()+”:”+ item.getValue());

设置cookie
Cookie cookie=new Cookie(“userName”,”pwz”);
response.addCookie(cookie);
}
```

如果指定名称的cookie已经存在了，那么执行addCookie方法后，新添加的cookie值将会覆盖旧值。

注意
在向cookie保存的信息中如果包含了中文，则需要先使用java.net.URLDecoder类下的encode()方法对信息编码。而在读取这个cookie信息的时候，需要使用decode()方法对信息进行解码

##### cookie的作用域

```
1、setPath()
通过setPath可以限制cookies的使用范围，cookie默认的使用方范围实在它产生的目录及其目录之内的目录，产生目录外的目录中都不能使用次cookie。
假设在tomcat的webapp目录下有两个项目weba和webb，
如果在保存cookie时将setPath()的参数设置为“/”那么在weba和webb中都能使用次cookie;
如果设置为“/weba”或者”/web/”,那么只能在weba下面使用；
如果在weba中创建的cookie，而path设置为”/webb/”那么在weba中也无法使用，只能在webb中使用
2、setDomain()
A机所在的域：www.a.com,A有应用weba 
B机所在的域：b.com，B有应用webb 
1）在weba下面设置cookie的时候，增加cookie.setDomain(“.b.com”);，这样在webb下面就可以取到cookie。

2）输入url访问webapp_b的时候，必须输入域名才能解析。比如说在A机器输入：http://haha.b.com:8080/webb,可以获取weba在客户端设置的cookie，而B机器访问本机的应用，输入：http://localhost:8080/webb则不可以获得cookie。

3）设置了cookie.setDomain(“.b.com”);，还可以在默认的www.a.com下面共享
```


##### cookie的有效期

```
1、如果没有显示设置cookie的有效期，那么cookie的有效期就是浏览器关闭后失效
2、如果setMaxAge()的参数为整数，那么就cookie一直到指定时候后失效，这个过程中无论浏览器是否关闭，cookie中保存的信息都会有效。
3、如果setMaxAge()的参数为0，那么就表示删除该cookie。
4、如果setMaxAge()的参数为负数，那么cookie就是临时保存，不会持久化保存到浏览器指定的目录中去。当窗口或者子窗口关闭后就会失效。
5、setMaxAge的参数单位是秒
```



#### session
 session在网络中被称为会话，和cookie的功能类似，用于保存请求信息。用户可以通过session在引用程序的web页面之间进行跳转，使整个会话一直保存，知道浏览器关闭。但是如果一个会话长时间没有向服务器发出请求，那么这个会话会自动关闭失效。这个时间取决于服务器，例如，tomcat服务器默认为30秒，这个时间可以通过程序进行修改。
实际上，一个会话过程可以理解为一次打电话，通过从拿起电话开始，到挂断电话为止，在这过程中，你可以聊很多话题，甚至是重复的话题。一个会话也是


```
保存会话信息
session.setAttribute(String name,Object object);
name是会话信息中的名字，object是会话名字所代表的值。

读取会话信息
session.getAttribute(String name);
点用此方法的返回值是Object类型的，因此在使用之前需要对其进行类型转化。

移除指定的会话
session.removeAttribute(String name);
注：如果要需要的会话名不存在，那么将会抛出异常

设置session有效时间
在浏览器长时间没有向服务器发出请求的情况下，可以设置session在多少时间后过期
session.setMaxInactiveInterval(int time)//单位是秒

销毁session
session.invalidate();
session对象销毁后，就不可以在当前请求中继续使用该session对象了，如果销毁后再调用就会抛出异常。当浏览器重新发出请求后，服务器会重新创建一个session对象，之前的session对象失效。

```