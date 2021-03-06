---
layout: article
title: 日常重装电脑系统
tags:
  - 电脑
lang: zh-Hans
---

日常重装电脑系统 以及 移动用户主文件夹到其他位置

<!--more-->

昨天看到[俄罗斯大神系统吧](https://tieba.baidu.com/f?kw=%B6%ED%C2%DE%CB%B9%B4%F3%C9%F1%CF%B5%CD%B3&fr=ala0&tpl=5)1月2号更新系统了，实在是受不了VS的占地面积，下午没事的时候就下载下来准备更新（重装）一下系统。

## 装系统

1. 先下载了**ISO系统镜像**2.3G左右，在此期间确认自己的电脑的启动方式和硬盘分区格式，可选择性备份一下**环境变量**，**user文件夹**，**驱动**，以及**U盘里的文件**  
> 我下载的系统镜像是从前文的贴吧链接里面下载的种子，使用迅雷或者百度云打开种子下载64位的ISO系统镜像文件  
启动方式和硬盘分区格式对应为：UEFI引导模式对应GPT硬盘分区格式、Legacy引导模式对应MBR硬盘分区格式，具体自行->[系统引导模式-百度](https://www.baidu.com/s?wd=%E7%B3%BB%E7%BB%9F%E5%BC%95%E5%AF%BC%E6%A8%A1%E5%BC%8F)，可以通过**DiskGenius**、傲梅分区等分区软件进行无损转换

2. 把ISO系统镜像用**软碟通**（UltraISO.exe）打开，插入U盘，然后选择“启动”->“写入硬盘映像”，在弹出的窗口中先点击“格式化”格式化U盘，格式化完成后点击“写入”，等待软件将系统镜像刻录到**U盘**上，写入后软件自动为U盘添加启动项  
![](/assets/images/2018-01-13-09.png)
![](/assets/images/2018-01-13-08.png)

3. 重启电脑，进入bios，确认电脑的启动方式为**UEFI启动**(如果支持的话)，然后退出，从U盘启动进入系统安装引导

4. 在引导过程中会有选择系统安装分区位置的选择，安装位置要选择自定义，把相应磁盘的C盘分区删除，使其未分配区域即为新系统的C盘大小，选择未分配区域点击继续安装系统，安装会把原有的C盘分区删除  
> 如果在这个步骤或者下面的步骤中出现硬盘分区格式和启动方式不对应需要转换的，而且在这个时候没有PE可以进入图形化界面进行操作的，可以使用命令行进行转换，不过这种方式会**格式化掉整个硬盘**，具体步骤为：  
在进入系统安装引导后按住**Shift**+**F10**，调出命令行窗口  
输入**diskpart**，点击回车  
输入**list disk**，点击回车，列出所有磁盘。磁盘是从0开始排序的，一般计算机硬盘为磁盘0，U盘或者其他启动盘为磁盘1，一般可根据硬盘大小来确定硬盘  
输入**select disk 0**（0即是上面查看到的磁盘编号），点击回车，即选择你要进行修改的磁盘  
输入**clean**，点击回车，会清除物理磁盘所有信息，即格式化掉整个硬盘  
输入**convert gpt**/(**convert gpt**)，点击回车，转换将硬盘分区格式转换为gpt（或者是mbr模式）  
再进入选择分区界面就可以了

## 设置

### 激活系统  

进入系统后，先激活系统，盘符不对的去磁盘管理里面把盘符改正，在**个性化->桌面图标**里面把**此电脑**打勾

### 安装驱动和软件

安装运行库、驱动，配置环境变量，安装像**QQ**、**chrome**、**Office**等等的“非绿色版应用”

## 移动User文件夹

平时的时候基本到这里就结束了，前些天想要转移**USERS**文件夹，在多次装系统中重复利用User文件夹并且解放C盘，然而失败了，我又默默打开了百度。。。

看了好几篇文章，我发现上次主要是由于文件被占用的问题，有两篇文章一个采用**Administrator**用户解决，一个采用恢复模式户解决  
> [恢复模式Win10移动User文件夹到其他位置](http://blog.csdn.net/CrowNAir/article/details/78533051)

这篇文章用了三个命令移动用户主文件夹  
> **xcopy** 复制文件夹
> **ren** 重命名文件夹以防不测
> **mklink** 建立映射

### 1. 切换用户

启用**Administrator**账户，注销当前用户，切换至**Administrator**登陆

### 2. 复制文件夹

![](/assets/images/2018-01-13-01.png)  
![](/assets/images/2018-01-13-02.png)

### 3. 重命名原用户主文件夹

![](/assets/images/2018-01-13-03.png)

### 4. 建立映射

![](/assets/images/2018-01-13-04.png)

### 5. 修改权限

![](/assets/images/2018-01-13-05.png)

### 6. 禁用**Administrator**帐户

![](/assets/images/2018-01-13-06.png)

### 7. 删除旧用户主文件夹

重启了下发现没问题就把重命名后的文件夹删除了
![](/assets/images/2018-01-13-07.png)

## mount盘符到目录

这个也挺有意思的  
> [Windows系统如何mount盘符到目录](https://blog.csdn.net/dog250/article/details/100140882?utm_source=app)
