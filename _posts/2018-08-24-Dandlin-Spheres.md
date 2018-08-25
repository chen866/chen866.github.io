---
layout: post
title: 丹迪林双球
tags:
  - 数学
  - 3Blue1Brown
lang: zh-Hans
---

这是一个有趣的三维几何问题，0基础包过！

<!--more-->

3Blue1Brown：深入浅出、直观明了地分享数学之美。  
中国官方账号[[传送门]](https://space.bilibili.com/88461692/#/)：

## 前置知识：

`椭圆`有多种定义方式：  

### 1. 单个方向上伸缩圆  
把圆的各个坐标设置为(x,y)，然后将所有的x坐标放大一个固定的倍数即可；

### 2. 双图钉画法  
将绳子绕在固定于纸上的两个图钉上，然后用铅笔拉紧绳子，让绳子保持紧绷，铅笔绕着画一圈，铅笔所画轨迹的每一个点，到两个图钉的距离之和都是一个定值，两个图钉的位置就是椭圆的焦点，也就是说**到两焦点距离和不变**的性质可以用来定义椭圆；

### 3. 斜切圆锥面  
斜切一个圆锥，切的角度小于圆锥的斜面角度，圆锥面和截面相交得到的曲线就是椭圆，这也是为什么椭圆是一种圆锥曲线。

### 离心率

椭圆的不同形状一般用`离心率`定量描述，有时可以理解为“扁平程度”。圆的离心率为0，椭圆越扁平，离心率越接近1。  
例如：地球绕太阳轨道的离心率是0.0167，接近圆；而哈雷彗星轨道的离心率有0.9671，扁平程度很高。 

在“到两焦点距离和不变的点的集合为椭圆”的定义下，离心率的大小取决于两个图钉相隔多远。具体公式为： 
> 两焦点的距离除以椭圆长轴的长度，即**离心率=焦距/长轴**

对于斜切圆锥的方法来说，离心率由所用截面的斜率确定。

## 证明以上三种定义等价

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-08-24-06.png)

对于第二种和第三种方法的等价，只需证明：  
> 在斜切的截面上存在着这样两个定点，使得曲线上的两个点到这两个定点的距离之和为定值

保罗·洛克哈特的《度量》（中文译名：《度量：一首献给数学的情歌》，英文名：Measurement）中有一个十分巧妙地证明方法：丹迪林双球。

1. 引入两个球体，分别放在斜截面的上方和下方，两个球体的大小恰好能使它们分别与圆锥面相切与两个圆处，而且分别与斜截面相切于不同的点。  
![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-08-24-01.png)  
这两个球体在曲线内部引入了两个特殊的点，即两球面和斜截面相切的点。这两个点并不只是和两球相交而已，还和圆锥面相切。  
![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-08-24-02.png)

2. 在曲线上随意选取一点，沿着圆锥面，从上切圆向下画一条线到下切圆，对于曲线上的每一点都存在一条线和上切圆和下切圆相连，且所有线从上切圆到此点的距离和此点到下切圆的距离之和长度相同，这个长度就是上切圆到下切圆的直线距离。  
![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-08-24-03.png)  
![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-08-24-04.png)  
从此点到上切圆的线段、此点到斜截面上与上球体相切的点的线段，都是上球体的切线，这两条线段交会于球外的一点，由此可得这两条线段相等，椭圆上所有的点到上切圆的距离等于该点到上球体和斜截面相切的点。

3. 同理，椭圆上所有的点到下切圆的距离等于该点到下球体和斜截面相切的点。  
所以，斜切面上和两个球体相交的点和曲线上的任意一点连接起来，该点到下球体和斜截面相切的点的距离和该点到上球体和斜截面相切的点的距离的和始终是恒定的，即为上切圆到下切圆在锥面上的距离。  
![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-08-24-05.png)  

4. 这两个点即为这个椭圆的两个焦点，因为它们满足`到两焦点距离和不变`的性质。  
这个证明由丹迪林（Dandelin）于1822年提出，所以这两个球又被称为`丹迪林双球（Dandelin spheres）`  

5. 使用同样的方法可以证明斜切一个圆柱也会得到椭圆。  

6. 由斜切圆柱可以证明拉伸圆形的椭圆定义和之前的两个定义等价。*（！！！）*  

## 从何而来  

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-08-24-07.png)

对于如何引入两个球体的解释：

> How do people come up with such ingenious arguments?  
It's the same way people come up with Madame bovary or Mona Lisa.   
I have no idea how it happens.   
I only know that when it happens to me, I feel very dortunate.
>> 人们是怎样想出如此巧妙的证明的呢？  
这与《包法利夫人》和蒙娜丽莎等作品的诞生是一样的吧。  
我不知道这样的构思是如何出现的，  
我只知道当我有这种想法时，那就是上天的眷顾。  

> --保罗·洛克哈特《度量》

这并不是运气使然的奇迹，而要看作丰富阅历经验的结晶。