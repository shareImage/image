
# CPU性能和CACHE

为了让程序能快点，特意了解了CPU的各种原理，比如多核、超线程、NUMA、睿频、功耗、GPU、大小核再到分支预测、cache_line失效、加锁代价、IPC等各种指标（都有对应的代码和测试数据）都会在这系列文章中得到答案。当然一定会有程序员最关心的分支预测案例、Disruptor无锁案例、cache_line伪共享案例等等。

这次让我们从最底层的沙子开始用8篇文章来回答各种疑问以及大量的实验对比案例和测试数据。

大的方面主要是从这几个疑问来写这些文章：

-   同样程序为什么CPU跑到800%还不如CPU跑到200%快？
-   IPC背后的原理和和程序效率的关系？
-   为什么数据库领域都爱把NUMA关了，这对吗？
-   几个国产芯片的性能到底怎么样？

## 系列文章

[CPU的制造和概念](/2021/06/01/CPU%E7%9A%84%E5%88%B6%E9%80%A0%E5%92%8C%E6%A6%82%E5%BF%B5/)

[Perf IPC以及CPU性能](/2021/05/16/Perf IPC以及CPU利用率/)

[CPU 性能和Cache Line](/2021/05/16/CPU Cache Line 和性能/)

[十年后数据库还是不敢拥抱NUMA？](/2021/05/14/%E5%8D%81%E5%B9%B4%E5%90%8E%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%98%E6%98%AF%E4%B8%8D%E6%95%A2%E6%8B%A5%E6%8A%B1NUMA/)

[Intel PAUSE指令变化是如何影响自旋锁以及MySQL的性能的](/2019/12/16/Intel PAUSE指令变化是如何影响自旋锁以及MySQL的性能的/)

[Intel、海光、鲲鹏920、飞腾2500 CPU性能对比](/2021/06/18/%E5%87%A0%E6%AC%BECPU%E6%80%A7%E8%83%BD%E5%AF%B9%E6%AF%94/)

[一次海光物理机资源竞争压测的记录](/2021/03/07/%E4%B8%80%E6%AC%A1%E6%B5%B7%E5%85%89%E7%89%A9%E7%90%86%E6%9C%BA%E8%B5%84%E6%BA%90%E7%AB%9E%E4%BA%89%E5%8E%8B%E6%B5%8B%E7%9A%84%E8%AE%B0%E5%BD%95/)

[飞腾ARM芯片(FT2500)的性能测试](/2021/05/15/%E9%A3%9E%E8%85%BEARM%E8%8A%AF%E7%89%87-FT2500%E7%9A%84%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95/)

## CPU中为什么要L1/L2等各级cache

因为CPU的速度和访问内存速度差异太大，也就导致了所谓的 **内存墙**

cpu的速度大概50-60%每年的增长率，内存只有7%每年增长率：

![A 1000× Improvement of the Processor-Memory Gap | SpringerLink](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/476909_1_En_15_Fig3_HTML.png)

## CPU中的cache变迁历史

80486(1989), 8K的L1 cache第一次被集成在CPU中:

![486 motherboard with CPU location and 2nd level cache marked](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/42gg2.png)

**80686**(1995) ，[L2被放入到CPU的Package](https://superuser.com/questions/196143/where-exactly-l1-l2-and-l3-caches-located-in-computer)上，可以看到L2大小和一个Die差不多:

![Picture of a pentium Pro CPU, 256KB cache model](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/eAvLK.png)

以酷睿为例，现在的CPU集成了L1/L2/L3等各级CACHE，**CACHE面积能占到CPU的一半**:

![modernCPUwithL3.png](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/4Z1nU.png)

从上图可以看到L3的大小快到die的一半，L1/L2由每个core独享，L3是所有core共享，3级CACHE总面积跟所有core差不多大了。

## cache对CPU性能的影响

CPU访问内存是非常慢的，所以我们在CPU中增加了多级缓存来**匹配**CPU和内存的速度。主频这20年基本都没怎么做高了，但是工艺提升了两个数量级，工艺提升的能力主要给了cache，从而整体CPU性能提升了很多。

### 缓存对Oceanbase ，MySQL, ODPS的性能影响

这部分数据来自 @彦军

以下测试数据主要来源于真实的业务场景：OB/MySQL/ODPS

![img](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/bb29ac99-3645-4482-8473-c55b190af777.png)

x86 Skylake之前，L1 I/D 32KB, L2 256KB, L3 2.5MB/core， 2.5MB/core的L3（LLC）芯片面积相当于1/2 CPU core 的尺寸

1.  关闭L3（2.5MB），关闭L2（256KB），此时性能CPI（越小越好）是4.25
1.  关闭L3，打开L2（256KB），此时性能CPI为2.23
1.  关闭L3，打开L2同时增加256KB，L2尺寸到512KB，性能CPI为1.38
1.  打开L3（2.5MB），打开L2（256KB），性能为1.28 ，该状态就是intel CPU出厂的状态
1.  打开L3，增加到16MB，打开L2（256KB），性能为1.25

上面的数据显示当L3关闭之后，从case 3 开始，L2仅仅增加256KB，L2芯片面积相对于CPU core 增加 5%(0.5 /2.5M * 025M)，性能相对于case 2 提升1.61倍（2.23/1.38），而使用case 4 ,L3 2.5MB打开，相对于case 3，增加2.3MB（2.5MB - 256KB）,芯片面积相对于CPU core 增加 46%（0.5/2.5M * 2.3M）， 而性能仅仅提升 1.07倍（1.38/1.28），所以14年给Intel提议需要增加L2尺寸降低L3尺寸，这些数据促使Intel开始重新考虑对于数据中心缓存新的设计。

2014年的 Broadwell 的第五代智能酷睿处理器，是 Haswell 的 14nm 升级版（$1745.00 - $1749.00）：

![image-20210719102039296](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/image-20210719102039296.png)

E5一个Die有16个物理core（上面截图是两个Core），所以每core的L3大小：40M/16=2.5M/core

2015年则推出 SkyLake 架构的Platinum 8269CY（$4702.00）, 每core的L3大小：36M/26=1.38M/core：

![image-20210719102112331](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/image-20210719102112331.png)

Intel 2015年 发表论文[《High Performing Cache Hierarchies for Server Workloads》](https://people.csail.mit.edu/emer/papers/2015.02.hpca.cache_hierarchy.pdf)证明了阿里提出的建议的正确性，从Skylake架构开始将L2 cache 由 256KB 升级到 1MB， L3由2.5MB /core 压缩到 1.375MB / core， Intel之所以没有完全去掉L3的原因是希望这样设计的CPU对于 使用 CPU2006的workload性能仍然能够做到不受影响。

![image-20210716102624566](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/image-20210716102624566.png)

上图是不同业务场景下，CPI 随cache大小的变化，可以看到随着cache增加性能基本不增加了。

### CPU L2, Last Level Cache (LLC) 缓存的演变

Last Level Cache(L3) 在2016年之前都是2MB/core 或者 2.5MB/core, 这个原因取决于在此之前行业都是使用CPU2006作为设计CPU的benchmark，如下图所示：

![img](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/141f4ccd-37ce-41e5-b404-101e6b9acf5d.png)

根据上图中CPU2006的MPKI数据显示如果LLC在4MB的时候非常好，LLC在2.5MB之后MKPI提升10%性能只有1～3%的提升，2.5MB LLC cache是 CPU core 1/2 的芯片面积，因此若将LLC 由2.5MB升级到4MB，换算成CPU core的芯片面积是增长30%（1/2 * 1.5M/2.5M），但性能仅仅提升最多3%，这就是为什么基于CPU2006的benchmark条件下，intel将LLC设定为2~2.5MB的原因。

最后再附加几个Latency数据，让大家比较起来更有体感一些

## 各级IO延迟数字

2012 年延迟数字对比表

![image-20210702161817496](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/image-20210702161817496.png)

一个比较有体感的比较：如果 CPU 访问 L1 缓存需要 1 秒，那么访问主存需要 3 分钟、从 SSD 中随机读取数据需要 3.4 天、磁盘寻道需要 2 个月，网络传输可能需要 1 年多的时间。

当然更古老一点的年代给出来的数据可能又不一样一点，但是基本比例差异还是差不多的：

![Memory Hierarchy](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/cache-hierarchy-1.jpg)

[推荐从这里看延时，拖动时间轴可以看到随着技术、工艺的改变Latency每一年的变化](https://colin-scott.github.io/personal_website/research/interactive_latency.html)

![image-20210613123006681](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/image-20210613123006681.png)

查看cpu cache数据

```
cat /proc/cpuinfo |grep -i cache

```

![image.png](https://ata2-img.cn-hangzhou.oss-pub.aliyun-inc.com/ad19b92ccc97763aa7f78d8d1d514c84.png)

### L1C、L2C、L3C、DDR 的Latency测试数据

[下图从左至右响应时间分别是L1C、L2C、L3C、DDR](https://topic.atatech.org/articles/100065)，可以看出这四个Latency变化还是非常明显的，泾渭分明。

![img](https://ata2-img.oss-cn-zhangjiakou.aliyuncs.com/58286da947132f269cb26ff3eda25c68.png)

![image-20210511160107225](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/image-20210511160107225.png)

![image.png](https://ata2-img.cn-hangzhou.oss-pub.aliyun-inc.com/f5728a2afb29c653a3e1bf21f4d56056.png)

## 为什么CACHE比内存快？

首先肯定是距离的原因，另外工艺不同导致的速度差异也很大，从上面可以看到一块4000刀的CPU有一半的面积是cache，也就是40M CACHE花了2000刀，如果用来买内存条能卖一大堆吧。

接下来说下CACHE（SRAM) 和内存（DRAM）的工艺差异

### SRAM（Static Random-Access Memory，静态随机存取存储器）的芯片

CPU Cache 用的是一种叫作 SRAM（Static Random-Access Memory，静态随机存取存储器）的芯片。

SRAM 之所以被称为"静态"存储器，是因为只要处在通电状态，里面的数据就可以保持存在。而一旦断电，里面的数据就会丢失了。在 SRAM 里面，一个比特的数据，需要 6～8 个晶体管。所以 SRAM 的存储密度不高。同样的物理空间下，能够存储的数据有限。不过，因为 SRAM 的电路简单，所以访问速度非常快。

L1和L2一般是SRAM， L1的容量通常比L2小，容量大的SRAM访问时间就越长，同样制程和设计的情况下，**访问延时与容量的开方大致是成正比**的。

另外工作原理不同速度差异也不一样，L1就是讲究快，比如L1是N路组相联，N路阻相联的意思就是N个Cache单元同时读取数据（有点类似RAID0）。

L3用的还是SRAM，但是在考虑换成STT-MRAM，这样容量更大。

### DRAM（Dynamic Random Access Memory，动态随机存取存储器）的芯片

为磁芯存储器画上句号的是集成电路随机存储器件。1966年，IBM Thomas J. Watson研究中心的Dr. Robert H. Dennard(人物照如下)开发出了单个单元的动态随机存储器DRAM，DRAM每个单元包含一个开关晶体管和一个电容，利用电容中的电荷存储数据。因为电容中的电荷会泄露，需要每个周期都进行刷新重新补充电量，所以称其为动态随机存储器。

内存用的芯片和 Cache 有所不同，它用的是一种叫作 DRAM（Dynamic Random Access Memory，动态随机存取存储器）的芯片，比起 SRAM 来说，它的密度更高，有更大的容量，而且它也比 SRAM 芯片便宜不少。

动态随机存取存储器（DRAM）是一种半导体存储器，主要的作用原理是利用电容内存储电荷的多寡来代表一个二进制比特（bit）是1还是0。由于**在现实中晶体管会有漏电电流的现象**，导致电容上所存储的电荷数量并不足以正确的判别数据，而导致数据毁损。因此对于DRAM来说，周期性地充电是一个无可避免的要件。由于这种需要定时刷新的特性，因此被称为“动态”存储器。相对来说，静态存储器（SRAM）只要存入数据后，纵使不刷新也不会丢失记忆。

DRAM 的一个比特，只需要一个晶体管和一个电容就能存储。所以，DRAM 在同样的物理空间下，能够存储的数据也就更多，也就是存储的"密度"更大。DRAM 的数据访问电路和刷新电路都比 SRAM 更复杂，所以访问延时也就更长。

![img](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/d39b0f2b3962d646133d450541fb75a6.png)

SRAM是比**DRAM**更为昂贵，但更为快速、非常低功耗（特别是在空闲状态）。 因此**SRAM**首选用于带宽要求高，或者功耗要求低，或者二者兼而有之。 **SRAM**比起**DRAM**更为容易控制，也更是随机访问。 由于复杂的内部结构，**SRAM**比**DRAM**的占用面积更大，因而不适合用于更高储存密度低成本的应用，如PC内存。

### SRAM和DRAM原理比较

[简单说DRAM只有一个晶体管和一个电容，SRAM就复杂多了，需要6个晶体管](https://mp.weixin.qq.com/s?__biz=MzI2NDYwMDAxOQ==&amp;mid=2247483772&amp;idx=1&amp;sn=d7c188247b9851f7985676e2f9dd9a0e&amp;chksm=eaab61c0dddce8d62bdb521de1ada13142264882feae1ff06d6dcd81430a0063377e4b34cedb&amp;scene=178&amp;cur_album_id=1368835510680272898#rd)

![What is the difference between SRAM and DRAM](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/image-20210603114550646.png)

详细比较：

![Difference Between SRAM and DRAM - YouTube](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU性能和CACHE/maxresdefault.jpg)

## 系列文章

[CPU的制造和概念](/2021/06/01/CPU%E7%9A%84%E5%88%B6%E9%80%A0%E5%92%8C%E6%A6%82%E5%BF%B5/)

[CPU 性能和Cache Line](/2021/05/16/CPU Cache Line 和性能/)

[Perf IPC以及CPU性能](/2021/05/16/Perf IPC以及CPU利用率/)

[Intel、海光、鲲鹏920、飞腾2500 CPU性能对比](/2021/06/18/%E5%87%A0%E6%AC%BECPU%E6%80%A7%E8%83%BD%E5%AF%B9%E6%AF%94/)

[飞腾ARM芯片(FT2500)的性能测试](/2021/05/15/%E9%A3%9E%E8%85%BEARM%E8%8A%AF%E7%89%87(FT2500)%E7%9A%84%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95/)

[十年后数据库还是不敢拥抱NUMA？](/2021/05/14/%E5%8D%81%E5%B9%B4%E5%90%8E%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%98%E6%98%AF%E4%B8%8D%E6%95%A2%E6%8B%A5%E6%8A%B1NUMA/)

[一次海光物理机资源竞争压测的记录](/2021/03/07/%E4%B8%80%E6%AC%A1%E6%B5%B7%E5%85%89%E7%89%A9%E7%90%86%E6%9C%BA%E8%B5%84%E6%BA%90%E7%AB%9E%E4%BA%89%E5%8E%8B%E6%B5%8B%E7%9A%84%E8%AE%B0%E5%BD%95/)

[Intel PAUSE指令变化是如何影响自旋锁以及MySQL的性能的](/2019/12/16/Intel PAUSE指令变化是如何影响自旋锁以及MySQL的性能的/)

## 参考资料

[数据中心CPU探索和分析](https://www.atatech.org/articles/209957)



Reference:

