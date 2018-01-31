---
layout: post
title: 在 Win10 上安装 Caffe
category: 工具
tags: Caffe
keywords: Caffe
description:
---
最近，看了几篇论文都是用的 Caffe 这个深度学习框架，实际上 Caffe 在工业界用的还是蛮多的，所以打算学习一下。

根据官方的说明，必须的环境是：
* VS2015 或者 2013（ Ninja 也可以）
* CMake 版本大于 3.4
* cmake.exe 和 python.exe 在系统环境变量 PATH 中  

可选的是：
* Anaconda Python 2.7 or 3.5 x 64 (or Miniconda)
* Matlab 接口
* CUDA 7.5 or 8.0 (用VS2015的话要用8)
* cuDNN v5 （其实V6等也可以）

我的环境是：
* VS 2015
* CMake 3.9
* Anaconda 2.7
* CUDA 8.0
* cuDNN v6.0

其实之前我在另一篇里已经介绍过安装 VS 2015, Anaconda 2.7, CUDA 8.0, cuDNN v6.0,这里沿用之前的就好了，所以只需要先安装 CMake 就行。
## 安装
### 下载 CMake 和 Caffe 的依赖包。
* [CMake](https://cmake.org/download/)
* [Caffe 依赖](https://github.com/willyd/caffe-builder/releases/ )
下好后 CMake 一路安装就行，Caffe 依赖包后边再用。
### 配置 Caffe
官网指示：
```
The fastest method to get started with caffe on Windows is by executing the following commands in a cmd prompt (we use C:\Projects as a root folder for the remainder of the instructions):
```
```bash
C:\Projects> git clone https://github.com/BVLC/caffe.git
C:\Projects> cd caffe
C:\Projects\caffe> git checkout windows
:: Edit any of the options inside build_win.cmd to suit your needs
C:\Projects\caffe> scripts\build_win.cmd
```
1. 首先打开 CMD，切换到打算安装 Caffe 的路径，然后按照官网上的指示，执行 Git 等一系列命令，先执行前三步。注意如果你的 Windows CMD 无法执行 git 命令，我建议下载 [git for windows](http://gitforwindows.org/) 工具，然后根据这个[教程](https://jingyan.baidu.com/article/9f7e7ec0b17cac6f2815548d.html)安装。然后根据你的 Windows CMD 中能不能使用 git 的配置选择直接在 Windows CMD 中执行命令还是在 Git CMD 中执行。我的是 Git CMD。
![](http://ww1.sinaimg.cn/mw690/006CYpBYly1fnzzofg0d0j30fk071mx4.jpg)
2. 接着根据自己的情况修改 caffe\scripts\bulid_win.cmd 里代码，使用文件编辑器（如 Sublime ） 打开这个文件（不要双击）。
    1. 修改 23-26 行
        ```bash
        :: Set python 2.7 with conda as the default python
        if !PYTHON_VERSION! EQU 2 (
            set CONDA_ROOT=C:\Miniconda-x64
        )
        ```  
         这里的 *C:\Miniconda-x64* 路径要改成自己的 Anaconda2 安装路径，比如说我的就改为 *D:\Anaconda2*
    2. 修改 69-100 行  
        ::为注释符，后面说明了相关的设置。
        ```bash
        ) else (
            :: Change the settings here to match your setup
            :: Change MSVC_VERSION to 12 to use VS 2013
            if NOT DEFINED MSVC_VERSION set MSVC_VERSION=14 
            :: Change to 1 to use Ninja generator (builds much faster)
            if NOT DEFINED WITH_NINJA set WITH_NINJA=1
            :: Change to 1 to build caffe without CUDA support
            if NOT DEFINED CPU_ONLY set CPU_ONLY=0
            :: Change to generate CUDA code for one of the following GPU architectures
            :: [Fermi  Kepler  Maxwell  Pascal  All]
            if NOT DEFINED CUDA_ARCH_NAME set CUDA_ARCH_NAME=Auto
            :: Change to Debug to build Debug. This is only relevant for the Ninja generator the Visual Studio generator will generate both Debug and Release configs
            if NOT DEFINED CMAKE_CONFIG set CMAKE_CONFIG=Release
            :: Set to 1 to use NCCL
            if NOT DEFINED USE_NCCL set USE_NCCL=0
            :: Change to 1 to build a caffe.dll
            if NOT DEFINED CMAKE_BUILD_SHARED_LIBS set CMAKE_BUILD_SHARED_LIBS=0
            :: Change to 3 if using python 3.5 (only 2.7 and 3.5 are supported)
            if NOT DEFINED PYTHON_VERSION set PYTHON_VERSION=2
            :: Change these options for your needs.
            if NOT DEFINED BUILD_PYTHON set BUILD_PYTHON=1
            if NOT DEFINED BUILD_PYTHON_LAYER set BUILD_PYTHON_LAYER=1
            if NOT DEFINED BUILD_MATLAB set BUILD_MATLAB=0
            :: If python is on your path leave this alone
            if NOT DEFINED PYTHON_EXE set PYTHON_EXE=python
            :: Run the tests
            if NOT DEFINED RUN_TESTS set RUN_TESTS=0
            :: Run lint
            if NOT DEFINED RUN_LINT set RUN_LINT=0
            :: Build the install target
            if NOT DEFINED RUN_INSTALL set RUN_INSTALL=0
        )
        ```
        在这里主要是设置 *MSVC_VERSION=14*，*WITH_NINJA=0*，*CPU_ONLY=0*，*BUILD_MATLAB=1*，分别表示使用 VS 2015，不用 Ninja，使用 GPU，使用 Matlab 接口，保存。
3. 在 CMD 中运行最后一句命令。  
`scripts\build_win.cmd`  
![](http://ww1.sinaimg.cn/mw690/006CYpBYly1fo00ergsd4j30qs0ctdg1.jpg)  
当你执行最后一句命令的时候，它会下载一个依赖包，并且自动在 C 盘用户文件夹上生成 .caffe 文件夹，下载的依赖包就放到里面。但是这里经常下载不了而导致出现错误，就像上面一样。所以推荐自己下载然后放在指定位置。把你下载的Caffe 依赖包压缩文件放到 C 盘用户文件夹下生成 .caffe 文件夹，如我的位置是在 C:\Users\wangy\\.caffe\dependencies\download（这个位置可以在运行这个命令的界面中找到，将之前下载的文件放在这个位置，再重新运行此命令即可）。   
建议先运行 build_win.cmd 命令，如果速度太慢再把它关闭，这样就不需要自己创建对应的文件夹，只需把压缩文件拷贝就行。    
再运行一次 build_win.cmd 命令，这次不会有什么问题，等待一小段时间 CMake 就把 VS 2015 的项目给创建出来了。  
![](http://ww1.sinaimg.cn/mw690/006CYpBYly1fo00xfnsbgj30np0clweu.jpg)
4. 生成项目  
在 caffe\build 文件夹下就生成了 caffe.sln，用 VS 2015 将其打开，并在release x64 或者 Debug x64 生成 ALL_BUILD 文件即可。  
![](http://ww1.sinaimg.cn/mw690/006CYpBYly1fo00zujmifj30eb0cygm2.jpg)  
最后没有报错，那么配置就告一段落了。
## 测试
1. 下载 MNIST 数据集 [https://pan.baidu.com/s/1o7YrhKe](​https://pan.baidu.com/s/1o7YrhKe)，解压缩后将 mnist-test-leveldb 与 mnist-train-leveldb 文件夹放到 \examples\mnist 下。
2. 修改 lenet_train_test.prototxt 文件：
    ```bash
    //需要修改四处地方，如下红色部分标注  
    name: "LeNet"  
    layer {  
    name: "mnist"  
    type: "Data"  
    top: "data"  
    top: "label"  
    include {  
        phase: TRAIN  
    }  
    transform_param {  
        scale: 0.00390625  
    }  
    data_param {  
        source: "....省略/examples/mnist/mnist-train-leveldb" //改为你的绝对路径 ，比如我的是D:/Caffe/caffe/examples/mnist/mnist-train-leveldb
        batch_size: 64  
        backend: LEVELDB //格式改成LEVELDB  
    }  
    }  
    layer {  
    name: "mnist"  
    type: "Data"  
    top: "data"  
    top: "label"  
    include {  
        phase: TEST  
    }  
    transform_param {  
        scale: 0.00390625  
    }  
    data_param {  
        source: "....省略/examples/mnist/mnist-test-leveldb" //改为你的绝对路径  
        batch_size: 100  
        backend: LEVELDB  //格式改成LEVELDB  
    }  
    }  
    ```
3. 修改 lenet_solver.prototxt 文件：
    ```bash
    net: "....省略/examples/mnist/lenet_train_test.prototxt"  //改为绝对路径  

    snapshot_prefix: "....省略/examples/mnist/lenet" //改为绝对路径  

    solver_mode: GPU //选择GPU模式
    ```
4. 编写批处理文件 run.bat 内容如下：
    ```bash
    D:\Caffe\caffe\build\tools\Release\caffe.exe train --solver=D:\Caffe\caffe\examples\mnist\lenet_solver.prototxt
    Pause 
    ```
当然还是要根据自己的实际路径来写，特别是要注意的是 2 和 3 中路径中是 / 不要写成 \，但是在 4 中是 \ 不要写成 /， 否则会出现错误。因为 Windows CMD 中命令使用的分隔符是 \，而一般的文件中的分隔符是 /。而 . bat 文件可以看作是 Windows 命令。  
然后双击 run.bat 执行，可以看到运行结果。  
![](http://ww1.sinaimg.cn/mw690/006CYpBYly1fo01vgemqgj30qs0ctdg1.jpg)  
![](http://ww1.sinaimg.cn/mw690/006CYpBYly1fo01vv43dnj30qy0asdgd.jpg)  
如果没有错误的话说明安装成功了。
 