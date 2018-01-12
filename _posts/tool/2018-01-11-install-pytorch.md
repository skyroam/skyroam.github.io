---
layout: post
title: 在 Ubantu 16.04 里安装 Pytorch
category: 工具
tags: Pytorch
kewords: Pytorch
discription:
---
虽然有的博客讲了怎么在 Windows 上安装 Pytorch 的，但是官方并未说 Pytorch 会支持 Windows ， 可以会出现各种问题，所以我还是在 Linux 上装一下吧。由于我用的是虚拟机，所以安装的是 CPU 版本的。

1. 安装 Anaconda。  
首先到 Anaconda 官网上下载最新版的 Anaconda 3.6, Pytorch 支持 3.6 版本。下载后一般是在 Downloads 文件夹内。打开终端，运行  

    ```
    cd Downloads
    bash Anaconda3-5.0.1-Linux-x86_64.sh
    ```
     然后就会进入到安装过程，根据提示进行操作，需要注意的是最后问是否要添加到系统路径的时候，需要输入 Yes, 程序默认是 No。  
    当然如果这一步忘记设置了，后面可以自己设定。
    后续。。。。
