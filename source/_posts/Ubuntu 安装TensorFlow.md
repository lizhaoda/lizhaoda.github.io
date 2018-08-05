---
title: Ubuntu 安装TensorFlow
url: 140.html
id: 140
categories:
  - Linux
date: 2016-10-09 17:28:12
tags:
---

1\.安装python pip 和 python-dev
```
sudo apt-get install python-pip python-dev
```
2\. 安装TensonFlow
```
sudo pip install --upgrade [https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.8.0-cp27-none-linux\_x86\_64.whl](https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.8.0-cp27-none-linux_x86_64.whl)
```
![](Ubuntu 安装TensorFlow/3f529ff67fc6d43eb8683cef7ed0b2f6_441a1c79-2788-4928-86ed-530c59dbe468.png)

如遇错误 多试几次 再不行 VPN翻墙

3\. 测试TensonFlow

终端输入 python

然后import tensonflow as tf

![](Ubuntu 安装TensorFlow/3f529ff67fc6d43eb8683cef7ed0b2f6_f633e60a-7233-4d06-8c32-da1d45fe9641.png)