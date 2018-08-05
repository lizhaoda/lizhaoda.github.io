---
title: Python起步(包管理与编辑器)
url: 585.html
id: 585
categories:
  - 技术
date: 2017-02-24 23:56:20
tags:
---
通常Python入门，很多情况都是直接在IDLE上敲代码，但是这个IDLE又没有补全，又没有赏心悦目的界面，这就很郁闷了….

于是，对于编辑器，这就好好选择了。
## 安装Anaconda
这是Python的一个包管理器，通过它，你可以十分轻松地对Python的包进行管理（安装与卸载）。（这包就是Python的生命呀，还不好好地弄好）

因为Python有2和3，相互又有点不兼容，所以Anaconda也有对应Python2和Python3的版本。

![](Python起步(包管理与编辑器)/52c7f031-ad51-4e99-bbf4-0e91c71d6979.png)

https://www.continuum.io/downloads/

在装完Anaconda时，同时它会自动帮你装上Python，所以之前如果有单独装过Python的，就可以卸掉了。

## 安装PyCharm
PyCharm是一个十分优秀的Python编辑器，虽然它的包比较大，运行的时候较为占用资源。但是它在编码以及调试的时候能提高不少效率。因为PyCharm只是编辑器，所以也就没有版本之分了。

![](Python起步(包管理与编辑器)/e4065ecc-1104-48fc-8ea4-5d17dbc90f1f.png)

http://www.jetbrains.com/pycharm/download/#section=windows
Community版免费，已经足够使用了（良心企业）。

一开始使用PyCharm可能挺难上手，但是用惯了以后则会慢慢享受起来了~

## 使用Anaconda安装Python的包（模块）
在windows的搜索中（开始键+s，还有各种搜索方法），可以找到这个

![](Python起步(包管理与编辑器)/55b23885-a46c-4710-891e-73b338049d37.png)

点开以后，会有如下界面（有些高级点的Anaconda Navigator 有交互界面,,,那个另说啦）

![](Python起步(包管理与编辑器)/d6484892-08ea-452e-8320-6335c299675d.png)

以最常见的numpy包为例，只要输入
```
conda install numpy
```
或者
```
pip install numpy
```

就能安装（推荐使用pip安装，还有忘了pip要不要事先安装了- -）

因为很多时候安装的包从国外下载，就会特别慢，所以换成国内的镜像（国外资源在国内的复制品的感觉吧），速度就嗖嗖嗖的

于是在命令改为
```
pip install numpy -i http://pypi.douban.com/simple
-i 后面的参数是更换的镜像源的地址，使用豆瓣的源
```
至此，如无意外，就能好好装包了，import ~~~

## 使用PyCharm
### 建立工程

![](Python起步(包管理与编辑器)/6e6628bf-2596-4996-967d-eeae3313cfaa.png)

Location：工程放到哪里
Interpreter：解析器，选择Anaconda的，不然刚装的包没有用

## 新建Python文件

![](Python起步(包管理与编辑器)/0547e188-e203-474c-9f7f-5bf544f3aa21.png)

对左方文件夹右键，如图 然后键入文件名就好
 
### 敲代码

这不用说了吧？

### 运行代码

敲完代码以后，直接在代码框上右键找到run

![](Python起步(包管理与编辑器)/d24c1ba6-4f06-457e-a9ea-fe678813111f.png)

或者右上角有个三角形的进行运行，不过甚是麻烦那个，就不说了。
 
运行结果在下面的框框中。

![](Python起步(包管理与编辑器)/fd7f66e9-40f3-40f8-8334-590ac1782246.png)

### 别的东西

下方栏中的几个工具：

![](Python起步(包管理与编辑器)/014da6eb-5f53-4937-aefa-b3f75fc893be.png)

跟原生IDLE差不多，具有交互功能（另外有个IPython的，具有补全功能的交互式IDLE）

然后这个

![](Python起步(包管理与编辑器)/aa9e98a4-571e-4d24-8080-aeec821983d1.png)

终端，，没什么好解释的，就是系统的那个黑色框框

如果程序需要外部运行时传入参数，用这个可以帮到你。

然后敲代码的时候，多按照PyCharm提示，调整自己的代码风格，我觉得是挺好的。

## Anaconda Navigator
具有交互式界面的Anaconda包管理器

![](Python起步(包管理与编辑器)/fcc2bd20-5e84-41fe-b77a-4adf9abe0ef2.png)

在Home界面，

![](Python起步(包管理与编辑器)/ee5c371e-cdda-4693-97ae-45caaf5f60c9.png)

这个可以打开ipynb文件，一种可以改动的交互式笔记本，常用作教学使用（好吧，我编不下去了，自己百度吧，，，）

![](Python起步(包管理与编辑器)/d6b78aeb-733b-4165-a00b-235356fc6e46.png)

IPython，之前提过，Python原生交互式Shell的增强版，在平时试语法试函数等等常用，带自动补全。

![](Python起步(包管理与编辑器)/6111f39b-44a5-4bbf-889e-cdfcce02d338.png)

跟Matlab特别想像的Python编辑器，可以查看变量之类的。（爬虫挺适合的吧）
在Environments可以浏览一堆Python包，选择安装。

其余就不细说了~~