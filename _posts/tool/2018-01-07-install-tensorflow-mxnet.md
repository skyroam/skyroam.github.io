---
layout: post
title: Win10下安装MXNet和Tensorflow
category: 工具
tags: 深度学习
keywords: MXNet, Tensorflow
discription:
---
在Windows上安装深度学习框架并进行 GPU 加速确实废了我不小的功夫，中文的一些垃圾博客根本就是天下文章一大抄，搞得别人晕头转向而且出现各种错误，所以在这里记录下来以便以后查看。

我主要关注的深度学习框架是 Tensorflow, Pytorch, MNNet这三个。其中 Pytorch 在 Windows 上装不了，而对于 Tensorflow, Windows 上对 Python3.5 比较支持，而 MXNet 在 Windows 上只支持 Python2， 确实很烦，要同时兼容 Python2 和 Python3 才行。而且更新的 Tensorflow 不在支持 cudnn5.1， 所以这里注意要下载 cudnn6.0。所以能在 Linux 上跑尽量用 Linux 好了，但是我不想装双系统。另外，虚拟机的 Linux 没法用 GPU 加速。

1. 首先装好对应的显卡驱动和 VS2015 community 版。   
推荐先自己安装对应的显卡，因为 NVIDIA 有专门用于 Notebook 的显卡，注意和台式机的区别。安装该显卡后一般在控制面板有三个新程序。当然也可以不安装 exprience 那个软件。装 VS2015 的时候选择自定义，勾上编程语言里的 Visual C++ 那个选项，其他的默认就好了。

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
    python
    exit()
    deactivate py3
    ```  

    如果没有错误并正常显示说明安装好了。  

3. 安装 CUDA8.0 和 cudnn6.0  
到官网上下载最新版的 CUDA8.0 和 cudnn6.0。然后直接运行 CUDA 安装文件，可以自定义一下需要安装的组件，如果之前已经安装过显卡了，则不需要再安装了。目录什么的无所谓，因为它只是临时目录，最后还是安装到了 C 盘。  
![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnah6sayhuj30gf0c4aca.jpg)  
上面除了后面两个不勾其他的都选，下面的目录保持默认。  
![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnah7wchvmj30gj0c3go4.jpg)  
然后要测试一下 CUDA8.0 是不是安装正确。打开一个 CMD 窗口。输入
    ```
    nvcc -V
    ```
    ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnae1w6df1j30ci06umx5.jpg)  
会显示安装好了 CUDA8.0。然而这样还未成功，还要测试一下 CUDA samples 才行。  

     进入目录 C:\ProgramData\NVIDIA Corporation\CUDA Samples\v8.0 （注意该目录是隐藏的，需要电脑勾选查看隐藏的项目），双击打开 Samples_vs2015.sln 这个文件，等 VS2015 加载好。
    注意选择 release 和 x64， 并在 1_Utilitis 上右键单击，点生成。  
    ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnahutg7uyj30vc0983z4.jpg)  
    如果显示生成5个成功，说明正确。然后打开 C:\ProgramData\NVIDIA Corporation\CUDA Samples\v8.0\bin\win64\Release 目录，发现有一些文件生成了。  

    打开 CMD 窗口，进入到这个 Release 目录， 先运行 deviceQuery.exe 程序，就是在当前目录的 CMD 窗口输入 `deviceQuery.exe` 并回车, 在结尾出现 result = PASS 说明正确。  
    然后在当前目录输入 `bandwidthTest.exe` 并回车， 同样看看是否 result = PASS 。如果是的话说明完全正确。 

    尴尬的是我这里出现了 cudaErrorLaunchTimeout 错误，NVIDIA 管网上对这个错误的解释是 “This indicates that the device kernel took too long to execute. This can only occur if timeouts are enabled - see the device property kernelExecTimeoutEnabled for more information. The device cannot be used until cudaThreadExit() is called. All existing device memory allocations are invalid and must be reconstructed if the program is to continue using CUDA.” 。额，不太懂。。。。可能是系统限制了驱动的执行时间,我暂时没解决。
    按照网上的一个做法可以修改注册表中驱动的一个属性值，详见[这里](https://stackoverflow.com/questions/17186638/modifying-registry-to-increase-gpu-timeout-windows-7)。  

    接着安装 cudnn6.0。直接解压该文件压缩包，看到里面有三个文件。  
    ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnadjfmozoj30gu05ijre.jpg)  
    直接将这三个文件夹一起复制到 C:\ProgramData\NVIDIA GPU Computing Toolkit\v8.0 目录里。现在打开系统环境变量设置，确认 CUDA_PATH 和 CUDA_PATH_V8.0 已经存在。添加 C:\ProgramData\NVIDIA GPU Computing Toolkit\v8.0\bin 到 Path 里面,确定并退出。
4. 安装 MXNet  
   1. 安装包  
到这个网址下载 [https://github.com/yajiedesign/mxnet/releases](https://github.com/yajiedesign/mxnet/releases) 最新的 MXNet 包，官网上没说清楚，其实 base 包两个都要下载。对应安装的版本下载好这三个包文件。  
![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnajob3r9sj30m00dvq43.jpg)  
分别解压一下。在想要安装 MXNet 的盘创建一个新的目录，比如 D:MXNet 。将两个 prebuildbase 的包里的文件都复制粘贴到这个目录。这里会发现有3个名称相同的重复文件，前两个无关紧要而且一模一样，所以留下哪个都无所谓，第三个保留日期较新的那个。  
![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnajtlyr63j30ee0ebt98.jpg)  
再把剩下的那个包里的文件同样复制粘贴到 D:MXNet 。再将之前下载的 cuDNN 压缩包解压到 D:\MXNet\3rdparty\cudnn 。  

   2. 设置环境变量  
后面选择手动添加环境变量而不是运行该目录下的 Setupenv.cmd 文件，因为它会把 MXNET_HOME 变量设置成 D:\MXNet\ 多了/斜线,结果在设置 Path 路径时引用这个变量时会多加 /。  
添加环境变量MXNET_HOME，值为 D:\MXNet 。  
添加环境变量Path的值:  
%MXNET_HOME%\lib;  
%MXNET_HOME%\3rdparty\cudnn\bin;  
%MXNET_HOME%\3rdparty\cudart;  
%MXNET_HOME%\3rdparty\opencv;  
%MXNET_HOME%\3rdparty\vc;  
%MXNET_HOME%\3rdparty\gnuwin;  
%MXNET_HOME%\3rdparty\openblas\bin;    
   3. 运行安装
   在 CMD 下进入D:\MXNet\python，执行
        ```
        python setup.py install
        ```   
        出现以下界面说明安装成功。  
        ![](http://ww1.sinaimg.cn/mw690/006CYpBYly1fnal18ai2aj30gj0e6glt.jpg)
   4. 测试 MXNet  
    在 CMD 窗口打开 MXNet, 执行
        ```
        python
        impoort mxnet
        ```  
        结果出现如下错误。  
        ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnal4z4qszj30hs0afwen.jpg)  
        上网查了一下原因，是因为 MXNet 目录里的 Build 文件中的 libmxnet.dll 缺少依赖。参见这个[解释] (https://github.com/apache/incubator-mxnet/issues/2601)  WindowsError: [Error 126]  
        This is usually because the dependent dll not find. So, you can:  
        1.use the 'Depends' tools open the libmxnet.dll, see which dll is missing.  
        2.make sure the u have the dll in your machine, and add the environment-path.
        
        这里需要下载一个 Depends 工具， [Dependency Walker](http://www.dependencywalker.com/)，选择 x64 位的。  
        ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnalng2p99j30hi0ay3zz.jpg)  
        解压到桌面，运行 depends.exe ，选择 file -> open -> libmxnet 打开，稍等一些时间。就能看到有那些包缺失，就是带？号的，也就是下面的红字。我们主要看第一层目录下的包，发现 CUDNN64_7.DLL 缺失。  
        ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnam5s406qj30rx0e9q4a.jpg)  
        然后我们把 D:\MXNet\3rdparty\bin 目录里的 cudnn64_7.dll 复制到 D:\MXNet\3rdparty\cudnn\bin 目录。然后在重新尝试导入，这次没有错误了，不过又出现了一个 DeprecationWarning 警告。  
        ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnamcypr6hj30qg07pglo.jpg)  
        这个警告好像并不影响结果，但是还是让人不舒服，又上网搜了一下。具体看这里的解决方法 [https://discuss.gluon.ai/t/topic/1829](https://discuss.gluon.ai/t/topic/1829) 。一是更新下 conda 和 urllib ，但是我的这两个在 Anaconda 里都是最新的。二是试一下 `pip install -U pyopenssl `，但是不行，会出现否决错误。暂时搁置下来吧。。。

        下面用一个例子用一下 MXNet， 打开 CMD， 输入 `python`，然后输入下面代码，看运行结果是不是符合。
        ```python
        >>> import mxnet as mx
        >>> a = mx.nd.ones((2, 3))
        >>> b = a * 2 + 1
        >>> b.asnumpy()
        array([[ 3.,  3.,  3.],
               [ 3.,  3.,  3.]], dtype=float32)
        ```  
        ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnampo2wryj30gt08o74b.jpg)  
        好像还行，成功了，然后输入 `exit()` 退出。过程真的艰辛，而且对我这种稍微有点强迫症的人来说，有一个 error (cudaErrorLaunchTimeout) 和 warning (DeprecationWarning) 还未解决，真的难受，所以还是推荐用 Linux。
5. 安装 Tensorflow  
Tensorflow 的安装轻松很多。首先要确定一下的东西： 
   1. 确保 Python 版本是3.5且系统为64位（非常重要）。 
   2. 确保 pip 版本最新，可用 `python -m pip install -U pip` 在CMD中升级pip 。 
   3. 确保安装 Visual Studio 2015。 
   4. 确保已安装 cuda 和 cuDNN。
     
   首先打开 CMD, 执行 `activate py3`，进入 Python3 环境，可以使用 `python -V` 命令查看版本信息。输入 `pip -V` 查看 pip 版本，一般来说都是最新的。然后安装 Tensorflow。
    ```
    pip install --upgrade tensorflow-gpu
    ```
   ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnaocni4sjj30kw099dfy.jpg)  
   显示安装好了，然后试一下例子。打开 CMD， 输入 `python`，然后输入下面代码，看运行结果是不是符合。
   ```python
   >>>import tensorflow as tf
   >>>a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a')
   >>>b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')
   >>>c = tf.matmul(a, b)

   >>>sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))

   >>>print(sess.run(c))
   ```  
   出现了下面的结果则说明安装成功了，哦也。。  
   ![](http://ww1.sinaimg.cn/mw690/006CYpBYgy1fnaosmtx9tj30kw0d6dg6.jpg)  
      安装好就行了，如果还想装 Keras 框架，可以执行
   ```
    pip install keras
   ```
   如果还想配置其他 IDE 自己配就好了，比如 Pycharm 或者 Spyder 什么的。
6. 至此安装成功，很6。