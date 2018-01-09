---
layout: post
title: 电脑用的一些小技巧
category: 技术
tags: trick
keywords: trick
discription:
---
这里不时地添加一些自己在用电脑的过程中需要的一些小技巧。 

1. 一些快捷键  
    `Windos 键 + Print Screen 键` 全屏截图,会把截图存到电脑图片的屏幕截图里

    `Alt 键 + Print Screen 键` 窗口截图,会把截图存到剪贴板，然后可以粘贴

    `python -v` : 显示一些信息，比如导入某个包

    `python -V` : 显示版本信息

    `dir` : Windows 里查看目录文件

    `ls` : Linux 里查看目录文件
    
2. 在 Markdown 里调整图片的显示大小  
    * 嵌入HTML代码  
        1.固定图片显示大小：  
        ```html
        <img src="http://img.blog.csdn.net/20151129213701642" width=256 height=256 />
        ```  
        2.根据一定比例显示： 
        ```html
        <img src="http://img.blog.csdn.net/20151129213701642" width="50%" height="50%" />
        ```   
        3.如果想给图像加个标注，可以这么做：  
        ```html
        <center>
        <img src="http://img.blog.csdn.net/20151129213701642" width="25%" height="25%" />
        Figure 1. Lena
        </center> 
        ```
    * 当支持 kramdown 时，可以使用  
        ```
        ![img](url){:height="100" width="100"}
        ```
    
    
