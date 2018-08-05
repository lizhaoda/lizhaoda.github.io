---
title: TX1 Caffe安装与配置
url: 571.html
id: 571
categories:
  - 技术
date: 2017-02-21 11:47:19
tags:
---

（本文大部分为别人整理的摘录，其中加入少许个人安装时候的补充）  

由于TX1是ARM[架构](http://lib.csdn.net/base/architecture "大型网站架构知识库")，其编译与下载与PC上的ubuntu系统有些许不同。以下进行归纳整理：

## 环境配置 
### 编译环境配置 
安装必要的caffe环境
```
sudo add-apt-repository universe  
sudo apt-get update  
sudo apt-get install cmake git aptitude screen g++ libboost-all-dev \  
libgflags-dev libgoogle-glog-dev protobuf-compiler libprotobuf-dev \  
bc libblas-dev libatlas-dev libhdf5-dev libleveldb-dev liblmdb-dev \  
libsnappy-dev libatlas-base-dev python-numpy libgflags-dev \  
libgoogle-glog-dev python-skimage python-protobuf python-pandas \  
libopencv-dev  
```
在apt安装时，上面有个openCV库通常在刷板子时就自动装进去了（OpenCV4Tegra），所以可以不用管  

### Caffe下载
```
git clone https://github.com/BVLC/caffe.git  
```
## 编译caffe 
所有环境都安装好之后，我们可以caffe的编译了。 

首先需要对caffe的配置文件进行修改。使用cuDNN。
```
cd caffe  
mv Makefile.config.sample Makefile.config  
gedit Makefile.config  
第5行：去掉#号，即 “USE_CUDNN := 1”  
保存退出  
```
修改配置文件，或使用
```
sed -i 's/# USE_CUDNN/USE_CUDNN/g' Makefile.config
```
另外要注意的是，查看自己cuda版本，需要继续修改Makefile.config使兼容
```
\# CUDA architecture setting: going with all of them.
\# For CUDA < 6.0, comment the *_50 through *_61 lines for compatibility.
# For CUDA < 8.0, comment the *_60 and *_61 lines for compatibility.
CUDA_ARCH := -gencode arch=compute_20,code=sm_20 \
                -gencode arch=compute_20,code=sm_21 \
                -gencode arch=compute_30,code=sm_30 \
                -gencode arch=compute_35,code=sm_35 \
                -gencode arch=compute_50,code=sm_50 \
                -gencode arch=compute_52,code=sm_52
#               -gencode arch=compute_60,code=sm_60 \
#               -gencode arch=compute_61,code=sm_61 \
#               -gencode arch=compute_61,code=compute_61
```
TX1装的cuda版本为7.0，按照文件中的提示，注释相应行。

另外，TX1对这个选项设置还需要作如下改动：

```
   CUDA_ARCH := -gencode arch=compute_20,code=sm_20 \
   -gencode arch=compute_20,code=sm_21 \
   -gencode arch=compute_30,code=sm_30 \
   -gencode arch=compute_35,code=sm_35 \
   -gencode arch=compute_50,code=sm_50 \
   -gencode arch=compute_52,code=sm_52 
```

添加  `-gencode arch=compute_53,code=sm_53` 一行

### 参考：

https://devtalk.nvidia.com/default/topic/991260/jetson-tx1/caffe-runtest-error-check-failed-error-cudasuccess-8-vs-0-invalid-device-function/


### 编译caffe
```
make -j 3 all
```

关于使用make all -j 3 源于以下一段摘录  
```
now compile caffe, remember Do NOT use all the cores by make all -j, or it hangs the system. Instead use the following

make -j 3 all

# just to make sure everything is fine, leave this if you are in a rush
make -j 3 runtest
```
### 参考：
http://qinhongwei.com/2016/05/08/how-to-install-caffe-on-NVIDIA-TX1/

## 测试MNIST 

我们使用caffe自带的mnist例子来测试caffe是否编译成功。
```
$ bash ./date/mnist/get_mnist.sh  
$ bash ./examples/mnist/create_mnist.sh  
$ bash ./examples/mnist/train_lenet.sh   
``` 

如果caffe编译成功，则上面的程序不会报错。 

## 使用g++编译caffe中的classification.cpp文件 

使用g++指令编译classification.cpp文件，需要注意一些链接库的位置。
```
$ g++ classification.cpp `pkg-config –libs –cflags opencv` -I /home/ubuntu/caffe/include -I /home/ubuntu/caffe/build/src -I /usr/local/cuda/include/ -L /home/ubuntu/caffe/build/lib/ -lcaffe -lprotobuf -lhdf5 -lhdf5_cpp -lhdf5_hl -lhdf5_hl_cpp   -lleveldb -llmdb  -lgflags -lglog -lboost_system -lboost_thread  
```
编译成功之后，运行生成的a.out文件测试效果。
```
$ sudo ./a.out deploy.prototxt vgg\_iter\_3600.caffemodel mean.binaryproto label.txt 1.jpg  
```
注意自己文件的路径，不能照抄  