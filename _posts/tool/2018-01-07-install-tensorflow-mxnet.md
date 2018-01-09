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
推荐先自己安装对应的显卡，因为 NVIDIA 有专门用于 Notebook 的显卡，安装该显卡后一般在控制面板有三个新程序。当然也可以不安装 exprience 那个软件。

2. 搭建 python2 和 python3 兼容环境  
在这里我们通过 Anaconda 来安装 python, 下载 Anaconda2 并将其作为主环境。按照安装顺序，如果需要的话改变一下默认目录，并勾选两个选项。安装好之后打开CMD， 执行下面的命令创建一个 Python3 虚拟环境。  
    ```
    conda create -n py3 python=3.5 anaconda
    ```  
    当然如果你有强迫症的话可以再创建一个对称的 py2  
    ```
    conda create -n py2 python=2.7 anaconda
    ```  
    这里就用原来的了。
    装好后可以测试一下，重新打开一个 CMD 窗口。 
    ```
    python
    exit()
    activate py3
    exit()
    deactivate py3
    ```
    如果没有错误并正常显示说明安装好了。  

3. 安装 CUDA8.0 和 cudnn6.0  
到官网上下载最新版的 CUDA8.0 和 cudnn6.0。然后直接运行 CUDA 安装文件，可以自定义一下需要安装的组件，如果之前已经安装过显卡了，则不需要再安装了。目录什么的无所谓，因为它只是临时目录，最后还是安装到了 C 盘。  
然后要测试一下 CUDA8.0 是不是安装正确。打开一个 CMD 窗口。输入  
    ```
    nvcc -V
    ```
    ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnae1w6df1j30ci06umx5.jpg)    
    会显示安装好了 CUDA8.0。然而这样还未成功，还要测试一下 CUDA samples 才行。

    进入目录 C:\ProgramData\NVIDIA Corporation\CUDA Samples\v8.0 （注意该目录是隐藏的，需要电脑勾选查看隐藏的项目），双击打开 Samples_vs2015.sln 这个文件，等 VS2015 加载好。
    注意选择 release 和 x64， 并在 1_Utilitis 上右键单击，点生成。
    如果显示生成5个成功，说明正确。然后打开 “C:\ProgramData\NVIDIA Corporation\CUDA Samples\v8.0\bin\win64\Release” 目录，发现有一些文件生成了。  

    打开 CMD 窗口，进入到这个 Release 目录， 先运行 deviceQuery.exe 程序，就是在当前目录的 CMD 窗口输入 ` deviceQuery.exe` 并回车, 在结尾出现 result = PASS 说明正确。  
    然后在当前目录输入 `bandwidthTest.exe` 并回车， 同样看看是否 result = PASS 。如果是的话说明完全正确。 

    尴尬的是我这里出现了 cudaErrorLaunchTimeout 错误，NVIDIA 管网上对这个错误的解释是“This indicates that the device kernel took too long to execute. This can only occur if timeouts are enabled - see the device property kernelExecTimeoutEnabled for more information. The device cannot be used until cudaThreadExit() is called. All existing device memory allocations are invalid and must be reconstructed if the program is to continue using CUDA.”。额，不太懂。。。。可能是系统限制了执行时间,我暂时没解决。
    按照网上的一个做法可以修改注册表中驱动的一个属性值，详见[这里](https://stackoverflow.com/questions/17186638/modifying-registry-to-increase-gpu-timeout-windows-7)。  

    接着安装 cudnn6.0。直接解压该文件压缩包，看到里面有三个文件。  
    ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnadjfmozoj30gu05ijre.jpg)  
    直接将这三个文件夹一起复制到 C:\ProgramData\NVIDIA GPU Computing Toolkit\v8.0 目录里。现在打开系统环境变量设置，确认 CUDA_PATH 和 CUDA_PATH_V8.0 已经存在。添加 C:\ProgramData\NVIDIA GPU Computing Toolkit\v8.0\bin 到 Path 里面,确定并退出。
    ![](http://ww1.sinaimg.cn/thumbnail/006CYpBYgy1fnae1w6df1j30ci06umx5.jpg)

