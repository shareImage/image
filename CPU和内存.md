
# CPU和内存

存储器主要是DRAM(53%)和Flash（NAND Flash-45%、NOR Flash-52%）

DRAM 和 NAND Flash 在 2019 年的供应商市场份额:

![img](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/5de1be14bc3680eebace3236110bae58.png)

## SRAM（Static Random-Access Memory，静态随机存取存储器）的芯片

CPU Cache 用的是一种叫作 SRAM（Static Random-Access Memory，静态随机存取存储器）的芯片。

SRAM 之所以被称为"静态"存储器，是因为只要处在通电状态，里面的数据就可以保持存在。而一旦断电，里面的数据就会丢失了。在 SRAM 里面，一个比特的数据，需要 6～8 个晶体管。所以 SRAM 的存储密度不高。同样的物理空间下，能够存储的数据有限。不过，因为 SRAM 的电路简单，所以访问速度非常快。

L1和L2一般是SRAM， L1的容量通常比L2小，容量大的SRAM访问时间就越长，同样制程和设计的情况下，**访问延时与容量的开方大致是成正比**的。离CPU越近速度越快。另外工作原理不同速度差异也不一样，L1就是讲究快，比如L1是N路组相联，N路阻相联的意思就是N个Cache单元同时读取数据（有点类似RAID0）。

L3用的还是SRAM，但是在考虑换成STT-MRAM，这样容量更大。

## DRAM（Dynamic Random Access Memory，动态随机存取存储器）的芯片

为磁芯存储器画上句号的是集成电路随机存储器件。1966年，IBM Thomas J. Watson研究中心的Dr. Robert H. Dennard(人物照如下)开发出了单个单元的动态随机存储器DRAM，DRAM每个单元包含一个开关晶体管和一个电容，利用电容中的电荷存储数据。因为电容中的电荷会泄露，需要每个周期都进行刷新重新补充电量，所以称其为动态随机存储器。

内存用的芯片和 Cache 有所不同，它用的是一种叫作 DRAM（Dynamic Random Access Memory，动态随机存取存储器）的芯片，比起 SRAM 来说，它的密度更高，有更大的容量，而且它也比 SRAM 芯片便宜不少。

动态随机存取存储器（DRAM）是一种半导体存储器，主要的作用原理是利用电容内存储电荷的多寡来代表一个二进制比特（bit）是1还是0。由于**在现实中晶体管会有漏电电流的现象**，导致电容上所存储的电荷数量并不足以正确的判别数据，而导致数据毁损。因此对于DRAM来说，周期性地充电是一个无可避免的要件。由于这种需要定时刷新的特性，因此被称为“动态”存储器。相对来说，静态存储器（SRAM）只要存入数据后，纵使不刷新也不会丢失记忆。

DRAM 的一个比特，只需要一个晶体管和一个电容就能存储。所以，DRAM 在同样的物理空间下，能够存储的数据也就更多，也就是存储的"密度"更大。DRAM 的数据访问电路和刷新电路都比 SRAM 更复杂，所以访问延时也就更长。

![img](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/d39b0f2b3962d646133d450541fb75a6.png)

SRAM是比**DRAM**更为昂贵，但更为快速、非常低功耗（特别是在空闲状态）。 因此**SRAM**首选用于带宽要求高，或者功耗要求低，或者二者兼而有之。 **SRAM**比起**DRAM**更为容易控制，也更是随机访问。 由于复杂的内部结构，**SRAM**比**DRAM**的占用面积更大，因而不适合用于更高储存密度低成本的应用，如PC内存。

### SRAM和DRAM比较

[简单说DRAM只有一个晶体管和一个电容，SRAM就复杂多了，需要6个晶体管](https://mp.weixin.qq.com/s?__biz=MzI2NDYwMDAxOQ==&amp;mid=2247483772&amp;idx=1&amp;sn=d7c188247b9851f7985676e2f9dd9a0e&amp;chksm=eaab61c0dddce8d62bdb521de1ada13142264882feae1ff06d6dcd81430a0063377e4b34cedb&amp;scene=178&amp;cur_album_id=1368835510680272898#rd)

![What is the difference between SRAM and DRAM](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/image-20210603114550646.png)

详细比较：

![Difference Between SRAM and DRAM - YouTube](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/maxresdefault.jpg)

## Cache

[各级cache的Latency](http://www.webstersystems.co.uk/threads.htm)：

![Cycle times](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/cycle_times.jpg)

[推荐从这里看延时，拖动时间轴可以看到每一年的变化](https://colin-scott.github.io/personal_website/research/interactive_latency.html)

![image-20210613123006681](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/image-20210613123006681.png)

## 双通道

双通道，也就是Dual Channel技术，让两个通道同时访问内存，互不干扰，大家一看感觉能够增大内存带宽一倍，一定是对性能有好处的。但因为写程序，代码和数据都是连续分布的，这也叫做内存访问的局部性。虚拟地址和物理地址分配也最小是以4K为最小颗粒，所有操作系统通过页表也不能改善这种状况。如果双通道，但各个内存通道内地址空间是连续的话，内存访问的局部性，会让性能的提高大打折扣。那怎么办呢？

通过**Interleave**。与服务器领域复杂无比的Interleave模式不同，笔记本和台式机只有一种Interleave，即按照Cache Line Interleave，也就是64个字节Interleave一次。换句话说，就是从0地址开始，每64个字节，它在两个Channel的分配就交换一次：

***0B~63B --> Channel 0***

***64B~127B -->Channel 1***

***128B ~ 191B -->Channel 0***

以此类推。

这样做好处是明显的。因为CPU内存访问都是以Cache Line为单位，又因为内存访问的局部性，两次Cache Line miss而访问内存，很大几率能够被分配到两条Channel上，大大提高了内存带宽的利用率。

但一般情况下，要采用对称双通道技术，内存就要对称，要求两个channel的内存要大小一样才行。那么我们用两个大小不同的内存条，是不是就不能Interleave了呢？本来是不行的，直到Intel推出了Flex Memory技术。

## 内存大小和CPU

一台服务器最多能插多少内存？目前单条内存最大为128G，一个物理CPU(一路，socket) 能接4个SMB(Scalable Memory Buffer)，一个SMB对应6个内存插槽。如果是8路的E5至强CPU那么最大支持：

> (4X6)X8=192条内存，192X128G=24T 内存


如下图中的红框就是SMB，篮筐是内存插槽，一个CPU对应两个这样的板子，内存条插在一个叫做SMB的芯片后面

![image-20210602114931825](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/image-20210602114931825.png)

这个板子叫做Memory Riser，红框的部分就是SMB，有两个。蓝色的部分是内存插槽。大家数一下，可以看到一个SMB后面可以插6根DIMM，分别属于两个Channel，上图只有两个SMB，另外两个在另外一个Memory Riser上，一个CPU可以接两个Memory Riser

## 内存和cache的latency对比

![latency](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/latency.png)

## Intel CPU内存性能测试

测试工具来自[这里](https://software.intel.com/content/www/us/en/develop/articles/intelr-memory-latency-checker.html)

V42机型E5-2682 v4测试结果：

```
#lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                64
On-line CPU(s) list:   0-63
Thread(s) per core:    2
Core(s) per socket:    16
Socket(s):             2
NUMA node(s):          2
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 79
Model name:            Intel(R) Xeon(R) CPU E5-2682 v4 @ 2.50GHz
Stepping:              1
CPU MHz:               2500.000
CPU max MHz:           3000.0000
CPU min MHz:           1200.0000
BogoMIPS:              5000.06
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              40960K
NUMA node0 CPU(s):     0-15,32-47
NUMA node1 CPU(s):     16-31,48-63
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 ds_cpl vmx smx est tm2 ssse3 fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch ida arat epb invpcid_single pln pts dtherm spec_ctrl ibpb_support tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm rdt rdseed adx smap xsaveopt cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local cat_l3

#./mlc
Intel(R) Memory Latency Checker - v3.9
Measuring idle latencies (in ns)...
    	Numa node
Numa node	     0	     1
       0	  85.0	 136.3
       1	 137.2	  84.2

Measuring Peak Injection Memory Bandwidths for the system
Bandwidths are in MB/sec (1 MB/sec = 1,000,000 Bytes/sec)
Using all the threads from each core if Hyper-threading is enabled
Using traffic with the following read-write ratios
ALL Reads        :	101491.2
3:1 Reads-Writes :	96604.5
2:1 Reads-Writes :	75687.1
1:1 Reads-Writes :	63222.9
Stream-triad like:	106032.5

Measuring Memory Bandwidths between nodes within system
Bandwidths are in MB/sec (1 MB/sec = 1,000,000 Bytes/sec)
Using all the threads from each core if Hyper-threading is enabled
Using Read-only traffic type
    	Numa node
Numa node	     0	     1
       0	62174.1	29696.3
       1	30361.6	62821.9

Measuring Loaded Latencies for the system
Using all the threads from each core if Hyper-threading is enabled
Using Read-only traffic type
Inject	Latency	Bandwidth
Delay	(ns)	MB/sec
==========================
 00000	295.67	 124989.0
 00002	292.62	 125189.5
 00008	295.38	 125042.0
 00015	290.07	 125258.1
 00050	340.17	 124643.1
 00100	283.22	 124123.5
 00200	131.79	  93700.8
 00300	115.01	  64086.0
 00400	109.75	  48703.6
 00500	100.85	  39328.6
 00700	 97.97	  28445.9
 01000	 90.63	  20239.2
 01300	 89.27	  15773.4
 01700	 88.23	  12257.3
 02500	 87.33	   8592.7
 03500	 86.67	   6357.4
 05000	 86.67	   4676.2
 09000	 91.17	   2890.5
 20000	 89.00	   1702.1

Measuring cache-to-cache transfer latency (in ns)...
Local Socket L2->L2 HIT  latency	42.1
Local Socket L2->L2 HITM latency	46.3
Remote Socket L2->L2 HITM latency (data address homed in writer socket)
    		Reader Numa Node
Writer Numa Node     0	     1
            0	     -	 108.6
            1	 108.8	     -
Remote Socket L2->L2 HITM latency (data address homed in reader socket)
    		Reader Numa Node
Writer Numa Node     0	     1
            0	     -	 109.7
            1	 110.1	     -
```

V62机型 Platinum 8269CY的测试结果：

```
#./mlc
Intel(R) Memory Latency Checker - v3.9
Measuring idle latencies (in ns)...
    Numa node
Numa node      0       1
       0    78.6   144.1
       1   144.7    78.5

Measuring Peak Injection Memory Bandwidths for the system
Bandwidths are in MB/sec (1 MB/sec = 1,000,000 Bytes/sec)
Using all the threads from each core if Hyper-threading is enabled
Using traffic with the following read-write ratios
ALL Reads        :  225318.5
3:1 Reads-Writes :  212855.2
2:1 Reads-Writes :  211005.5
1:1 Reads-Writes :  199156.8
Stream-triad like:  190504.7

Measuring Memory Bandwidths between nodes within system
Bandwidths are in MB/sec (1 MB/sec = 1,000,000 Bytes/sec)
Using all the threads from each core if Hyper-threading is enabled
Using Read-only traffic type
    Numa node
Numa node      0       1
       0  113322.6  50882.8
       1  50869.9 113450.5

Measuring Loaded Latencies for the system
Using all the threads from each core if Hyper-threading is enabled
Using Read-only traffic type
Inject  Latency Bandwidth
Delay (ns)  MB/sec
==========================
 00000  264.14   225549.2
 00002  262.26   225611.7
 00008  263.60   225528.7
 00015  263.99   225948.2
 00050  259.76   225921.5
 00100  259.25   225908.7
 00200  131.78   195867.9
 00300  102.90   133820.1
 00400   95.40   101360.2
 00500   91.64    81599.5
 00700   87.66    58799.0
 01000   85.62    41545.1
 01300   84.10    32227.5
 01700   83.35    24864.6
 02500   82.74    17184.5
 03500   82.81    12508.6
 05000   81.98     9001.1
 09000   81.81     5351.4
 20000   79.52     2862.4

Measuring cache-to-cache transfer latency (in ns)...
Local Socket L2->L2 HIT  latency  50.1
Local Socket L2->L2 HITM latency  50.1
Remote Socket L2->L2 HITM latency (data address homed in writer socket)
      Reader Numa Node
Writer Numa Node     0       1
            0      -   111.3
            1  111.0       -
Remote Socket L2->L2 HITM latency (data address homed in reader socket)
      Reader Numa Node
Writer Numa Node     0       1
            0      -   174.2
            1  176.8       -

[root@numaopen.et93 ]
#lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                104
On-line CPU(s) list:   0-103
Thread(s) per core:    2
Core(s) per socket:    26
Socket(s):             2
NUMA node(s):          2
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 85
Model name:            Intel(R) Xeon(R) Platinum 8269CY CPU @ 2.50GHz
Stepping:              7
CPU MHz:               3200.000
CPU max MHz:           3800.0000
CPU min MHz:           1200.0000
BogoMIPS:              4998.89
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              1024K
L3 cache:              36608K
NUMA node0 CPU(s):     0-25,52-77
NUMA node1 CPU(s):     26-51,78-103
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch ida arat epb invpcid_single pln pts dtherm spec_ctrl ibpb_support tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm mpx rdt avx512f avx512dq rdseed adx smap clflushopt avx512cdavx512bw avx512vl xsaveopt xsavec xgetbv1 cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local cat_l3 mba
```

从上面两个测试结果来看，因为都是一个Socket一个NUMA Node，所以两个机型NUMA Node之间的latency差异不大，当然同一个机型跨NUMA Latency差了快1倍了。

但是V62明显带宽更强（包括跨NUMA的带宽），大了有一倍多了。

## cpu到内存的带宽

![image-20210714202151597](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/image-20210714202151597.png)

## DDR bandwidth and latency

![image-20210714203416058](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/image-20210714203416058.png)

![image-20210714203345953](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/image-20210714203345953.png)

## 存储芯片能力

![img](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/89850a9cfdbcf6422843efbf395839eb.jpg)

![img](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/c74bfce9-dd44-40e2-a6ff-4ac5a7932c3d.png)

## 多 Die 异构封装

下面 Intel 的最新 7nm GPU 的芯片，把不同功能多个小硅片封装在一起。这里是 16 个 CPU Die、8 个 Cache Die、8 个 HBM Die、2 个 IO Die，还有几个未标明功能的填充或者测试用 Die，封装成一个 GPU 的芯片。

![img](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/2e37cc0cc5b997fb3f48284525ba68c1.jpeg)

未来 SoC 设计，会更重视功能分区，把追求最新工艺的逻辑部分，追求高密度的存储芯片，和在新工艺上收益不显著的模拟部分，分开设计，然后再封装在一起，取得最佳的设计性价比。看看最新一代的 Intel 和 AMD 的云端大芯片，都是多 Die（裸片）异构封装了

## 芯片设计：存内计算、整 Wafer 芯片、光子计算和量子计算

打破冯·诺伊曼架构，做存内计算。存内计算，顾名思义就是直接在存储内做计算。其具体实现方式有若干条技术路径。例如有望提高能效比 10 倍以上的模拟存内计算，就非常适合人工智能在边缘计算的场景。

超摩尔定律的光子芯片、量子计算等。特别是光子芯片，相对于硅芯片，在延时、带宽、功耗方面都有超 10 倍的优势，而且已经有公司在进行商业化实现了。

## 直接内存访问（Direct Memory Access）DMA

其实 DMA 技术很容易理解，本质上，DMA 技术就是我们在主板上放一块独立的芯片。在进行内存和 I/O 设备的数据传输的时候，我们不再通过 CPU 来控制数据传输，而直接通过 DMA 控制器（DMA Controller，简称 DMAC）。这块芯片，我们可以认为它其实就是一个协处理器（Co-Processor）。

在从硬盘读取数据的时候CPU发出指令给DMAC然后就不用管了，毕竟硬盘太慢，DMAC来将数据读取到内存完毕后再通知CPU。

### Kafka和DMA

Kafka 是一个用来处理实时数据的管道，我们常常用它来做一个消息队列，或者用来收集和落地海量的日志。作为一个处理实时数据和日志的管道，瓶颈自然也在 I/O 层面。

Kafka 里面会有两种常见的海量数据传输的情况。一种是从网络中接收上游的数据，然后需要落地到本地的磁盘上，确保数据不丢失。另一种情况呢，则是从本地磁盘上读取出来，通过网络发送出去。

比如在如下代码过程中，数据一共发生了四次传输的过程。其中两次是 DMA 的传输，另外两次，则是通过 CPU 控制的传输

```
File.read(fileDesc, buf, len);
Socket.send(socket, buf, len);
```

![img](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/e0e85505e793e804e3b396fc50871cd5.jpg)

类似零拷贝和bypass

最终kafka会利用Java NIO库里面的transferTo来将上面的4次搬运改成2次，并且这两次都不需要CPU参与

```
@Override
public long transferFrom(FileChannel fileChannel, long position, long count) throws IOException {
    return fileChannel.transferTo(position, count, socketChannel);
}
```

![img](https://cdn.jsdelivr.net/gh/plantegg/plantegg.github.io@_md2zhihu_blog_cee8f3b4/CPU和内存/596042d111ad9b871045d970a10464ab.jpg)

## Grub定制内存大小

物理内存768G，要求OS只能用512G, 修改grub为如下值：

```
# cat /boot/grub/grub.cfg  |grep 512
    linux	/vmlinuz-4.19.0-arm64-server root=UUID=dbc68010-8c36-40bf-b794-271e59ff5727 ro  splash quiet console=tty video=VGA-1:1280x1024@60 mem=512G DEEPIN_GFXMODE=$DEEPIN_GFXMODE
    	linux	/vmlinuz-4.19.0-arm64-server root=UUID=dbc68010-8c36-40bf-b794-271e59ff5727 ro  splash quiet console=tty video=VGA-1:1280x1024@60 mem=512G DEEPIN_GFXMODE=$DEEPIN_GFXMODE		
```

24条32G的内存条，总内存768G

```
# dmidecode -t memory |grep "Size: 32 GB"
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
    Size: 32 GB
root@uos15:/etc# dmidecode -t memory |grep "Size: 32 GB" | wc -l
24
```

## 内存带宽和延时对性能的影响

google在文章内指出，内存延时比内存带宽更容易成为性能的瓶颈。他们通过长时间观察线上机器的内存带宽利用率，发现在95%的时间内内存带宽都低于31%。我们抓取了各个业务的top-down指标，也发现了类似的规律，从top-down的占比上看，super queue中存在大量未完成的内存请求的占比的时间很少，更多的情况下只存在1-3个未完成的内存请求这种情况，因此在业务场景中内存延时的占比更为重要。未来架构演进的时候也要权衡这两个方面，尽量减少访存延时。
![image.png](/Users/ren/src/blog/951413iMgBlog/1612237323002-628961c6-835e-4426-abf6-489901b410fb.png#align=left&display=inline&height=107&margin=[object Object]&name=image.png&originHeight=213&originWidth=1363&size=33143&status=done&style=none&width=681.5)

谷歌对他们的一些线上的C++应用（ads、bigtable、disk、flightsearch、gmail、gmail-fe、indexing1、indexing2、search1、search2、search3、video）做了大量的profiling，发现前段取指令和译码导致的stall的比例占比在15-30%左右，并且前端的fetch latency在5%左右，是speccpu benchmark的2-3倍，他们认为导致这一问题的最大的可能：前端取指令的时候产生了较深的cache miss（一般L2 也cache miss，L2 指令和数据混合，指令压力更加明显），而造成cache miss最有可能的原因是业务的代码量比较大并且代码热点分散。目前从我们抓取的各个业务（搜索：rtp/HA3(qrs/seacher)、存储bs、数据库mysql、电商应用buy2）的top-down指标上来看，各个业务前端的压力在13%-43%之间，fetch latency普遍大于5%。这一点和他们的结论是类似的，未来架构的演进可以多考虑一下前端取指这一块，目前他们提出了三种较为可行的方案：
（1）比较直接的方法就是增大指令cache的大小。
（2）设计更加复杂的指令预取算法。
（3）在L2 cache设计的时候将指令和数据分离，或者在L2 cache替换的时候考虑指令的优先级大于数据。

<table>
<tr class="header">
<th></th>
<th>Frontend Bound</th>
<th>Backend Bound</th>
<th>Bad Speculation</th>
<th>Retiring</th>
</tr>
<tr class="odd">
<td>qrs</td>
<td>18.67</td>
<td>39.98</td>
<td>9.02</td>
<td>32.33</td>
</tr>
<tr class="even">
<td>searcher</td>
<td>14.24</td>
<td>19.64</td>
<td>25.58</td>
<td>40.54</td>
</tr>
<tr class="odd">
<td>rtp</td>
<td>13.33</td>
<td>45.08</td>
<td>6.73</td>
<td>34.86</td>
</tr>
<tr class="even">
<td>bs</td>
<td>14.05</td>
<td>55.62</td>
<td>2.43</td>
<td>27.89</td>
</tr>
<tr class="odd">
<td>mysql</td>
<td>43.40</td>
<td>13.07</td>
<td>5.99</td>
<td>37.54</td>
</tr>
<tr class="even">
<td>buy2</td>
<td>26.19</td>
<td>36.85</td>
<td>8.06</td>
<td>28.90</td>
</tr>
</table>

![image.png](/Users/ren/src/blog/951413iMgBlog/1611898911428-a155d9f4-3d71-4564-b0e7-2ff18a7dbc65.png#align=left&display=inline&height=121&margin=[object Object]&name=image.png&originHeight=242&originWidth=1280&size=87827&status=done&style=none&width=640)

## lmbench 测试

[内存测试命令，以及华为海思arm处理器：­Hisilicon Hi1620， 海光处理器Hygon C86 7185，Intel处理器8163性能比较](https://topic.atatech.org/articles/175032)

```
总带宽：
#taskset -c 0-96 -W 5 -N 5 -M 64M //其中a表示系统中逻辑核总数。

Die2Die测试命令, 其中j表示遍历Dide的内存节点，nodenum表示die中所包含的逻辑cpu数量。
numactl -C j ./stream -v1 -P $nodenum -W 5 -N 5 -M 64M

socket2socket测试命令, 其中cpusocket[表示第个中的逻辑数量，j]表示遍历所有socket的内存节点。
numactl -C {cpusocket[i]} -m {memsocket[j]} ./stream -v1 -P {numsocket[i]} -W 5 -N 5 -M 64M
```

内存延时测试命令：

```
延时测试命令, 其中cpus为die所在的某个cpu, j表示遍历所有die。
numactl -C j ./lat_mem_rd -P 1 -W 5 -N 5 -t 1024M
```

参考：https://winddoing.github.io/post/54953.html

[内存延时测试](https://topic.atatech.org/articles/107682)：

```
#./bin/lat_mem_rd 1 16 //1(范围, 单位M) 16(步长)
"stride=16
0.00049 1.540
0.00098 1.540
0.00195 1.540
0.00293 1.540
```

## 测试工具 toplev

`toplev`是一个基于`perf`和TMAM(Top-down Microarchitecture Analysis Method)方法的应用性能分析工具。从之前的[介绍文章 ](https://decodezp.github.io/2019/01/27/quickwords13-tma/)中可以了解到TMAM本质上是对CPU Performance Counter的整理和加工。取得Performance Counter的读数需要`perf`来协助，对读数的计算进而明确是Frondend bound还是Backend bound等等。

在最终计算之前，你大概需要做三件事：

-   明确CPU型号，因为不同的CPU，对应的PMU也不一样（依赖网络）
-   读取TMAM需要的`perf event`读数
-   按TMAM规定的算法计算，具体算法在这个[Excel表格](https://share.weiyun.com/5jNsb6o)里

这三步可以自动化地由程序来做。本质上`toplev`就是在做这件事。

`toplev`的[Github地址](https://github.com/andikleen/pmu-tools)：https://github.com/andikleen/pmu-tools

另外补充一下，TMAM作为一种`Top-down`方法，它一定是分级的。通过上一级的结果下钻，最终定位性能瓶颈。那么`toplev`在执行的时候，也一定是包含这个"等级"概念的。

pmu-tools 的工具在第一次运行的时会通过 `event_download.py` 把本机环境的 PMU 映射表自动下载下来, 但是前提是你的机器能正常连接 01.day 的网络. 很抱歉我司内部的服务器都是不行的, 因此 pmu-tools 也提供了手动下载的方式.

因此当我们的环境根本无法连接外部网络的时候, 我们只能通过其他机器下载实际目标环境的事件映射表下载到另一个系统上, 有点交叉编译的意思.

### [首先获取目标机器的 CPU 型号](https://oskernellab.com/2021/01/24/2021/0127-0001-Topdown_analysis_as_performed_on_Intel_CPU_using_pmu-tools/)

```
printf "GenuineIntel-6-%X\n" $(awk '/model\s+:/ { print $3 ; exit } ' /proc/cpuinfo )
```

> cpu的型号信息是由 vendor_id/cpu_family/model/stepping 等几个标记的.
> 
> 他其实标记了当前 CPU 是哪个系列那一代的产品, 对应的就是其微架构以及版本信息.
> 
> 注意我们使用了 %X 按照 16 进制来打印
> 
> 注意上面的命令显示制定了 vendor_id 等信息, 因为当前服务器端的 CPU 前面基本默认是 GenuineIntel-6 等.
> 
> 不过如果我们是其他机器, 最好查看下 cpufino 信息确认.


比如我这边机器的 CPU 型号为 :

```
processor       : 7
vendor_id       : GenuineIntel`
cpu family      : 6
model           : 85
model name      : Intel(R) Xeon(R) Gold 6161 CPU @ 2.20GHz
stepping        : 4
microcode       : 0x1
```

对应的结果就是 `GenuineIntel-6-55-4`.

我们也可以直接用 `-v` 打出来 CPU 信息.

```
$ python ./event_download.py  -v

My CPU GenuineIntel-6-55-4
```

### TMAM(Top-down Microarchitecture Analysis Method)

在最近的英特尔微体系结构上，流水线的 Front-end 每个 CPU 周期（cycle）可以分配4个 uOps ，而 Back-end 可以在每个周期中退役4个 uOps。 流水线槽（pipeline slot）代表处理一个 uOp 所需的硬件资源。 TMAM 假定对于每个 CPU 核心，在每个 CPU 周期内，有4个 pipeline slot 可用，然后使用专门设计的 PMU 事件来测量这些 pipeline slot 的使用情况。在每个 CPU 周期中，pipeline slot 可以是空的或者被 uOp 填充。 如果在一个 CPU 周期内某个 pipeline slot 是空的，称之为一次停顿（stall）。如果 CPU 经常停顿，系统性能肯定是受到影响的。TMAM 的目标就是确定系统性能问题的主要瓶颈。

下图展示并总结了乱序执行微体系架构中自顶向下确定性能瓶颈的分类方法。这种自顶向下的分析框架的优点是一种结构化的方法，有选择地探索可能的性能瓶颈区域。 带有权重的层次化节点，使得我们能够将分析的重点放在确实重要的问题上，同时无视那些不重要的问题。

![topdown.PNG](https://kernel.taobao.org/2019/03/Top-down-Microarchitecture-Analysis-Method/topdown.PNG)

例如，如果应用程序性能受到指令提取问题的严重影响， TMAM 将它分类为 Front-end Bound 这个大类。 用户或者工具可以向下探索并仅聚焦在 Front-end Bound 这个分类上，直到找到导致应用程序性能瓶颈的直接原因或一类原因。

### toplev实例

配置对应cpu型号的事件

> export EVENTMAP=/root/.cache/pmu-events/GenuineIntel-6-55-7-core.json  这样就可以了,上述资料中的两个export是可选的


```
# python toplev.py --core C0 --no-desc -l1 taskset -c 0  bash -c 'echo "7^199999" | bc > /dev/null'
Will measure complete system.
Using level 1.
# 4.2-full-perf on Intel(R) Xeon(R) Platinum 8269CY CPU @ 2.50GHz [clx/skylake]
S0-C0    BAD            Bad_Speculation  % Slots                   36.9  <==
S1-C0    BE             Backend_Bound    % Slots                   56.2  <==
Run toplev --describe Bad_Speculation^ Backend_Bound^ to get more information on bottlenecks
Add --nodes '!+Bad_Speculation*/2,+Backend_Bound*/2,+MUX' for breakdown.


```



Reference:

