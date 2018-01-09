---
layout: post
title: Win10下安装MXNet和Tensorflow
category: 工具
tags: 深度学习
keywords: MXNet, Tensorflow
discription:
---
在Windows上安装深度学习框架确实废了我不小的功夫，所以在这里记录下来以便以后查看。

我主要关注的深度学习框架是Tensorflow, Pytorch, MNNet这三个。其中 Pytorch 在 Windows 上装不了，而对于 Tensorflow, Windows 对 Python3.5 比较支持，而 MXNet却只支持 Python2， 确实很烦，要同时兼容 Python2 和 Python3 才行。而且更新的Tensorflow不在支持 cudnn5.1， 所以这里注意要下载 cudnn6.0。  

1. 首先装好对应的显卡驱动和 VS2015 community 版。
2. 在Windows上同时安装 Python2 和 Python3.  
在这里我们通过 Anaconda 来安装 python, 下载 Anaconda2 并将其作为主环境。按照安装顺序，如果需要的话改变一下默认目录，并勾选两个选项。安装好之后打开CMD， 执行下面的命令创建一个 Python3 虚拟环境。   
`
conda create -n py3 python=3.5
`  
当然如果你有强迫症的话可以再创建一个对称的 py2  
`
conda create -n py2 python=2.7
`  
这里就用原来的了。
装好后可以测试一下，重新打开一个 CMD 窗口。 
```
python
exit()
activate py3
exit()
deactivate py3
```
3. 安装 CUDA8.0 和 cudnn6.0
到官网上下载最新版的 CUDA8.0 和 cudnn6.0。然后直接运行 CUDA 安装文件，可以自定义一下需要安装的组件，如果之前已经安装过显卡了，则不需要再
