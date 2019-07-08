---
layout: article
title: 第2章 序列构成的数组(三)
tags:
  - python
  - 笔记
  - 《流畅的Python》
---

<!--more-->

### 2.9 当列表不是首选时
#### 数组
* 如果要存放 1000 万个浮点数的话，数组（array）的效率要高得多，因为数组在背后存的并不是 float 对象，而是数字的机器翻译，也就是字节表述。这一点就跟 C 语言中的数组一样。  
* 如果我们需要一个只包含数字的列表，那么 array.array 比 list 更高效。数组支持所有跟可变序列有关的操作，包括 .pop 、.insert 和.extend 。另外，数组还提供从文件读取和存入文件的更快的方法，如.frombytes 和 .tofile 。  
* 如果需要频繁对序列做先进先出的操作，deque（双端队列）的速度应该会更快。  
* 如果代码里包含操作的频率很高（比如检查一个元素是否出现在一个集合中），用 set（集合）会更合适。set 专为检查元素是否存在做过优化。但是它并不是序列，因为 set 是无序的。  

另外一个快速序列化数字类型的方法是使用 [pickle](https://docs.python.org/3/library/pickle.html) 模块。pickle.dump处理浮点数组的速度几乎跟 array.tofile 一样快。不过前者可以处理几乎所有的内置数字类型，包含复数、嵌套集合，甚至用户自定义的类。前提是这些类没有什么特别复杂的实现。

一个浮点型数组的创建、 存入文件和从文件读取的过程
```python
#引入 array 类型
from array import array
from random import random
#利用一个可迭代对象来建立一个双精度浮点数组（类型码是 'd'），这里我们用的可迭代对象是一个生成器表达式。
floats = array('d', (random() for i in range(10**7)))
#把数组存入一个二进制文件里
with open('floats.bin', 'wb') as fp:
    floats.tofile(fp)
#新建一个双精度浮点空数组
floats2 = array('d')
# 把 1000 万个浮点数从二进制文件里读取出来
with open('floats.bin', 'rb') as fp:
    floats2.fromfile(fp, 10**7)
#查看数组的最后一个元素
print(floats[-1])
#查看新数组的最后一个元素
print(floats2[-1])
#检查两个数组的内容是不是完全一样
print(floats2 == floats)

#0.07802343889111107
#0.07802343889111107
#True
```

#### 内存视图
memoryview 是一个内置类，它能让用户在不复制内容的情况下操作同一个数组的不同切片。

> Travis Oliphant 是 NumPy 的主要作者，他在回答“ When should a memoryview be used?”（http://stackoverflow.com/questions/4845418/when-should-amemoryview-be-used/）这个问题时是这样说的：  
>> 内存视图其实是泛化和去数学化的 NumPy 数组。它让你在不需要复制内容的前提下，在数据结构之间共享内存。其中数据结构可以是任何形式，比如 PIL 图片、SQLite 数据库和 NumPy 的数组，等等。这个功能在处理大型数据集合的时候非常重要。

通过改变数组中的一个字节来更新数组里某个元素的值
```python
#利用含有 5 个短整型有符号整数的数组（类型码是 'h'）创建一个memoryview 
numbers = array.array('h', [-2, -1, 0, 1, 2])
memv = memoryview(numbers)
print(len(memv))
#5
#memv 里的 5 个元素跟数组里的没有区别
print(memv[0])
#-2
#创建一个 memv_oct，这一次是把 memv 里的内容转换成 'B' 类型，也就是无符号字符
memv_oct = memv.cast('B')
#以列表的形式查看 memv_oct 的内容
print(memv_oct.tolist())
#[254, 255, 255, 255, 0, 0, 1, 0, 2, 0]
#把位于位置 5 的字节赋值成 4
memv_oct[5] = 4
#因为我们把占 2 个字节的整数的高位字节改成了 4，所以这个有符号整数的值就变成了 1024
print(numbers)
#array('h', [-2, -1, 1024, 1, 2])
```

#### NumPy和SciPy
凭借着 NumPy 和 SciPy 提供的高阶数组和矩阵操作，Python 成为科学计算应用的主流语言。

NumPy 实现了多维同质数组（homogeneousarray）和矩阵，这些数据结构不但能处理数字，还能存放其他由用户定义的记录。通过 NumPy，用户能对这些数据结构里的元素进行高效的操作。

SciPy 是基于 NumPy 的另一个库，它提供了很多跟科学计算有关的算法，专为线性代数、数值积分和统计学而设计。

> SciPy 的高效和可靠性归功于其背后的 C 和 Fortran 代码，而这些跟计算有关的部分都源自于Netlib 库（http://www.netlib.org）。换句话说，SciPy 把基于 C 和Fortran 的工业级数学计算功能用交互式且高度抽象的 Python 包装起来，让科学家如鱼得水。

对 numpy.ndarray 的行和列进行基本操作
```python
import numpy
#新建一个 0~11 的整数的 numpy.ndarry，然后把它打印出来 
a = numpy.arange(12)
print(a)
#array([ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11])
print(type(a))
#<class 'numpy.ndarray'>
#看看数组的维度，它是一个一维的、有 12 个元素的数组
print(a.shape)
#(12,)
#把数组变成二维的，然后把它打印出来看看
a.shape = 3, 4
print(a)
# array([[ 0, 1, 2, 3],
#     [ 4, 5, 6, 7],
#     [ 8, 9, 10, 11]])
print(a[2])
#array([ 8, 9, 10, 11])
#打印第 2 行第 1 列的元素
print(a[2, 1])
#9
#把第 1 列打印出来
print(a[:, 1])
#array([1, 5, 9])
#把行和列交换，就得到了一个新数组
print(a.transpose())
# array([[ 0, 4, 8],
#     [ 1, 5, 9],
#     [ 2, 6, 10],
#     [ 3, 7, 11]])
```

NumPy 也可以对 numpy.ndarray 中的元素进行抽象的读取、保存和其他操作
```python
import numpy
#从文本文件里读取 1000 万个浮点数
floats = numpy.loadtxt('floats-10M-lines.txt')
#利用序列切片来读取其中的最后 3 个数
floats[-3:]
#array([ 3016362.69195522, 535281.10514262, 4566560.44373946])
#把数组里的每个数都乘以 0.5，然后再看看最后 3 个数
floats *= .5
floats[-3:]
#array([ 1508181.34597761, 267640.55257131, 2283280.22186973])
#导入精度和性能都比较高的计时器（Python 3.3 及更新的版本中都有这个库）
from time import perf_counter as pc
#把每个元素都除以 3，可以看到处理 1000 万个浮点数所需的时间还不足 40 毫秒
#python中的分号是可加、可不加的
t0 = pc(); floats /= 3; pc() - t0
#0.03690556302899495
#把数组存入后缀为 .npy 的二进制文件
numpy.save('floats-10M', floats)
#将上面的数据导入到另外一个数组里，这次 load 方法利用了一种叫作内存映射的机制，它让我们在内存不足的情况下仍然可以对数组做切片
floats2 = numpy.load('floats-10M.npy', 'r+')
#把数组里每个数乘以 6 之后，再检视一下数组的最后 3 个数
floats2 *= 6
print(floats2[-3:])
#memmap([3016362.69195522, 535281.10514262, 4566560.44373946])
```

#### 双向队列
> 利用 .append 和 .pop 方法，我们可以把列表当作栈或者队列来用（比如，把 .append 和 .pop(0) 合起来用，就能模拟栈的“先进先出”的特点）。但是删除列表的第一个元素（抑或是在第一个元素之前添加一个元素）之类的操作是很耗时的，因为这些操作会牵扯到移动列表里的所有元素。

collections.deque 类（双向队列）是一个线程安全、可以快速从两端添加或者删除元素的数据类型。而且如果想要有一种数据类型来存放“最近用到的几个元素”，deque 也是一个很好的选择。这是因为在新建一个双向队列的时候，你可以指定这个队列的大小，如果这个队列满员了，还可以从反向端删除过期的元素，然后在尾端添加新的元素。

使用双向队列
```python
from collections import deque
#maxlen 是一个可选参数，代表这个队列可以容纳的元素的数量，而且一旦设定，这个属性就不能修改了
dq = deque(range(10), maxlen=10)
print(dq)
#deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], maxlen=10)
#队列的旋转操作接受一个参数 n，当 n > 0 时，队列的最右边的 n个元素会被移动到队列的左边。当 n < 0 时，最左边的 n 个元素会被移动到右边
dq.rotate(3)
print(dq)
#deque([7, 8, 9, 0, 1, 2, 3, 4, 5, 6], maxlen=10)
dq.rotate(-4)
print(dq)
#deque([1, 2, 3, 4, 5, 6, 7, 8, 9, 0], maxlen=10)
#当试图对一个已满（len(d) == d.maxlen）的队列做尾部添加操作的时候，它头部的元素会被删除掉
dq.appendleft(-1)
print(dq)
#deque([-1, 1, 2, 3, 4, 5, 6, 7, 8, 9], maxlen=10)
#在尾部添加 3 个元素的操作会挤掉 -1 、1 和 2
dq.extend([11, 22, 33])
print(dq)
#deque([3, 4, 5, 6, 7, 8, 9, 11, 22, 33], maxlen=10)
#extendleft(iter) 方法会把迭代器里的元素逐个添加到双向队列的左边，因此迭代器里的元素会逆序出现在队列里
dq.extendleft([10, 20, 30, 40])
print(dq)
#deque([40, 30, 20, 10, 3, 4, 5, 6, 7, 8], maxlen=10)
```

> 双向队列实现了大部分列表所拥有的方法，也有一些额外的符合自身设计的方法，比如说 popleft 和 rotate 。但是为了实现这些方法，双向队列也付出了一些代价，从队列中间删除元素的操作会慢一些，因为它只对在头尾的操作进行了优化。append 和 popleft 都是原子操作，也就说是 deque 可以在多线程程序中安全地当作先进先出的栈使用，而使用者不需要担心资源锁的问题。

#### 其他的 Python 标准库对队列的实现
* **queue**  
提供了同步（线程安全）类 Queue 、LifoQueue 和
PriorityQueue，不同的线程可以利用这些数据类型来交换信息。这
三个类的构造方法都有一个可选参数 maxsize，它接收正整数作为输
入值，用来限定队列的大小。但是在满员的时候，这些类不会扔掉旧的
元素来腾出位置。相反，如果队列满了，它就会被锁住，直到另外的线
程移除了某个元素而腾出了位置。这一特性让这些类很适合用来控制活
跃线程的数量。
* **multiprocessing**  
这个包实现了自己的 Queue，它跟 queue.Queue 类似，是设计给
进程间通信用的。同时还有一个专门的
multiprocessing.JoinableQueue 类型，可以让任务管理变得更方
便。
* **asyncio**  
Python 3.4 新提供的包，里面有 Queue 、LifoQueue
、PriorityQueue 和 JoinableQueue，这些类受到 queue 和
multiprocessing 模块的影响，但是为异步编程里的任务管理提供了专门的便利。
* **heapq**  
heapq 没有队列类，而是提供了
heappush 和 heappop 方法用户可以把可变序列当作堆队列或者优
先队列来使用。

#### 第二章小结
Python 序列类型最常见的分类就是可变和不可变序列。但另外一种分类方式就是把它们分为扁平序列和容器序列。  
* 扁平序列的体积更小、速度更快而且用起来更简单，但是它只能保存一些原子性的数据，比如数字、字符和字节。  
* 容器序列则比较灵活，但是当容器序列遇到可变对象时，特别是带嵌套的数据结构出现时，用户要多费一些心思来保证代码的正确。 

列表推导和生成器表达式则提供了灵活构建和初始化序列的方式。 

元组在 Python 里既可以用作无名称的字段的记录，又可以看作不可变的列表。  
当元组被当作记录来用的时候，拆包是最安全可靠地从元组里提取不同字段信息的方式。新引入的 * 句法让元组拆包的便利性更上一层楼，让用户可以选择性忽略不需要的字段。具名元组也已经不是一个新概念了，但它似乎没有受到应有的重视。就像普通元组一样，具名元组的实例也很节省空间，但它同时提供了方便地通过名字来获取元组各个字段信息的方式，另外还有个实用的._asdict() 方法来把记录变成 OrderedDict 类型。Python 里最受欢迎的一个语言特性就是序列切片，而且很多人其实还没完全了解它的强大之处。比如，用户自定义的序列类型也可以选择支持NumPy 中的多维切片和省略（...）。另外，对切片赋值是一个修改可变序列的捷径。 

重复拼接 seq * n 在正确使用的前提下，能方便地初始化含有不可变元素的多维列表。增量赋值 += 和 *= 会区别对待可变和不可变序列。在遇到不可变序列时，这两个操作会在背后生成新的序列。但如果被赋值的对象是可变的，那么这个序列会就地修改——然而这也取决于序列本身对特殊方法的实现。 

序列的 sort 方法和内置的 sorted 函数虽然很灵活，但是用起来都不难。这两个方法都比较灵活，是因为它们都接受一个函数作为可选参数来指定排序算法如何比较大小，这个参数就是 key 参数。key 还可以被用在 min 和 max 函数里。如果在插入新元素的同时还想保持有序序列的顺序，那么需要用到 bisect.insort 。bisect.bisect 的作用则是快速查找。 

除了列表和元组，Python 标准库里还有 array.array 。另外，虽然NumPy 和 SciPy 都不是 Python 标准库的一部分，但稍微学习一下它们，会让你在处理大规模数值型数据时如有神助。 

末尾介绍了 collections.deque 这个类型，它具有灵活多用和线程安全的特性。最后也提及了一些标准库中的其他队列类型的实现。