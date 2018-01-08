---
layout: post
title: Win10下安装MXNet和Tensorflow
category: 工具
tags: 深度学习
keywords: MXNet, Tensorflow
discription:
---
在Windows上安装深度学习框架确实废了我不小的功夫，所以在这里记录下来以便以后查看。

我主要关注的深度学习框架是Tensorflow, Pytorch, MNNet这三个。其中 Pytorch 在 Windows 上装不了，而对于 Tensorflow, Windows 对 Python3 比较支持，而 MXNet却支持 Python2， 确实很烦，要同时兼容 Python2 和 Python3 才行。
1. 在Windows上同时安装 Python2 和 Python3.  
在这里我们通过 Anaconda 来安装 python, 下载 Anaconda2 并将其作为主环境。按照安装顺序，如果需要的话改变一下默认目录，并勾选两个选项。安装好之后打开CMD， 执行下面的命令创建一个 Python3 虚拟环境。   
`
conda create -n py3 python=3.5
`  
当然如果你有强迫症的话可以再创建一个对称的 py2  
`
conda create -n py2 python=2.7
`  
这里就用原来的了。
