---
layout: post
title: 在 Ubantu 16.04 里安装 Pytorch
category: 工具
tags: Pytorch
kewords: Pytorch
description:
---
虽然有的博客讲了怎么在 Windows 上安装 Pytorch 的，但是官方并未说 Pytorch 会支持 Windows ， 可能会出现各种问题，所以我还是在 Linux 上装一下吧。由于我用的是虚拟机，所以安装的是 CPU 版本的。

1. 安装 Anaconda。  
首先到 Anaconda 官网上下载最新版的 Anaconda 3.6, Pytorch 支持 3.6 版本。下载后一般是在 Downloads 文件夹内。打开终端，运行
    ```bash
    cd Downloads
    bash Anaconda3-5.0.1-Linux-x86_64.sh
    ```
     然后就会进入到安装过程，根据提示进行操作，需要注意的是最后问是否要添加到系统路径的时候，需要输入 Yes, 程序默认是 No。  
    当然如果这一步忘记设置了，后面可以自己设定。
    打开终端，输入以下命令
    ```bash
    sudo gedit ~/.bashrc 
    export PATH="/home/user/anaconda3/bin:$PATH"
    ```
    其中user为自己的 ubantu 账户名。
2. 安装pytorch  
打开 Pytorch 官网，选择对应的安装选项  
![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnfvwujnhmj30t60bcdgw.jpg)  
在终端运行对应的安装命令，等待安装完成，十分方便。
    ```bash
    conda install pytorch-cpu torchvision -c pytorch
    ```
    这里会有个问题就是有的时候网络不好会出现 conda http error， 我之前尝试了几次都没有成功，之后早起安装成功了，成功显示如下。  
![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnfw1l9u4pj30kd09w75p.jpg)  
看网上说，安装 Pytorch 需要翻墙，我这里可以翻墙，所以有时安装不了可能是无法翻墙的缘故，如果不行的添加清华的源看看，打开终端，执行下面的命令。
    ```shell
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
    conda config --set show_channel_urls yes
    ```  
    如果还是安装不了的话可以进行源码安装，详细看这篇[博客](https://www.jianshu.com/p/50ef56ff79ae)。  
3. 测试 Pytorch  
现在找一个简单的例子测试一下 Pytorch。打开终端，进入 Python 环境，输入
    ```python
    import torch
    import numpy as np

    np_data = np.arange(6).reshape((2, 3))
    torch_data = torch.from_numpy(np_data)
    tensor2array = torch_data.numpy()
    print(
        '\nnumpy array:', np_data,          # [[0 1 2], [3 4 5]]
        '\ntorch tensor:', torch_data,      #  0  1  2 \n 3  4  5    [torch.LongTensor of size 2x3]
        '\ntensor to array:', tensor2array, # [[0 1 2], [3 4 5]]
    )
    ```  
    可以看到结果正确，这里的例子取自[莫烦 Python](https://morvanzhou.github.io/tutorials/machine-learning/torch/2-01-torch-numpy/)。可以看到结果正确，至此安装成功。
    ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnfwuaw1dxj30ke0620t1.jpg)