---
title: ubuntu实体机安装及其基础知识
url: 289.html
id: 289
categories:
  - Linux
date: 2016-10-29 04:03:46
tags:
---

## 前言：
  实体机相对于虚拟机而言有自己独特的优势，例如性能更好之类的，于是我们就又写了一篇关于实体机的安装经验。并在文章的结尾加上了原理篇，可能由于笔者水平，写的并不是太好懂，但是这是笔者从许多论坛、电脑杂志、书籍总结出来的经验。遇到安装问题，可以参考着原理去找原因。如果问题还没有解决，欢迎留言。
准备工具：
- ubuntu系统
- 大于4G可被格式化的U盘一只
- 电脑
- rufus烧录软件。官网地址：http://rufus.akeo.ie/

### 第一步：烧录ubuntu系统。
插上U盘，打开rufus软件，如下图所示，设备选择自己的U盘,。为了保证可以从uefi启动，这里的文件系统选择FAT或者FAT32，其他的保持默认。

![](ubuntu实体机安装及其基础知识/ec2c9be3-4a49-4ff0-aafe-3276aeb018b6.png)

选择镜像，点击下图的光碟形状的按钮，选择你的Ubuntu镜像：

![](ubuntu实体机安装及其基础知识/317e7abed3264d7b484f007a4f3f28c6.png)

![](ubuntu实体机安装及其基础知识/7a9ab9c2-0c6c-4249-a353-3a583509b1ac.png)

然后点击开始按钮，弹出下面界面，选择默认，点击OK

![](ubuntu实体机安装及其基础知识/4f442f1e-8dd4-4487-a5ed-c57b294f4aef.png)

弹出以下界面，点击确定，即可开始烧录：

![](ubuntu实体机安装及其基础知识/a36dad9c-7558-4f6f-adc7-356f73a6cc24.png)

等待，此间不要拔掉U盘，待显示下图，即可弹出U盘，烧录完成。

![](ubuntu实体机安装及其基础知识/3bbf6e53-b072-4d36-91b1-3aa9d949b796.png)

### 第二步：分出一部分磁盘空间用于安装ubuntu系统：
右击我的电脑->管理->存储->磁盘管理(本地),见下图：

![](ubuntu实体机安装及其基础知识/0f18b369-7528-4a67-83c3-dc383d4a3363.png)

右击你要切割的分区，不能是C盘，选择这个操作是无损的：

![](ubuntu实体机安装及其基础知识/beff7ed6c1dbebfe7bfb5b8526ff323c.png)

如下图：输入你要分的大小，这里建议给ubuntu 10G以上的空间，笔者的这个分区可用空间过小，这里用1G举例，然后点击压缩：

![](ubuntu实体机安装及其基础知识/ff5df62b-e897-4c42-9bce-db4e50e7b14c.jpg)

切割完以后如下图，后面的那个未分配的1G空间就是我们要装ubuntu的地方：

![](ubuntu实体机安装及其基础知识/29199731-2944-4074-8f16-8cdea9814a0b.png)

### 第三步：进入BIOS：
如果你现在windows系统，请重启，因为如果windows开了快速启动(所谓快速启动就是windows将主要进程打压到硬盘中，下次开机直接从硬盘中读取文件，避免了开机速度慢的现象，现在的windows8及其以上的系统开机很快也是这个原理)，那么关机再开机会直接进入操作系统，不会给你进入BIOS的机会。点击重启后，趁加载windows之前立即反复按下进入BIOS的快捷键。请根据你的电脑品牌选择快捷键，见下图(有部分电脑如果无法进入，则同时按下Fn+下面快捷健)：

![](ubuntu实体机安装及其基础知识/0415559e206a7e2d6addb0e1030adda4.png)

### 第四步：更改BIOS设置：
因为笔者的电脑是神舟的，所以在此以神舟的BIOS设置举例，在这里，我们通常需要设置以下几项，：

boot引导顺序，找到boot选项卡，找到你的U盘，然后设为第一引导。

如在uefi模式下启动，请参考下图将uefi boot 改成enable。如果在legacy模式启动，请改成disabled（如何查看自己电脑模式，请翻看下文原理篇解释）

如果有Secure boot，请更改secure boot 为 disabled 。这个是微软为了保护主板强行让厂商加上的一个主板锁，当这个选项为enabled的时候，主板有可能会拒绝除了windows之外所有的系统的安装。所以这里将他关闭。

神舟BIOS详解：

![](ubuntu实体机安装及其基础知识/bb4c8cb1a82543c49374a82fef7f7e1f.jpg)

![](ubuntu实体机安装及其基础知识/88f76c9dffce0fc4ed32ae89869b0e47.jpg)

此外，常见的选项还有
- Reset to Setup Mode恢复bios出厂设置，
- Restore Factory Keys用于清除或恢复内置的安全启动密钥，
- OS Optimized Defaults是一个“顶级”设置选项，开启该选项后，BIOS会自动将所有相关选项恢复为预装Win8/8.1 默认启动方式所要求的标准设置。         

设置以上完毕后，进入Exit选项卡，选择保存这是并重启，即可进入ubuntu安装界面。

### 第五步：开始安装(因为实体机不容易截图，这里用虚拟机模拟实体机安装):
点击install  ，进入如下界面，选择语言为English(如果你不想装13..还是选中文ba...)，点击install Ubuntu：

![](ubuntu实体机安装及其基础知识/a8d4551f7ecdddf62a2dbe3a34997ea9.png)

这里什么都不勾选(请根据自己需要修改)，点击Continue

![](ubuntu实体机安装及其基础知识/e584ac285859082299f41d0538eb4f9c.png)
 
这里选择Something else

![](ubuntu实体机安装及其基础知识/d5a8a193184c4b8e8c97ff8364c675d0.png)
 
如下图所示：选中free space，即你第二步切出来的空闲空间，这里主要不要选错了，否则轻则windows崩溃，重则自己重要数据丢失

![](ubuntu实体机安装及其基础知识/5efed7b562dac30c81ef71a0d15a1b42.png)
 
点击下图中的“+“号，弹出下图，在其中的Size 框中输入大小，这里建议10G以上。其他的设置请和下图中的一致，然后点击ok

![](ubuntu实体机安装及其基础知识/040893fd9b4282a40525e84b1828a9f0.png)

重新选择剩下的free 空间，然后点击”+“号，请根据自己实际情况输入Size大小，然后选择Mount point 为 /home，然后点击ok                        

![](ubuntu实体机安装及其基础知识/64912e9eb25c578bd760ac6aa280bd5f.png)
                 
重新选择剩下的free space ，点击”+“ ，然后在弹出的界面的Size中按实际情况输入大小，这里建议电脑内存的两倍大小（其实并不需要....）， 选use as 为swap area，然后点击ok        

![](ubuntu实体机安装及其基础知识/8a9d7c15409816a0dcfe330065d1fb52.png)

最后的分区情况如图所示：(请按需分配自己每个分区的大小)。下图的Device for boot loader installation默认。然后点击install now
 
![](ubuntu实体机安装及其基础知识/9f8100f4a2fdde3eb4d92993451d1c0c.png)

弹出下图，点击 continue

![](ubuntu实体机安装及其基础知识/2509dc21cb2969862e9117fe71a61012.png)
下图选择 shanghai，点击Continue 

![](ubuntu实体机安装及其基础知识/5c468f6945c19010ad351a2210985b64.png)

这里保持默认，选择 Continue

![](ubuntu实体机安装及其基础知识/470a4a7e160f172d4b6aec14fbc56740.png)

如图，进行用户设定，计算机名 是主机名，用户名 是登录时用的账户名称，密码 则是你所设 用户名 的登录密码，请务必记牢。                  

![](ubuntu实体机安装及其基础知识/a99b1aab21b0ff54ce550ce380543536.png)

配置选择已完成，接下来请耐心等待安装过程，如图，请不要点击 SKIP

![](ubuntu实体机安装及其基础知识/ac3e3862412c8f0ea405f263836c822f.png)

耐心等待安装完成。点击Restart Now 

![](ubuntu实体机安装及其基础知识/646e9a799b8bf5d4f451736088f7a803.png)

然后会重启进入系统，用你上面配置的用户名和密码登录，请注意最好不要登录 root ，你可以用 sudo命令来获取相应的权限。

## 总结：
安装系统是一件比较简单的事情，但是比较浪费时间，遇到问题大家可以参考下面的原理篇。

## 和平回答
### Ask：什么是从cmos？
Answer：CMOS常指保存计算机基本启动信息（如日期、时间、启动设置等）的芯片。

### Ask：什么是bios？
Answer：BIOS是英文"Basic Input Output System"的缩略词，直译过来后中文名称就是"基本输入输出系统"。它保存着计算机最重要的基本输入输出的程序、开机后自检程序和系统自启动程序，它可从CMOS中读写系统设置的具体信息。 其主要功能是为计算机提供最底层的、最直接的硬件设置和控制。

### Ask：bios和cmos的关系：
Answer：CMOS是主板上的一块可读写的并行或串行FLASH芯片，是用来保存BIOS的硬件配置和用户对某些参数的设定。

### Ask：什么是uefi和legacy：
Answer：uefi是新式的BIOS，legacy是传统BIOS。

### Ask：怎么区分自己的电脑是legacy和uefi模式：
Answer：
  方法一：电脑刚买过来，预装win8、win8.1、win10的，没有拿去电脑店被别人重新装过系统的均是uefi启动，预装win7系统一般是legacy。被电脑店重新装过系统的一般都是legacy。
   方法二：见下图：右击我的电脑，点击管理，选择存储，选择磁盘管理(本地)，显示有红色框内的EFI分区的是uefi启动 (原理上fat32分区格式也可以引导uefi，充当EFI分区的功能，详细请看下面的讲解)，没有类似分区的启动方式均为legacy。

![](ubuntu实体机安装及其基础知识/ec69fbd1-5cf8-4711-acab-187c65333f5d.png)

### Ask：什么是MBR和GPT：
Answer：MBR和GPT均是磁盘的不同分区表格式(什么是分区表，请看我们推送的上一篇文章——Ubuntu 基础知识与虚拟机安装中分区表的介绍)。

### Ask：什么是分区：
Answer：是物理磁盘的一部分，其作用如同一个物理分隔单元。可以理解为：比如你买了一间很大的房子(磁盘)，必须用墙分成房子(分区)才能使用。当然你可以只有一个分区，但是不建议。

### Ask：什么是主分区和扩展分区：
Answer：
主分区：标记为由操作系统使用的一部分物理磁盘。
扩展分区：因为在MBR分区格式下只能有4个主分区，当超过4个分区的时候，我们就用扩展分区代替主分区进行管理。

### Ask：什么是活动分区和非活动分区：
Answer：活动分区是计算机系统分区，启动操作系统的文件都装在这个分区。

### Ask：什么是文件系统：
Answer：操作系统组织管理文件的一种方法，直白点说就是把硬盘上的数据以文件的形式呈现给用户。Fat32、NTFS都是常见的文件系统类型)的支持，它能够直接读取FAT分区中的文件。

### Ask：MBR和GPT 分区表格式有什么区别：
Answer：
#### MBR分区结构
我们可将MBR磁盘分区结构用下图简单表示（Windows下基本磁盘、4个主分区）：

![](ubuntu实体机安装及其基础知识/2b4a0b07c68cb0dddfbc43586697893e.png)

为了方便计算机访问硬盘，把硬盘上的空间划分成许许多多的区块（英文叫sectors，即扇区），然后给每个区块分配一个地址，称为逻辑块地址（即LBA）。在MBR磁盘的第一个扇区内保存着启动代码(446bytes)和硬盘分区表(64bytes)。 启动代码(446bytes) 的作用是指引计算机从活动分区引导启动操作系统（BIOS下启动操作系统的方式）；分区表的作用是记录硬盘的分区信息。在MBR中，分区表的大小(64bytes)是固定的，一共可容纳4个主分区信息,每一个都记录区段的开始与结束柱面号码。扩展分区（操作系统限制）最多有一个，扩展分区的目的是使用额外的扇区来记录分区信息 。

在MBR分区表中逻辑块地址采用32位二进制数表示，因此一共可表示2^32（2的32次方）个逻辑块地址。如果一个扇区大小为512字节，那么硬盘最大分区容量仅为2TB.

#### GPT分区结构

![](ubuntu实体机安装及其基础知识/6565e0efa75b9ee45f7759c702067500.png)

在GTP磁盘的第一个数据块中同样有一个与MBR（主引导记录）类似的标记，叫做PMBR。PMBR的作用是，当使用不支持GPT的分区工具时，整个硬盘将显示为一个受保护的分区，以防止分区表及硬盘数据遭到破坏。UEFI并不从PMBR中获取GPT磁盘的分区信息，它有自己的分区表，即GPT分区表。

GPT的分区方案之所以比MBR更先进，是因为在GPT分区表头中可自定义分区数量的最大值，也就是说GPT分区表的大小不是固定的。在Windows中，微软设定GPT磁盘最大分区数量为128个。另外，GPT分区方案中逻辑块地址（LBA）采用64位二进制数表示，可以计算一下2^64是一个多么庞大的数据，以我们的需求来讲完全有理由认为这个大小约等于无限，所以当硬盘大于2T大小的时候，我们必须采用GPT格式。除此之外，GPT分区方案在硬盘的末端还有一个备份分区表，保证了分区信息不容易丢失。

GPT分区最多有128个主分区，已经足够用了，所以在GPT分区格式下没有扩展分区这一说法。

因为legacy模式下无法识别GPT分区，所以legacy模式下下GPT分区的磁盘不能用于启动操作系统，在操作系统提供支持的情况下可用于数据存储。
UEFI可同时识别MBR分区和GPT分区，因此UEFI下，MBR磁盘和GPT磁盘都可用于启动操作系统和数据存储。

### Ask：uefi、legacy与MBR和GPT之间的关系
Answer：
从上面我们已经知道了uefi和legacy是bios的启动方式。MBR和GPT是磁盘的分区格式。
为了简单起见，我们通常将uefi+GPT绑定，即uefi启动方式下你的磁盘必须是GPT格式；legacy和MBR绑定，即当legacy启动方式下你的磁盘必须是MBR格式。实际上uefi+MBR也是可以的，只要含有FAT32格式的分区放引导即可。但是很少有人这样用，想折腾的人可以试试。

### Ask：legacy+MBR和uefi+GPT两种模式有什么区别？
Answer：
#### Legacy+MBR
1.原理：
磁盘起始位置以特定格式描述磁盘上的分区，并包含“启动装载程序 (boot loader)”，BIOS 固件知道如何执行这一小段启动装载程序代码。启动装载程序的职责是启动操作系统（现代启动装载程序的大小通常超出了 MBR 空间所能容纳的范围，因此必须采用多阶段设计，其中 MBR 部分只知道如何从其他位置加载下一阶段）。

2.引导顺序：
bios->mbr(内含引导加载程序)->引导加载程序boot loader(一个可读取内核文件来执行的软件)(作用1提供菜单、作用2载入内核、作用3将控制权转移给其它的loader)。安装系统的时候mbr和boot sector(每一个分区的第一个扇区，文件系统会保留)均放bootloader(所以先装linux后装windows，linux的引导会掉)。但是windows的bootloader没有转移控制权的功能。所以一般mbr放grub引导装载程序(功能1指向linux内核文件、功能2移交控制权给boot sector的windows bootloader)。

3.系统启动时顺序 ：
windows XP：BIOS -> MBR (主引导记录)-> DPT (硬盘分区表)-> PBR -> 寻找根目录下 NTLDR(XP) ->boot.ini(xp)
windows 7及以上：BIOS -> MBR -> DPT -> PBR-> 寻找根目录下bootmgr->\boot\bcd->winload.exe->内核
linux：BIOS -> MBR -> DPT -> PBR-> 寻找根目录下grldr(Grub)->主引导搜索指定位置的Grub.cfg->加载Grub.cfg菜单->linux内核
注意：当xp和win7共存，则bootmgr起作用。Xp显示为早期的windows版本

4.要求：
bios方式只要存在一个非隐藏的活动主分区就能引导系统（必须放在这，如果不放在活动分区，可以进windows PE用ntbootautofix修复，会自动从非活动分区的系统盘复制引导进入活动分区！）所以可以将系统装在任何一个分区。也可以将系统和引导放不同分区。这个主分区的位置不固定。然后安装系统成功后系统自动设置这个活动主分区为隐藏。

#### uefi+GPT
1.Windows操作系统对GPT磁盘的支持
32位Windows对GPT分区支持情况

![](ubuntu实体机安装及其基础知识/c60e60ff41fda02dc912a0c7044f873b.png)

64位Windows对GPT分区支持情况

![](ubuntu实体机安装及其基础知识/0fd49965b80ee18877fd596a13119691.png)

总结：凡是32位的操作系统都不能设置为uefi+GPT启动，另外windows7原生也是不支持uefi启动的，所以需要你添加支持uefi引导文件，感兴趣的可以自己查资料。

2.原理
和legacy启动不同的是，uefi启动的时候会寻找电脑中的ESP分区或者FAT32格式分区下的引导文件，即efi文件引导系统。

在ESP分区下（或者FAT32格式的分区，即使是FAT32分区，放完引导文件之后在windows系统下该分区还不可见），

“√”表示uefi引导系统必不可少的文件，其他可以删除。但是必须是下面的路径！！！，不区分大小写：
    EFI/Boot/bootx64.efi(或bootia32.efi)
√ EFI/Microsoft/Boot/bootmgfw.efi
√ EFI/Microsoft/Boot/BCD
   EFI/Microsoft/Boot/zh-CN

boot×64是系统引导，凌驾于win引导之上，可以读取开机项。

3.启动顺序：
windows系统
①一般情况下(系统引导和windows引导都管用)
\efi\boot\bootx64.efi->\efi\microsoft\boot\bootmgfw.efi->\efi\microsoft\boot\bcd->winload.efi
②系统引导(windows boot manager不管用了，系统引导管用)
\efi\boot\bootx64.efi->\efi\microsoft\boot\bcd->winload.efi
③windows引导(windows boot manager) 
\efi\microsoft\boot\bootmgfw.efi->\efi\microsoft\boot\bcd->winload.efi
这里加载\efi\microsoft\BCD 启动菜单文件是因为当前的efi文件的内容是微软写的，efi内容下一步就指向\efi\microsoft\BCD。我们当然可以创建一个abc.efi,然后改名，bootx64.efi 或者bootia32.efi，让UEFI开机的时候加载，这时候你可以让你自己写的abc.efi指向某个目录的某个CFG文件这都随你愿意，从而实现调用......

linux系统：
\efi\boot\bootx64.efi （grub2.efi改名）--->搜索指定位置的Grub.cfg--->加载Grub.cfg菜单，有用户自行选择启动项。
大家如需自行定制Grub2的话需具备 ubuntu 系统，并且需要BIOS和UEFI版本的各一个，然后使用 grub-mkimage 定制。

4.要求
UEFI+GPT的中ESP分区的位置也是可以随意设置的，在硬盘起始位置、中间位置、末尾，都可以，只要分区属性和其中的引导文件正确，就可以引导启动操作系统 
gpt一定要uefi才能引导系统。uefi不一定要gpt只要有一个fat分区即可。
不支持uefi的主板（系统支持uefi文件读取），使用大于2.2T的硬盘，可以用mbr（u盘，另一个硬盘）加gpt。前者放引导。此时系统可以装在gpt与mbr。
uefi加mbr下没有了活动分区，直接引导分区和系统分区。不能把引导放在ESP分区，要放fat里面，并且这个分区一定要是活动分区！！！

5.修复引导
bcdboot.exe 会修复系统引导，而且会同时修复计算机默认引导和Windows 默认引导，即在esp(fat)分区下没有任何文件下的情况下完成所有的文件复制。在ESP分区同时出现bootx64.efi和bootmgfw.efi，并且bootx64.efi是由bootmgfw.efi 改名而来的。与此同时在Boot Menu启动选择菜单那里生成“Windows Boot Manager”，Windows Boot Manager 及其包含的信息是保存在主板上的NVRAM里面的，而不是保存在硬盘上，故删除Windows Boot Manager需要到BIOS设置区删除。

6.UEFI及其优势
仅从系统启动原理方面来做比较。UEFI之所以比BIOS强大，是因为UEFI本身已经相当于一个微型操作系统，其带来的便利之处在于：
首先，UEFI已具备文件系统
其次，可开发出直接在UEFI下运行的应用程序，这类程序文件通常以efi结尾。既然UEFI可以直接识别FAT分区中的文件，又有可直接在其中运行的应用程序。那么完全可以将Windows安装程序做成efi类型应用程序，然后把它放到任意fat分区中直接运行即可，如此一来安装Windows操作系统这件过去看上去稍微有点复杂的事情突然就变非常简单了，就像在Windows下打开QQ一样简单。而事实上，也就是这么一回事。要知道，这些都是BIOS做不到的。因为BIOS下启动操作系统之前，必须从硬盘上指定扇区读取系统启动代码（包含在主引导记录中），然后从活动分区中引导启动操作系统。对扇区的操作远比不上对分区中文件的操作更直观更简单，所以在BIOS下引导安装Windows操作系统，我们不得不使用一些工具对设备进行配置以达到启动要求。而在UEFI下，这些统统都不需要，不再需要主引导记录，不再需要活动分区，不需要任何工具，只要复制安装文件到一个FAT32（主）分区/U盘中，然后从这个分区/U盘启动，安装Windows就是这么简单。

7.经验
当uefi方式启动时，如果系统引导丢了，目前出现了不能关机，但是能重启成功的现象。goto 5.