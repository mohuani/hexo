---
title: "PHP简单图形图像处理"
date: 2017-12-04 21:22:30
draft: true
tags: [PHP]
categories:
- [PHP]
---

#PHP简单图形图像处理


引语
--

 

php不仅仅局限于html的输出，还可以创建和操作各种各样的图像文件，如GIF、PNG、JPEG、WBMP、XBM等。

php还可以将图像流直接显示在浏览器中。

要处理图像，就要用到php的GD库。

ps：确保php.ini文件中可以加载GD库。可以在php.ini文件中找到“;extension=php_gd2.dll”，将选项前的分号删除，保存，再重启Apache服务器即可。


----------


步骤
--

 

在php中创建一个图像一般需要四个步骤：

 - 1.创建一个背景图像，以后的所有操作都是基于此背景。

 - 2.在图像上绘图等操作。

 - 3.输出最终图像。

 - 4.销毁内存中的图像资源。

 


----------


1.创建背景图像
--------

 

下面的函数可以返回一个图像标识符，代表了一个宽为x_size像素、高为y_size像素的背景，默认为黑色。



> resource imagecreatetruecolor(int x_size , int y_size)

在图像上绘图需要两个步骤：首先需要选择颜色。通过imagecolorallocate()函数创建颜色对象。

> int imagecolorallocate(resource image, int red, int green, int blue) 

然后将颜色绘制到图像上。

 >bool imagefill(resource image, int x, int y, int color) 

imagefill()函数会在image图像的坐标（x，y）处用color颜色进行填充。


----------


2.在图像上绘图
--------

 

> bool iamgeline(resource image, int begin_x, int begin_y, int end_x, int end_y, int color) 

imageline()函数用color颜色在图像image中画出一条从（begin_x,begin_y）到（end_x,end_y）的线段。

>bool imagestring(resource image, int font, int begin_x, int begin_y, string s, int color ) 

imagestring()函数用color颜色将字符串s画到图像image的（begin_x,begin_y）处（这是字符串的左上角坐标）。如果font等于1,2,3,4或5，则使用内置字体，同时数字代表字体的粗细。如果font字体不是内置的，则需要导入字体库后使用。

 


----------


3.输出最终图像
--------

 

创建图像以后就可以输出图形或者保存到文件中了，如果需要输出到浏览器中需要使用header()函数发送一个图形的报头“欺骗”浏览器，使它认为运行的php页面是一个图像。

>header("Content-type: image/png"); 

发送数据报头以后，利用imagepng()函数输出图形。后面的filename可选，代表生成的图像文件的保存名称。

> bool image(resource image [, string filename]) 

 


----------


4.销毁相关的内存资源
-----------

 

最后需要销毁图像占用的内存资源。

> bool imagedestroy(resource image) 

**例子:**

```

 <?php
	$width=300;                                          //图像宽度
	$height=200;                                         //图像高度
    $img=imagecreatetruecolor($width,$height);           //创建图像
    $white=imagecolorallocate($img,255,255,255);         //白色
    $black=imagecolorallocate($img,0,0,0);               //黑色
    $red=imagecolorallocate($img,255,0,0);               //红色
    $green=imagecolorallocate($img,0,255,0);             //绿色
    $blue=imagecolorallocate($img,0,0,255);              //蓝色
    imagefill($img,0,0,$white);                          //将背景设置为白色
    imageline($img,20,20,260,150,$red);                  //画出一条红色的线
    imagestring($img,5,50,50,"hello,world!!",$blue);     //显示蓝色的文字
    header("content-type: image/png");                   //输出图像的MIME类型
    imagepng($img);                                      //输出一个PNG图像数据
    imagedestroy($img);                                   //清空内存
  ?>
```