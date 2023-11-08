
# 不同CPU性能大PK

## 前言

比较Hygon7280、Intel、AMD、鲲鹏920、飞腾2500的性能情况

<table>
<tr class="header">
<th>CPU型号</th>
<th>Hygon 7280</th>
<th>AMD 7H12</th>
<th>AMD 7T83</th>
<th>Intel 8163</th>
<th>鲲鹏920</th>
<th>飞腾2500</th>
<th>倚天710</th>
</tr>
<tr class="odd">
<td>物理核数</td>
<td>32</td>
<td>32</td>
<td>64</td>
<td>24</td>
<td>48</td>
<td>64</td>
<td>128core</td>
</tr>
<tr class="even">
<td>超线程</td>
<td>2</td>
<td>2</td>
<td>2</td>
<td>2</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td>路</td>
<td>2</td>
<td>2</td>
<td>2</td>
<td>2</td>
<td>2</td>
<td>2</td>
<td>1</td>
</tr>
<tr class="even">
<td>NUMA Node</td>
<td>8</td>
<td>2</td>
<td>4</td>
<td>2</td>
<td>4</td>
<td>16</td>
<td>2</td>
</tr>
<tr class="odd">
<td>L1d</td>
<td>32K</td>
<td>32K</td>
<td>32K</td>
<td>32K</td>
<td>64K</td>
<td>32K</td>
<td>64K</td>
</tr>
<tr class="even">
<td>L2</td>
<td>512K</td>
<td>512K</td>
<td>512K</td>
<td>1024K</td>
<td>512K</td>
<td>2048K</td>
<td>1024K</td>
</tr>
</table>

AMD 7T83 有8个Die, 每个Die L3大小 32M，L2 大小4MiB, 每个Die上  L1I/L1D 各256KiB，每个Die有8core，2、3代都是带有独立 IO Die
倚天710是一路服务器，单芯片2块对称的 Die

![image-20220528105526139](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/b881d7038bf01f4b-image-20220528105526139.png)

## 参与比较的几款CPU参数

IPC的说明：

> IPC: insns per cycle  insn/cycles  也就是每个时钟周期能执行的指令数量，越大程序跑的越快
> 
> 程序的执行时间 = 指令数/(主频*IPC) //单核下，多核的话再除以核数


### Hygon 7280

Hygon 7280 就是AMD Zen架构，最大IPC能到5.

```shell
架构：                           x86_64
CPU 运行模式：                   32-bit, 64-bit
字节序：                         Little Endian
Address sizes:                   43 bits physical, 48 bits virtual
CPU:                             128
在线 CPU 列表：                  0-127
每个核的线程数：                 2
每个座的核数：                   32
座：                             2
NUMA 节点：                      8
厂商 ID：                        HygonGenuine
CPU 系列：                       24
型号：                           1
型号名称：                       Hygon C86 7280 32-core Processor
步进：                           1
CPU MHz：                        2194.586
BogoMIPS：                       3999.63
虚拟化：                         AMD-V
L1d 缓存：                       2 MiB
L1i 缓存：                       4 MiB
L2 缓存：                        32 MiB
L3 缓存：                        128 MiB
NUMA 节点0 CPU：                 0-7,64-71
NUMA 节点1 CPU：                 8-15,72-79
NUMA 节点2 CPU：                 16-23,80-87
NUMA 节点3 CPU：                 24-31,88-95
NUMA 节点4 CPU：                 32-39,96-103
NUMA 节点5 CPU：                 40-47,104-111
NUMA 节点6 CPU：                 48-55,112-119
NUMA 节点7 CPU：                 56-63,120-127
```

**架构说明：**

每个CPU有4个Die，每个Die有两个CCX（2 core-Complexes），每个CCX最多有4core（例如7280/7285）共享一个L3 cache；每个Die有两个Memory Channel，每个CPU带有8个Memory Channel，并且每个Memory Channel最多支持2根Memory；

海光7系列架构图：

![image-20231108173339159](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/040c8247c449c534-image-20231108173339159.png)

曙光H620-G30A 机型硬件结构，CPU是hygon 7280（截图只截取了Socket0）

![image-20211231202402561](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/f1cde8e2eaf9f8b0-image-20211231202402561.png)

### AMD EPYC 7T83(NC)

两路服务器，4 numa node，[Z3架构](https://en.wikichip.org/wiki/amd/microarchitectures/zen_3)

![img](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/32cbfcafe6f7140c-AMD-EPYC-Milan-Zen-3-Server-CPU.png)

![image-20220902113036283](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/8e373c62f0b81719-image-20220902113036283.png)

![image-20220902113336705](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/753e9d9b04097933-image-20220902113336705.png)

详细信息：

```
#lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                256
On-line CPU(s) list:   0-255
Thread(s) per core:    2
Core(s) per socket:    64
Socket(s):             2
NUMA node(s):          4
Vendor ID:             AuthenticAMD
CPU family:            25
Model:                 1
Model name:            AMD EPYC 7T83 64-Core Processor
Stepping:              1
CPU MHz:               2154.005
CPU max MHz:           2550.0000
CPU min MHz:           1500.0000
BogoMIPS:              5090.93
Virtualization:        AMD-V
L1d cache:             32K
L1i cache:             32K
L2 cache:              512K
L3 cache:              32768K
NUMA node0 CPU(s):     0-31,128-159
NUMA node1 CPU(s):     32-63,160-191
NUMA node2 CPU(s):     64-95,192-223
NUMA node3 CPU(s):     96-127,224-255

#cat /sys/devices/system/cpu/cpu{0,1,8,16,30,31,32,128}/cache/index3/shared_cpu_map
00000000,00000000,00000000,000000ff,00000000,00000000,00000000,000000ff
00000000,00000000,00000000,000000ff,00000000,00000000,00000000,000000ff
00000000,00000000,00000000,0000ff00,00000000,00000000,00000000,0000ff00
00000000,00000000,00000000,00ff0000,00000000,00000000,00000000,00ff0000
00000000,00000000,00000000,ff000000,00000000,00000000,00000000,ff000000
00000000,00000000,00000000,ff000000,00000000,00000000,00000000,ff000000
00000000,00000000,000000ff,00000000,00000000,00000000,000000ff,00000000
00000000,00000000,00000000,000000ff,00000000,00000000,00000000,000000ff

#cat /sys/devices/system/cpu/cpu0/cache/index2/shared_cpu_map
00000000,00000000,00000000,00000001,00000000,00000000,00000000,00000001
```

L3是8个物理核，16个超线程共享，相当于单核2MB，一块CPU有8个L3，总共是256MB

```
#cat cpu0/cache/index3/shared_cpu_list
0-7,128-135
#cat cpu0/cache/index3/size
32768K
#cat cpu0/cache/index2/shared_cpu_list
0,128

#cat /sys/devices/system/cpu/cpu{0,1,8,16,30,31,32,128}/cache/index3/shared_cpu_list
0-7,128-135
0-7,128-135
8-15,136-143
16-23,144-151
24-31,152-159
24-31,152-159
32-39,160-167
0-7,128-135
```

L1D、L1I各为 2MiB，单物理核为32KB

空跑nop的IPC为6（有点吓人）

```
#perf stat ./cpu/test
 Performance counter stats for process id '449650':

          2,574.29 msec task-clock                #    1.000 CPUs utilized
                 0      context-switches          #    0.000 K/sec
                 0      cpu-migrations            #    0.000 K/sec
                 0      page-faults               #    0.000 K/sec
     8,985,622,182      cycles                    #    3.491 GHz                      (83.33%)
         4,390,929      stalled-cycles-frontend   #    0.05% frontend cycles idle     (83.34%)
     4,387,560,442      stalled-cycles-backend    #   48.83% backend cycles idle      (83.34%)
    53,711,907,863      instructions              #    5.98  insn per cycle
                                                  #    0.08  stalled cycles per insn  (83.34%)
       418,902,363      branches                  #  162.725 M/sec                    (83.34%)
            15,036      branch-misses             #    0.00% of all branches          (83.32%)

       2.574347594 seconds time elapsed
```

sysbench 测试7T83 比7H12 略好，可能是ECS、OS等带来的差异。

测试环境：4.19.91-011.ali4000.alios7.x86_64，5.7.34-log MySQL Community Server (GPL)

<table>
<tr class="header">
<th>测试核数</th>
<th>AMD EPYC 7H12 2.5G（QPS、IPC）</th>
<th>说明</th>
</tr>
<tr class="odd">
<td>单核</td>
<td>24363 0.58</td>
<td>CPU跑满</td>
</tr>
<tr class="even">
<td>一对HT</td>
<td>33519 0.40</td>
<td>CPU跑满</td>
</tr>
<tr class="odd">
<td>2物理核(0-1)</td>
<td>48423 0.57</td>
<td>CPU跑满</td>
</tr>
<tr class="even">
<td>2物理核(0,32) 跨node</td>
<td>46232 0.55</td>
<td>CPU跑满</td>
</tr>
<tr class="odd">
<td>2物理核(0,64) 跨socket</td>
<td>45072 0.52</td>
<td>CPU跑满</td>
</tr>
<tr class="even">
<td>4物理核(0-3)</td>
<td>97759 0.58</td>
<td>CPU跑满</td>
</tr>
<tr class="odd">
<td>16物理核(0-15)</td>
<td>367992 0.55</td>
<td>CPU跑满，sys占比20%，si 10%</td>
</tr>
<tr class="even">
<td>32物理核(0-31)</td>
<td>686998 0.51</td>
<td>CPU跑满，sys占比20%, si 12%</td>
</tr>
<tr class="odd">
<td>64物理核(0-63)</td>
<td>1161079 0.50</td>
<td>CPU跑到95%以上，sys占比20%, si 12%</td>
</tr>
<tr class="even">
<td>64物理核(0-31,64-95)</td>
<td>964441 0.49</td>
<td>socket2上的32核一直比较闲，数据无参考意义</td>
</tr>
<tr class="odd">
<td>64物理核(0-31,64-95)</td>
<td>1147846 0.48</td>
<td>重启mysqld，立即绑核，sysbench 在32-63上，导致0-31的CPU只能跑到89%</td>
</tr>
</table>

说明，压测过程动态通过taskset绑核，所以会有数据残留其它核的cache问题

跨socket taskset绑核的时候要压很久任务才会跨socket迁移过去，也就是刚taskset后CPU是跑不满的

```
#numastat -p 437803

Per-node process memory usage (in MBs) for PID 437803 (mysqld)
                           Node 0          Node 1          Node 2
                  --------------- --------------- ---------------
Huge                         0.00            0.00            0.00
Heap                         1.15            0.00         5403.27
Stack                        0.00            0.00            0.09
Private                   1921.60           16.22        10647.66
----------------  --------------- --------------- ---------------
Total                     1922.75           16.22        16051.02

                           Node 3           Total
                  --------------- ---------------
Huge                         0.00            0.00
Heap                         0.03         5404.45
Stack                        0.00            0.09
Private                     16.20        12601.68
----------------  --------------- ---------------
Total                       16.23        18006.22
```

### AMD EPYC 7H12(ECS)

AMD EPYC 7H12 64-Core（ECS，非物理机），最大IPC能到5.

```shell
# lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                64
On-line CPU(s) list:   0-63
Thread(s) per core:    2
Core(s) per socket:    16
座：                 2
NUMA 节点：         2
厂商 ID：           AuthenticAMD
CPU 系列：          23
型号：              49
型号名称：        AMD EPYC 7H12 64-Core Processor
步进：              0
CPU MHz：             2595.124
BogoMIPS：            5190.24
虚拟化：           AMD-V
超管理器厂商：  KVM
虚拟化类型：     完全
L1d 缓存：          32K
L1i 缓存：          32K
L2 缓存：           512K
L3 缓存：           16384K
NUMA 节点0 CPU：    0-31
NUMA 节点1 CPU：    32-63
```

AMD EPYC 7T83 ECS

```
[root@bugu88 cpu0]# cd /sys/devices/system/cpu/cpu0
[root@bugu88 cpu0]# cat cache/index0/size
32K
[root@bugu88 cpu0]# cat cache/index1/size
32K
[root@bugu88 cpu0]# cat cache/index2/size
512K
[root@bugu88 cpu0]# cat cache/index3/size
32768K
[root@bugu88 cpu0]# lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                16
On-line CPU(s) list:   0-15
Thread(s) per core:    2
Core(s) per socket:    8
座：                 1
NUMA 节点：         1
厂商 ID：           AuthenticAMD
CPU 系列：          25
型号：              1
型号名称：        AMD EPYC 7T83 64-Core Processor
步进：              1
CPU MHz：             2545.218
BogoMIPS：            5090.43
超管理器厂商：  KVM
虚拟化类型：     完全
L1d 缓存：          32K
L1i 缓存：          32K
L2 缓存：           512K
L3 缓存：           32768K
NUMA 节点0 CPU：    0-15
```

stream：

```
[root@bugu88 lmbench-master]# for i in $(seq 0 15); do echo $i; numactl -C $i -m 0 ./bin/stream -W 5 -N 5 -M 64M; done
0
STREAM copy latency: 0.68 nanoseconds
STREAM copy bandwidth: 23509.84 MB/sec
STREAM scale latency: 0.69 nanoseconds
STREAM scale bandwidth: 23285.51 MB/sec
STREAM add latency: 0.96 nanoseconds
STREAM add bandwidth: 25043.73 MB/sec
STREAM triad latency: 1.40 nanoseconds
STREAM triad bandwidth: 17121.79 MB/sec
1
STREAM copy latency: 0.68 nanoseconds
STREAM copy bandwidth: 23513.96 MB/sec
STREAM scale latency: 0.68 nanoseconds
STREAM scale bandwidth: 23580.06 MB/sec
STREAM add latency: 0.96 nanoseconds
STREAM add bandwidth: 25049.96 MB/sec
STREAM triad latency: 1.35 nanoseconds
STREAM triad bandwidth: 17741.93 MB/sec
```

### Intel 8163

这次对比测试的Intel 8163 CPU信息如下，最大IPC 是4：

```shell
#lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                96
On-line CPU(s) list:   0-95
Thread(s) per core:    2
Core(s) per socket:    24
Socket(s):             2
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 85
Model name:            Intel(R) Xeon(R) Platinum 8163 CPU @ 2.50GHz
Stepping:              4
CPU MHz:               2499.121
CPU max MHz:           3100.0000
CPU min MHz:           1000.0000
BogoMIPS:              4998.90
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              1024K
L3 cache:              33792K
NUMA node0 CPU(s):     0-95

-----8269CY
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
```

#### 不同 intel 型号的差异

如下图是8269CY和E5-2682上跑的MySQL在相同业务、相同流量下的差异：

![image-20221121105650582](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/46ed8b25036fbccd-image-20221121105650582.png)

CPU使用率差异(下图8051C是E5-2682，其它是 8269CY，主频也有30%的差异)

![image-20221121110004127](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/982080a0142f9670-image-20221121110004127.png)

### 鲲鹏920

```
[root@ARM 19:15 /root/lmbench3]
#numactl -H
available: 4 nodes (0-3)
node 0 cpus: 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
node 0 size: 192832 MB
node 0 free: 146830 MB
node 1 cpus: 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47
node 1 size: 193533 MB
node 1 free: 175354 MB
node 2 cpus: 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71
node 2 size: 193533 MB
node 2 free: 175718 MB
node 3 cpus: 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95
node 3 size: 193532 MB
node 3 free: 183643 MB
node distances:
node   0   1   2   3
  0:  10  12  20  22
  1:  12  10  22  24
  2:  20  22  10  12
  3:  22  24  12  10

  #lscpu
Architecture:          aarch64
Byte Order:            Little Endian
CPU(s):                96
On-line CPU(s) list:   0-95
Thread(s) per core:    1
Core(s) per socket:    48
Socket(s):             2
NUMA node(s):          4
Model:                 0
CPU max MHz:           2600.0000
CPU min MHz:           200.0000
BogoMIPS:              200.00
L1d cache:             64K
L1i cache:             64K
L2 cache:              512K
L3 cache:              24576K
NUMA node0 CPU(s):     0-23
NUMA node1 CPU(s):     24-47
NUMA node2 CPU(s):     48-71
NUMA node3 CPU(s):     72-95
Flags:                 fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp asimdhp cpuid asimdrdm jscvt fcma dcpop asimddp asimdfhm
```

### 飞腾2500

飞腾2500用nop去跑IPC的话，只能到1，但是跑其它代码能到2.33

```shell
#lscpu
Architecture:          aarch64
Byte Order:            Little Endian
CPU(s):                128
On-line CPU(s) list:   0-127
Thread(s) per core:    1
Core(s) per socket:    64
Socket(s):             2
NUMA node(s):          16
Model:                 3
BogoMIPS:              100.00
L1d cache:             32K
L1i cache:             32K
L2 cache:              2048K
L3 cache:              65536K
NUMA node0 CPU(s):     0-7
NUMA node1 CPU(s):     8-15
NUMA node2 CPU(s):     16-23
NUMA node3 CPU(s):     24-31
NUMA node4 CPU(s):     32-39
NUMA node5 CPU(s):     40-47
NUMA node6 CPU(s):     48-55
NUMA node7 CPU(s):     56-63
NUMA node8 CPU(s):     64-71
NUMA node9 CPU(s):     72-79
NUMA node10 CPU(s):    80-87
NUMA node11 CPU(s):    88-95
NUMA node12 CPU(s):    96-103
NUMA node13 CPU(s):    104-111
NUMA node14 CPU(s):    112-119
NUMA node15 CPU(s):    120-127
Flags:                 fp asimd evtstrm aes pmull sha1 sha2 crc32 cpuid

#perf stat ./nop
failed to read counter stalled-cycles-frontend
failed to read counter stalled-cycles-backend
failed to read counter branches

 Performance counter stats for './nop':

      78638.700540      task-clock (msec)         #    0.999 CPUs utilized
              1479      context-switches          #    0.019 K/sec
                55      cpu-migrations            #    0.001 K/sec
                37      page-faults               #    0.000 K/sec
      165127619524      cycles                    #    2.100 GHz
   <not supported>      stalled-cycles-frontend
   <not supported>      stalled-cycles-backend
      165269372437      instructions              #    1.00  insns per cycle
   <not supported>      branches
           3057191      branch-misses             #    0.00% of all branches

      78.692839007 seconds time elapsed

#dmidecode -t processor
# dmidecode 3.0
Getting SMBIOS data from sysfs.
SMBIOS 3.2.0 present.
# SMBIOS implementations newer than version 3.0 are not
# fully supported by this version of dmidecode.

Handle 0x0004, DMI type 4, 48 bytes
Processor Information
    Socket Designation: BGA3576
    Type: Central Processor
    Family: <OUT OF SPEC>
    Manufacturer: PHYTIUM
    ID: 00 00 00 00 70 1F 66 22
    Version: S2500
    Voltage: 0.8 V
    External Clock: 50 MHz
    Max Speed: 2100 MHz
    Current Speed: 2100 MHz
    Status: Populated, Enabled
    Upgrade: Other
    L1 Cache Handle: 0x0005
    L2 Cache Handle: 0x0007
    L3 Cache Handle: 0x0008
    Serial Number: N/A
    Asset Tag: No Asset Tag
    Part Number: NULL
    Core Count: 64
    Core Enabled: 64
    Thread Count: 64
    Characteristics:
    	64-bit capable
    	Multi-Core
    	Hardware Thread
    	Execute Protection
    	Enhanced Virtualization
    	Power/Performance Control
```

### 其它

2Die，2node

```
#lscpu
Architecture:          aarch64
Byte Order:            Little Endian
CPU(s):                128
On-line CPU(s) list:   0-127
Thread(s) per core:    1
Core(s) per socket:    128
Socket(s):             1
NUMA node(s):          2
Model:                 0
BogoMIPS:              100.00
L1d cache:             64K
L1i cache:             64K
L2 cache:              1024K
L3 cache:              65536K //64core share
NUMA node0 CPU(s):     0-63
NUMA node1 CPU(s):     64-127
Flags:                 fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp asimdhp cpuid asimdrdm jscvt fcma lrcpc dcpop sha3 sm3 sm4 asimddp sha512 sve asimdfhm dit uscat ilrcpc flagm ssbs sb dcpodp sve2 sveaes svepmull svebitperm svesha3 svesm4 flagm2 frint svei8mm svebf16 i8mm bf16 dgh

#cat cpu{0,1,8,16,30,31,32,127}/cache/index3/shared_cpu_list
0-63
0-63
0-63
0-63
0-63
0-63
0-63
64-127

#grep -E "core|64.000" lat.log
core:0
64.00000 59.653
core:8
64.00000 62.265
core:16
64.00000 59.411
core:24
64.00000 55.836
core:32
64.00000 55.909
core:40
64.00000 56.176
core:48
64.00000 57.240
core:56
64.00000 59.485
core:64
64.00000 131.818
core:72
64.00000 127.182
core:80
64.00000 122.452
core:88
64.00000 121.673
core:96
64.00000 126.533
core:104
64.00000 125.673
core:112
64.00000 124.188
core:120
64.00000 130.202

#numactl -H
available: 2 nodes (0-1)
node 0 cpus: 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63
node 0 size: 515652 MB
node 0 free: 514913 MB
node 1 cpus: 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99 100 101 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127
node 1 size: 516086 MB
node 1 free: 514815 MB
node distances:
node   0   1
  0:  10  15
  1:  15  10
```

## 单核以及HT计算Prime性能比较

以上两款CPU但从物理上的指标来看似乎AMD要好很多，从工艺上AMD也要领先一代(2年），从单核参数上来说是2.0 VS 2.5GHz，但是IPC 是5 VS 4，算下来理想的单核性能刚好一致（2*5=2.5 *4）。

从外面的一些跑分结果显示也是AMD 要好，但是实际性能怎么样呢？

测试命令，这个测试命令无论在哪个CPU下，用2个物理核用时都是一个物理核的一半，所以这个计算是可以完全并行的

```
taskset -c 1 /usr/bin/sysbench --num-threads=1 --test=cpu --cpu-max-prime=50000 run //单核用一个threads，绑核; HT用2个threads，绑一对HT
```

测试结果为耗时，单位秒

<table>
<tr class="header">
<th style="text-align: left;">测试项</th>
<th>AMD EPYC 7H12 2.5G CentOS 7.9</th>
<th>Hygon 7280 2.1GHz CentOS</th>
<th style="text-align: left;">Hygon 7280 2.1GHz 麒麟</th>
<th>Intel 8269 2.50G</th>
<th style="text-align: left;">Intel 8163 CPU @ 2.50GHz</th>
<th style="text-align: left;">Intel E5-2682 v4 @ 2.50GHz</th>
</tr>
<tr class="odd">
<td style="text-align: left;">单核 prime 50000 耗时</td>
<td>59秒 IPC 0.56</td>
<td>77秒 IPC 0.55</td>
<td style="text-align: left;">89秒 IPC 0.56;</td>
<td>83 0.41</td>
<td style="text-align: left;">105秒 IPC 0.41</td>
<td style="text-align: left;">109秒 IPC 0.39</td>
</tr>
<tr class="even">
<td style="text-align: left;">HT prime 50000 耗时</td>
<td>57秒 IPC 0.31</td>
<td>74秒 IPC 0.29</td>
<td style="text-align: left;">87秒 IPC 0.29</td>
<td>48 0.35</td>
<td style="text-align: left;">60秒 IPC 0.36</td>
<td style="text-align: left;">74秒 IPC 0.29</td>
</tr>
</table>

相同CPU下的 指令数 基本= 耗时 * IPC * 核数

以上测试结果显示Hygon 7280单核计算能力是要强过Intel 8163的，但是超线程在这个场景下太不给力，相当于没有。

当然上面的计算Prime太单纯了，代表不了复杂的业务场景，所以接下来用MySQL的查询场景来看看。

如果是arm芯片在计算prime上明显要好过x86，猜测是除法取余指令上有优化

```
#taskset -c 11 sysbench cpu --threads=1 --events=50000  run
sysbench 1.0.20 (using bundled LuaJIT 2.1.0-beta2)
```

测试结果为10秒钟的event

<table>
<tr class="header">
<th style="text-align: left;">测试项</th>
<th>FT2500 2.1G</th>
<th>鲲鹏920-4826 2.6GHz</th>
<th style="text-align: left;">Intel 8163 CPU @ 2.50GHz</th>
<th>Hygon C86 7280 2.1GHz</th>
<th>AMD 7T83</th>
</tr>
<tr class="odd">
<td style="text-align: left;">单核 prime 10秒 events</td>
<td>21626 IPC 0.89</td>
<td>30299 IPC 1.01</td>
<td style="text-align: left;">8435 IPC 0.41</td>
<td>10349 IPC 0.63</td>
<td>40112 IPC 1.38</td>
</tr>
</table>

## 对比MySQL sysbench和tpcc性能

分别将MySQL 5.7.34社区版部署到intel+AliOS以及hygon 7280+CentOS上，将mysqld绑定到单核，一样的压力配置均将CPU跑到100%，然后用sysbench测试点查， HT表示将mysqld绑定到一对HT核。

### sysbench点查

测试命令类似如下：

```
sysbench --test='/usr/share/doc/sysbench/tests/db/select.lua' --oltp_tables_count=1 --report-interval=1 --oltp-table-size=10000000  --mysql-port=3307 --mysql-db=sysbench_single --mysql-user=root --mysql-password='Bj6f9g96!@#'  --max-requests=0   --oltp_skip_trx=on --oltp_auto_inc=on  --oltp_range_size=5  --mysql-table-engine=innodb --rand-init=on   --max-time=300 --mysql-host=x86.51 --num-threads=4 run
```

测试结果(测试中的差异AMD、Hygon CPU跑在CentOS7.9， intel CPU、Kunpeng 920 跑在AliOS上, xdb表示用集团的xdb替换社区的MySQL Server， 麒麟是国产OS)：

<table>
<tr class="header">
<th style="text-align: left;">测试核数</th>
<th>AMD EPYC 7H12 2.5G</th>
<th style="text-align: left;">Hygon 7280 2.1G</th>
<th>Hygon 7280 2.1GHz 麒麟</th>
<th>Intel 8269 2.50G</th>
<th style="text-align: left;">Intel 8163 2.50G</th>
<th style="text-align: left;">Intel 8163 2.50G XDB5.7</th>
<th>鲲鹏 920-4826 2.6G</th>
<th>鲲鹏 920-4826 2.6G XDB8.0</th>
<th>FT2500 alisql 8.0 本地–socket</th>
</tr>
<tr class="odd">
<td style="text-align: left;">单核</td>
<td>24674 0.54</td>
<td style="text-align: left;">13441 0.46</td>
<td>10236 0.39</td>
<td>28208 0.75</td>
<td style="text-align: left;">25474 0.84</td>
<td style="text-align: left;">29376 0.89</td>
<td>9694 0.49</td>
<td>8301 0.46</td>
<td>3602 0.53</td>
</tr>
<tr class="even">
<td style="text-align: left;">一对HT</td>
<td>36157 0.42</td>
<td style="text-align: left;">21747 0.38</td>
<td>19417 0.37</td>
<td>36754 0.49</td>
<td style="text-align: left;">35894 0.6</td>
<td style="text-align: left;">40601 0.65</td>
<td>无HT</td>
<td>无HT</td>
<td>无HT</td>
</tr>
<tr class="odd">
<td style="text-align: left;">4物理核</td>
<td>94132 0.52</td>
<td style="text-align: left;">49822 0.46</td>
<td>38033 0.37</td>
<td>90434 0.69 350%</td>
<td style="text-align: left;">87254 0.73</td>
<td style="text-align: left;">106472 0.83</td>
<td>34686 0.42</td>
<td>28407 0.39</td>
<td>14232 0.53</td>
</tr>
<tr class="even">
<td style="text-align: left;">16物理核</td>
<td>325409 0.48</td>
<td style="text-align: left;">171630 0.38</td>
<td>134980 0.34</td>
<td>371718 0.69 1500%</td>
<td style="text-align: left;">332967 0.72</td>
<td style="text-align: left;">446290 0.85 //16核比4核好！</td>
<td>116122 0.35</td>
<td>94697 0.33</td>
<td>59199 0.6 8core:31210 0.59</td>
</tr>
<tr class="odd">
<td style="text-align: left;">32物理核</td>
<td>542192 0.43</td>
<td style="text-align: left;">298716 0.37</td>
<td>255586 0.33</td>
<td>642548 0.64 2700%</td>
<td style="text-align: left;">588318 0.67</td>
<td style="text-align: left;">598637 0.81 CPU 2400%</td>
<td>228601 0.36</td>
<td>177424 0.32</td>
<td>114020 0.65</td>
</tr>
</table>

-   麒麟OS下CPU很难跑满，大致能跑到90%-95%左右，麒麟上装的社区版MySQL-5.7.29；飞腾要特别注意mysqld所在socket，同时以上飞腾数据都是走--sock压测所得，32core走网络压测QPS为：99496（15%的网络损耗）[^说明]

### Mysqld 二进制代码所在 page cache带来的性能影响

如果是飞腾跨socket影响很大，mysqld二进制跨socket性能会下降30%以上

对于鲲鹏920，双路服务器上测试，mysqld绑在node0, 但是分别将mysqld二进制load进不同的node上的page cache，然后执行点查

<table>
<tr class="header">
<th>mysqld</th>
<th>node0</th>
<th>node1</th>
<th>node2</th>
<th>node3</th>
</tr>
<tr class="odd">
<td>QPS</td>
<td>190120 IPC 0.40</td>
<td>182518 IPC 0.39</td>
<td>189046 IPC 0.40</td>
<td>186533 IPC 0.40</td>
</tr>
</table>

以上数据可以看出这里node0到node1还是很慢的，居然比跨socket还慢，反过来说鲲鹏跨socket性能很好

绑定mysqld到不同node的page cache操作

```
#systemctl stop mysql-server

[root@poc65 /root/vmtouch]
#vmtouch -e /usr/local/mysql/bin/mysqld
           Files: 1
     Directories: 0
   Evicted Pages: 5916 (23M)
         Elapsed: 0.00322 seconds

#vmtouch -v /usr/local/mysql/bin/mysqld
/usr/local/mysql/bin/mysqld
[                                                            ] 0/5916

           Files: 1
     Directories: 0
  Resident Pages: 0/5916  0/23M  0%
         Elapsed: 0.000204 seconds

#taskset -c 24 md5sum /usr/local/mysql/bin/mysqld

#grep mysqld /proc/`pidof mysqld`/numa_maps  //检查mysqld具体绑定在哪个node上
00400000 default file=/usr/local/mysql/bin/mysqld mapped=3392 active=1 N0=3392 kernelpagesize_kB=4
0199b000 default file=/usr/local/mysql/bin/mysqld anon=10 dirty=10 mapped=134 active=10 N0=134 kernelpagesize_kB=4
01a70000 default file=/usr/local/mysql/bin/mysqld anon=43 dirty=43 mapped=120 active=43 N0=120 kernelpagesize_kB=4
```

### 网卡以及node距离带来的性能差异

在鲲鹏920+mysql5.7+alios，将内存分配锁在node0上，然后分别绑核在1、24、48、72core，进行sysbench点查对比

<table>
<tr class="header">
<th></th>
<th>Core1</th>
<th>Core24</th>
<th>Core48</th>
<th>Core72</th>
</tr>
<tr class="odd">
<td>QPS</td>
<td>10800</td>
<td>10400</td>
<td>7700</td>
<td>7700</td>
</tr>
</table>

以上测试的时候业务进程分配的内存全限制在node0上（下面的网卡中断测试也是同样内存结构）

```
#/root/numa-maps-summary.pl </proc/123853/numa_maps
N0        :      5085548 ( 19.40 GB)
N1        :         4479 (  0.02 GB)
N2        :            1 (  0.00 GB)
active    :            0 (  0.00 GB)
anon      :      5085455 ( 19.40 GB)
dirty     :      5085455 ( 19.40 GB)
kernelpagesize_kB:         2176 (  0.01 GB)
mapmax    :          348 (  0.00 GB)
mapped    :         4626 (  0.02 GB)
```

对比测试，将内存锁在node3上，重复进行以上测试结果如下：

<table>
<tr class="header">
<th></th>
<th>Core1</th>
<th>Core24</th>
<th>Core48</th>
<th>Core72</th>
</tr>
<tr class="odd">
<td>QPS</td>
<td>10500</td>
<td>10000</td>
<td>8100</td>
<td>8000</td>
</tr>
</table>

```
#/root/numa-maps-summary.pl </proc/54478/numa_maps
N0        :           16 (  0.00 GB)
N1        :         4401 (  0.02 GB)
N2        :            1 (  0.00 GB)
N3        :      1779989 (  6.79 GB)
active    :            0 (  0.00 GB)
anon      :      1779912 (  6.79 GB)
dirty     :      1779912 (  6.79 GB)
kernelpagesize_kB:         1108 (  0.00 GB)
mapmax    :          334 (  0.00 GB)
mapped    :         4548 (  0.02 GB)
```

机器上网卡eth1插在node0上，由以上两组对比测试发现网卡影响比内存跨node影响更大，网卡影响有20%。而内存的影响基本看不到（就近好那么一点点，但是不明显，只能解释为cache命中率很高了）

此时软中断都在node0上，如果将软中断绑定到node3上，第72core的QPS能提升到8500，并且非常稳定。同时core0的QPS下降到10000附近。

#### 网卡软中断以及网卡远近的测试结论

测试机器只是用了一块网卡，网卡插在node0上。

一般网卡中断会占用一些CPU，如果把网卡中断挪到其它node的core上，在鲲鹏920上测试，业务跑在node3（使用全部24core），网卡中断分别在node0和node3，QPS分别是：179000 VS 175000 （此时把中断放到node0或者是和node3最近的node2上差别不大）

如果将业务跑在node0上（全部24core），网卡中断分别在node0和node1上得到的QPS分别是：204000 VS 212000

### tpcc 1000仓

测试结果(测试中Hygon 7280分别跑在CentOS7.9和麒麟上， 鲲鹏/intel CPU 跑在AliOS、麒麟是国产OS)：

tpcc测试数据，结果为1000仓，tpmC (NewOrders) ，未标注CPU 则为跑满了

<table>
<tr class="header">
<th>测试核数</th>
<th>Intel 8269 2.50G</th>
<th>Intel 8163 2.50G</th>
<th>Hygon 7280 2.1GHz 麒麟</th>
<th>Hygon 7280 2.1G CentOS 7.9</th>
<th>鲲鹏 920-4826 2.6G</th>
<th>鲲鹏 920-4826 2.6G XDB8.0</th>
</tr>
<tr class="odd">
<td>1物理核</td>
<td>12392</td>
<td>9902</td>
<td>4706</td>
<td>7011</td>
<td>6619</td>
<td>4653</td>
</tr>
<tr class="even">
<td>一对HT</td>
<td>17892</td>
<td>15324</td>
<td>8950</td>
<td>11778</td>
<td>无HT</td>
<td>无HT</td>
</tr>
<tr class="odd">
<td>4物理核</td>
<td>51525</td>
<td>40877</td>
<td>19387 380%</td>
<td>30046</td>
<td>23959</td>
<td>20101</td>
</tr>
<tr class="even">
<td>8物理核</td>
<td>100792</td>
<td>81799</td>
<td>39664 750%</td>
<td>60086</td>
<td>42368</td>
<td>40572</td>
</tr>
<tr class="odd">
<td>16物理核</td>
<td>160798 抖动</td>
<td>140488 CPU抖动</td>
<td>75013 1400%</td>
<td>106419 1300-1550%</td>
<td>70581 1200%</td>
<td>79844</td>
</tr>
<tr class="even">
<td>24物理核</td>
<td>188051</td>
<td>164757 1600-2100%</td>
<td>100841 1800-2000%</td>
<td>130815 1600-2100%</td>
<td>88204 1600%</td>
<td>115355</td>
</tr>
<tr class="odd">
<td>32物理核</td>
<td>195292</td>
<td>185171 2000-2500%</td>
<td>116071 1900-2400%</td>
<td>142746 1800-2400%</td>
<td>102089 1900%</td>
<td>143567</td>
</tr>
<tr class="even">
<td>48物理核</td>
<td>19969l</td>
<td>195730 2100-2600%</td>
<td>128188 2100-2800%</td>
<td>149782 2000-2700%</td>
<td>116374 2500%</td>
<td>206055 4500%</td>
</tr>
</table>

tpcc并发到一定程度后主要是锁导致性能上不去，所以超多核意义不大。

如果在Hygon 7280 2.1GHz 麒麟上起两个MySQLD实例，每个实例各绑定32物理core，性能刚好翻倍：![image-20210823082702539](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/f6720dff2086a67d-image-20210823082702539.png)

测试过程CPU均跑满（未跑满的话会标注出来），IPC跑不起来性能就必然低，超线程虽然总性能好了但是会导致IPC降低(参考前面的公式)。可以看到对本来IPC比较低的场景，启用超线程后一般对性能会提升更大一些。

CPU核数增加到32核后，MySQL社区版性能追平xdb， 此时sysbench使用120线程压性能较好（AMD得240线程压）

32核的时候对比下MySQL 社区版在Hygon7280和Intel 8163下的表现：

![image-20210817181752243](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/b8f1effa8e0c64a4-image-20210817181752243.png)

## 三款CPU的性能指标

<table>
<tr class="header">
<th style="text-align: left;">测试项</th>
<th>AMD EPYC 7H12 2.5G</th>
<th style="text-align: left;">Hygon 7280 2.1GHz</th>
<th style="text-align: left;">Intel 8163 CPU @ 2.50GHz</th>
</tr>
<tr class="odd">
<td style="text-align: left;">内存带宽(MiB/s)</td>
<td>12190.50</td>
<td style="text-align: left;">6206.06</td>
<td style="text-align: left;">7474.45</td>
</tr>
<tr class="even">
<td style="text-align: left;">内存延时(遍历很大一个数组)</td>
<td>0.334ms</td>
<td style="text-align: left;">0.336ms</td>
<td style="text-align: left;">0.429ms</td>
</tr>
</table>

## 在lmbench上的测试数据

stream主要用于测试带宽，对应的时延是在带宽跑满情况下的带宽。

lat_mem_rd用来测试操作不同数据大小的时延。总的来说带宽看stream、时延看lat_mem_rd

### 飞腾2500

用stream测试带宽和latency，可以看到带宽随着numa距离不断减少、对应的latency不断增加，到最近的numa node有10%的损耗，这个损耗和numactl给出的距离完全一致。跨socket访问内存latency是node内的3倍，带宽是三分之一，但是socket1性能和socket0性能完全一致

```
time for i in $(seq 7 8 128); do echo $i; numactl -C $i -m 0 ./bin/stream -W 5 -N 5 -M 64M; done

#numactl -C 7 -m 0 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 2.84 nanoseconds
STREAM copy bandwidth: 5638.21 MB/sec
STREAM scale latency: 2.72 nanoseconds
STREAM scale bandwidth: 5885.97 MB/sec
STREAM add latency: 2.26 nanoseconds
STREAM add bandwidth: 10615.13 MB/sec
STREAM triad latency: 4.53 nanoseconds
STREAM triad bandwidth: 5297.93 MB/sec

#numactl -C 7 -m 1 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 3.16 nanoseconds
STREAM copy bandwidth: 5058.71 MB/sec
STREAM scale latency: 3.15 nanoseconds
STREAM scale bandwidth: 5074.78 MB/sec
STREAM add latency: 2.35 nanoseconds
STREAM add bandwidth: 10197.36 MB/sec
STREAM triad latency: 5.12 nanoseconds
STREAM triad bandwidth: 4686.37 MB/sec

#numactl -C 7 -m 2 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 3.85 nanoseconds
STREAM copy bandwidth: 4150.98 MB/sec
STREAM scale latency: 3.95 nanoseconds
STREAM scale bandwidth: 4054.30 MB/sec
STREAM add latency: 2.64 nanoseconds
STREAM add bandwidth: 9100.12 MB/sec
STREAM triad latency: 6.39 nanoseconds
STREAM triad bandwidth: 3757.70 MB/sec

#numactl -C 7 -m 3 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 3.69 nanoseconds
STREAM copy bandwidth: 4340.24 MB/sec
STREAM scale latency: 3.62 nanoseconds
STREAM scale bandwidth: 4422.18 MB/sec
STREAM add latency: 2.47 nanoseconds
STREAM add bandwidth: 9704.82 MB/sec
STREAM triad latency: 5.74 nanoseconds
STREAM triad bandwidth: 4177.85 MB/sec

[root@101a05001.cloud.a05.am11 /root/lmbench3]
#numactl -C 7 -m 7 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 3.95 nanoseconds
STREAM copy bandwidth: 4051.51 MB/sec
STREAM scale latency: 3.94 nanoseconds
STREAM scale bandwidth: 4060.63 MB/sec
STREAM add latency: 2.54 nanoseconds
STREAM add bandwidth: 9434.51 MB/sec
STREAM triad latency: 6.13 nanoseconds
STREAM triad bandwidth: 3913.36 MB/sec

[root@101a05001.cloud.a05.am11 /root/lmbench3]
#numactl -C 7 -m 10 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 8.80 nanoseconds
STREAM copy bandwidth: 1817.78 MB/sec
STREAM scale latency: 8.59 nanoseconds
STREAM scale bandwidth: 1861.65 MB/sec
STREAM add latency: 5.55 nanoseconds
STREAM add bandwidth: 4320.68 MB/sec
STREAM triad latency: 13.94 nanoseconds
STREAM triad bandwidth: 1721.76 MB/sec

[root@101a05001.cloud.a05.am11 /root/lmbench3]
#numactl -C 7 -m 11 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 9.27 nanoseconds
STREAM copy bandwidth: 1726.52 MB/sec
STREAM scale latency: 9.31 nanoseconds
STREAM scale bandwidth: 1718.10 MB/sec
STREAM add latency: 5.65 nanoseconds
STREAM add bandwidth: 4250.89 MB/sec
STREAM triad latency: 14.09 nanoseconds
STREAM triad bandwidth: 1703.66 MB/sec

[root@101a05001.cloud.a05.am11 /root/lmbench3]
#numactl -C 88 -m 11 ./bin/stream  -W 5 -N 5 -M 64M //在另外一个socket上测试本numa，和node0性能完全一致
STREAM copy latency: 2.93 nanoseconds
STREAM copy bandwidth: 5454.67 MB/sec
STREAM scale latency: 2.96 nanoseconds
STREAM scale bandwidth: 5400.03 MB/sec
STREAM add latency: 2.28 nanoseconds
STREAM add bandwidth: 10543.42 MB/sec
STREAM triad latency: 4.52 nanoseconds
STREAM triad bandwidth: 5308.40 MB/sec

[root@101a05001.cloud.a05.am11 /root/lmbench3]
#numactl -C 7 -m 15 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 8.73 nanoseconds
STREAM copy bandwidth: 1831.77 MB/sec
STREAM scale latency: 8.81 nanoseconds
STREAM scale bandwidth: 1815.13 MB/sec
STREAM add latency: 5.63 nanoseconds
STREAM add bandwidth: 4265.21 MB/sec
STREAM triad latency: 13.09 nanoseconds
STREAM triad bandwidth: 1833.68 MB/sec
```

Lat_mem_rd 用cpu7访问node0和node15对比结果，随着数据的加大，延时在加大，64M时能有3倍差距，和上面测试一致

下图 第一列 表示读写数据的大小（单位M），第二列表示访问延时（单位纳秒），一般可以看到在L1/L2/L3 cache大小的地方延时会有跳跃，远超过L3大小后，延时就是内存延时了

![image-20210924185044090](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/7e3922a54a9fc9e7-image-20210924185044090.png)

```
numactl -C 7 -m 0 ./bin/lat_mem_rd -W 5 -N 5 -t 64M  //-C 7 cpu 7, -m 0 node0, -W 热身 -t stride
```

同样的机型，开关numa的测试结果，关numa 时延、带宽都差了几倍

![image-20220323153507557](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/e62b2187cdfeb7d1-image-20220323153507557.png)

关闭numa的机器上测试结果随机性很强，这应该是和内存分配在那里有关系，不过如果机器一直保持这个状态反复测试的话，快的core一直快，慢的core一直慢，这是因为物理地址分配有一定的规律，在物理内存没怎么变化的情况下，快的core恰好分到的内存比较近。

同时不同机器状态（内存使用率）测试结果也不一样

### 鲲鹏920

```
#for i in $(seq 0 15); do echo core:$i; numactl -N $i -m 7 ./bin/stream  -W 5 -N 5 -M 64M; done
STREAM copy latency: 1.84 nanoseconds
STREAM copy bandwidth: 8700.75 MB/sec
STREAM scale latency: 1.86 nanoseconds
STREAM scale bandwidth: 8623.60 MB/sec
STREAM add latency: 2.18 nanoseconds
STREAM add bandwidth: 10987.04 MB/sec
STREAM triad latency: 3.03 nanoseconds
STREAM triad bandwidth: 7926.87 MB/sec

#numactl -C 7 -m 1 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 2.05 nanoseconds
STREAM copy bandwidth: 7802.45 MB/sec
STREAM scale latency: 2.08 nanoseconds
STREAM scale bandwidth: 7681.87 MB/sec
STREAM add latency: 2.19 nanoseconds
STREAM add bandwidth: 10954.76 MB/sec
STREAM triad latency: 3.17 nanoseconds
STREAM triad bandwidth: 7559.86 MB/sec

#numactl -C 7 -m 2 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 3.51 nanoseconds
STREAM copy bandwidth: 4556.86 MB/sec
STREAM scale latency: 3.58 nanoseconds
STREAM scale bandwidth: 4463.66 MB/sec
STREAM add latency: 2.71 nanoseconds
STREAM add bandwidth: 8869.79 MB/sec
STREAM triad latency: 5.92 nanoseconds
STREAM triad bandwidth: 4057.12 MB/sec

#numactl -C 7 -m 3 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 3.94 nanoseconds
STREAM copy bandwidth: 4064.25 MB/sec
STREAM scale latency: 3.82 nanoseconds
STREAM scale bandwidth: 4188.67 MB/sec
STREAM add latency: 2.86 nanoseconds
STREAM add bandwidth: 8390.70 MB/sec
STREAM triad latency: 4.78 nanoseconds
STREAM triad bandwidth: 5024.25 MB/sec

#numactl -C 24 -m 3 ./bin/stream  -W 5 -N 5 -M 64M
STREAM copy latency: 4.10 nanoseconds
STREAM copy bandwidth: 3904.63 MB/sec
STREAM scale latency: 4.03 nanoseconds
STREAM scale bandwidth: 3969.41 MB/sec
STREAM add latency: 3.07 nanoseconds
STREAM add bandwidth: 7816.08 MB/sec
STREAM triad latency: 5.06 nanoseconds
STREAM triad bandwidth: 4738.66 MB/sec
```

### 海光7280

可以看到跨numa（一个numa也就是一个socket，等同于跨socket）RT从1.5上升到2.5，这个数据比鲲鹏920要好很多

```
[root@hygon8 14:32 /root/lmbench-master]
#lscpu
架构：                           x86_64
CPU 运行模式：                   32-bit, 64-bit
字节序：                         Little Endian
Address sizes:                   43 bits physical, 48 bits virtual
CPU:                             128
在线 CPU 列表：                  0-127
每个核的线程数：                 2
每个座的核数：                   32
座：                             2
NUMA 节点：                      8
厂商 ID：                        HygonGenuine
CPU 系列：                       24
型号：                           1
型号名称：                       Hygon C86 7280 32-core Processor
步进：                           1
CPU MHz：                        2194.586
BogoMIPS：                       3999.63
虚拟化：                         AMD-V
L1d 缓存：                       2 MiB
L1i 缓存：                       4 MiB
L2 缓存：                        32 MiB
L3 缓存：                        128 MiB
NUMA 节点0 CPU：                 0-7,64-71
NUMA 节点1 CPU：                 8-15,72-79
NUMA 节点2 CPU：                 16-23,80-87
NUMA 节点3 CPU：                 24-31,88-95
NUMA 节点4 CPU：                 32-39,96-103
NUMA 节点5 CPU：                 40-47,104-111
NUMA 节点6 CPU：                 48-55,112-119
NUMA 节点7 CPU：                 56-63,120-127

//可以看到7号core比15、23、31号core明显要快，就近访问node 0的内存，跨numa node（跨Die）没有内存交织分配
[root@hygon8 14:32 /root/lmbench-master]
#time for i in $(seq 7 8 64); do echo $i; numactl -C $i -m 0 ./bin/stream -W 5 -N 5 -M 64M; done
7
STREAM copy latency: 1.38 nanoseconds    
STREAM copy bandwidth: 11559.53 MB/sec
STREAM scale latency: 1.16 nanoseconds
STREAM scale bandwidth: 13815.87 MB/sec
STREAM add latency: 1.40 nanoseconds
STREAM add bandwidth: 17145.85 MB/sec
STREAM triad latency: 1.44 nanoseconds
STREAM triad bandwidth: 16637.18 MB/sec
15
STREAM copy latency: 1.67 nanoseconds
STREAM copy bandwidth: 9591.77 MB/sec
STREAM scale latency: 1.56 nanoseconds
STREAM scale bandwidth: 10242.50 MB/sec
STREAM add latency: 1.45 nanoseconds
STREAM add bandwidth: 16581.00 MB/sec
STREAM triad latency: 2.00 nanoseconds
STREAM triad bandwidth: 12028.83 MB/sec
23
STREAM copy latency: 1.65 nanoseconds
STREAM copy bandwidth: 9701.49 MB/sec
STREAM scale latency: 1.53 nanoseconds
STREAM scale bandwidth: 10427.98 MB/sec
STREAM add latency: 1.42 nanoseconds
STREAM add bandwidth: 16846.10 MB/sec
STREAM triad latency: 1.97 nanoseconds
STREAM triad bandwidth: 12189.72 MB/sec
31
STREAM copy latency: 1.64 nanoseconds
STREAM copy bandwidth: 9742.86 MB/sec
STREAM scale latency: 1.52 nanoseconds
STREAM scale bandwidth: 10510.80 MB/sec
STREAM add latency: 1.45 nanoseconds
STREAM add bandwidth: 16559.86 MB/sec
STREAM triad latency: 1.92 nanoseconds
STREAM triad bandwidth: 12490.01 MB/sec
39
STREAM copy latency: 2.55 nanoseconds
STREAM copy bandwidth: 6286.25 MB/sec
STREAM scale latency: 2.51 nanoseconds
STREAM scale bandwidth: 6383.11 MB/sec
STREAM add latency: 1.76 nanoseconds
STREAM add bandwidth: 13660.83 MB/sec
STREAM triad latency: 3.68 nanoseconds
STREAM triad bandwidth: 6523.02 MB/sec
```

如果这种芯片在bios里设置Die interleaving，4块die当成一个numa node吐出来给OS

```
#lscpu
架构：                           x86_64
CPU 运行模式：                   32-bit, 64-bit
字节序：                         Little Endian
Address sizes:                   43 bits physical, 48 bits virtual
CPU:                             128
在线 CPU 列表：                  0-127
每个核的线程数：                 2
每个座的核数：                   32
座：                             2
NUMA 节点：                      2
厂商 ID：                        HygonGenuine
CPU 系列：                       24
型号：                           1
型号名称：                       Hygon C86 7280 32-core Processor
步进：                           1
CPU MHz：                        2108.234
BogoMIPS：                       3999.45
虚拟化：                         AMD-V
L1d 缓存：                       2 MiB
L1i 缓存：                       4 MiB
L2 缓存：                        32 MiB
L3 缓存：                        128 MiB
//注意这里和真实物理架构不一致，bios配置了Die Interleaving Enable
//表示每路内多个Die内存交织分配，这样整个一路就是一个大Die
NUMA 节点0 CPU：                 0-31,64-95  
NUMA 节点1 CPU：                 32-63,96-127


//enable die interleaving 后继续streaming测试
//最终测试结果表现就是7/15/23/31 core性能一致，因为默认一个numa内内存交织分配
//可以看到同一路下的四个die内存交织访问，所以4个node内存延时一样了（被平均），都不如就近快
[root@hygon3 16:09 /root/lmbench-master]
#time for i in $(seq 7 8 64); do echo $i; numactl -C $i -m 0 ./bin/stream -W 5 -N 5 -M 64M; done
7
STREAM copy latency: 1.48 nanoseconds
STREAM copy bandwidth: 10782.58 MB/sec
STREAM scale latency: 1.20 nanoseconds
STREAM scale bandwidth: 13364.38 MB/sec
STREAM add latency: 1.46 nanoseconds
STREAM add bandwidth: 16408.32 MB/sec
STREAM triad latency: 1.53 nanoseconds
STREAM triad bandwidth: 15696.00 MB/sec
15
STREAM copy latency: 1.51 nanoseconds
STREAM copy bandwidth: 10601.25 MB/sec
STREAM scale latency: 1.24 nanoseconds
STREAM scale bandwidth: 12855.87 MB/sec
STREAM add latency: 1.46 nanoseconds
STREAM add bandwidth: 16382.42 MB/sec
STREAM triad latency: 1.53 nanoseconds
STREAM triad bandwidth: 15691.48 MB/sec
23
STREAM copy latency: 1.50 nanoseconds
STREAM copy bandwidth: 10700.61 MB/sec
STREAM scale latency: 1.27 nanoseconds
STREAM scale bandwidth: 12634.63 MB/sec
STREAM add latency: 1.47 nanoseconds
STREAM add bandwidth: 16370.67 MB/sec
STREAM triad latency: 1.55 nanoseconds
STREAM triad bandwidth: 15455.75 MB/sec
31
STREAM copy latency: 1.50 nanoseconds
STREAM copy bandwidth: 10637.39 MB/sec
STREAM scale latency: 1.25 nanoseconds
STREAM scale bandwidth: 12778.99 MB/sec
STREAM add latency: 1.46 nanoseconds
STREAM add bandwidth: 16420.65 MB/sec
STREAM triad latency: 1.61 nanoseconds
STREAM triad bandwidth: 14946.80 MB/sec
39
STREAM copy latency: 2.35 nanoseconds
STREAM copy bandwidth: 6807.09 MB/sec
STREAM scale latency: 2.32 nanoseconds
STREAM scale bandwidth: 6906.93 MB/sec
STREAM add latency: 1.63 nanoseconds
STREAM add bandwidth: 14729.23 MB/sec
STREAM triad latency: 3.36 nanoseconds
STREAM triad bandwidth: 7151.67 MB/sec
47
STREAM copy latency: 2.31 nanoseconds
STREAM copy bandwidth: 6938.47 MB/sec
```

[以华为泰山服务器(鲲鹏920芯片)配置为例](https://support.huawei.com/enterprise/zh/doc/EDOC1100088653/32aa8773)：![image-20211228165542167](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/cd5d6e55a71341da-image-20211228165542167.png)

> Die Interleaving 控制是否使能DIE交织。使能DIE交织能充分利用系统的DDR带宽，并尽量保证各DDR通道的带宽均衡，提升DDR的利用率


### hygon5280测试数据

```
-----hygon5280测试数据
[root@localhost lmbench-master]# for i in $(seq 0 8 24); do echo $i; numactl -C $i -m 0 ./bin/stream -W 5 -N 5 -M 64M; done
0
STREAM copy latency: 1.22 nanoseconds
STREAM copy bandwidth: 13166.34 MB/sec
STREAM scale latency: 1.13 nanoseconds
STREAM scale bandwidth: 14166.95 MB/sec
STREAM add latency: 1.15 nanoseconds
STREAM add bandwidth: 20818.63 MB/sec
STREAM triad latency: 1.39 nanoseconds
STREAM triad bandwidth: 17211.81 MB/sec
8
STREAM copy latency: 1.56 nanoseconds
STREAM copy bandwidth: 10273.07 MB/sec
STREAM scale latency: 1.50 nanoseconds
STREAM scale bandwidth: 10701.89 MB/sec
STREAM add latency: 1.20 nanoseconds
STREAM add bandwidth: 19996.68 MB/sec
STREAM triad latency: 1.93 nanoseconds
STREAM triad bandwidth: 12443.70 MB/sec
16
STREAM copy latency: 2.52 nanoseconds
STREAM copy bandwidth: 6357.71 MB/sec
STREAM scale latency: 2.48 nanoseconds
STREAM scale bandwidth: 6454.95 MB/sec
STREAM add latency: 1.67 nanoseconds
STREAM add bandwidth: 14362.51 MB/sec
STREAM triad latency: 3.65 nanoseconds
STREAM triad bandwidth: 6572.85 MB/sec
24
STREAM copy latency: 2.44 nanoseconds
STREAM copy bandwidth: 6554.24 MB/sec
STREAM scale latency: 2.41 nanoseconds
STREAM scale bandwidth: 6642.80 MB/sec
STREAM add latency: 1.44 nanoseconds
STREAM add bandwidth: 16695.82 MB/sec
STREAM triad latency: 3.61 nanoseconds
STREAM triad bandwidth: 6639.18 MB/sec
[root@localhost lmbench-master]# lscpu
架构：                           x86_64
CPU 运行模式：                   32-bit, 64-bit
字节序：                         Little Endian
Address sizes:                   43 bits physical, 48 bits virtual
CPU:                             64
在线 CPU 列表：                  0-63
每个核的线程数：                 2
每个座的核数：                   16
座：                             2
NUMA 节点：                      4
厂商 ID：                        HygonGenuine
CPU 系列：                       24
型号：                           1
型号名称：                       Hygon C86 5280 16-core Processor
步进：                           1
Frequency boost:                 enabled
CPU MHz：                        2799.311
CPU 最大 MHz：                   2500.0000
CPU 最小 MHz：                   1600.0000
BogoMIPS：                       4999.36
虚拟化：                         AMD-V
L1d 缓存：                       1 MiB
L1i 缓存：                       2 MiB
L2 缓存：                        16 MiB
L3 缓存：                        64 MiB
NUMA 节点0 CPU：                 0-7,32-39
NUMA 节点1 CPU：                 8-15,40-47
NUMA 节点2 CPU：                 16-23,48-55
NUMA 节点3 CPU：                 24-31,56-63
Vulnerability Itlb multihit:     Not affected
Vulnerability L1tf:              Not affected
Vulnerability Mds:               Not affected
Vulnerability Meltdown:          Not affected
Vulnerability Spec store bypass: Mitigation; Speculative Store Bypass disabled via prctl and seccomp
Vulnerability Spectre v1:        Mitigation; usercopy/swapgs barriers and __user pointer sanitization
Vulnerability Spectre v2:        Mitigation; Full AMD retpoline, IBPB conditional, STIBP disabled, RSB
                                 filling
Vulnerability Srbds:             Not affected
Vulnerability Tsx async abort:   Not affected
标记：                           fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse3
                                 6 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdts
                                 cp lm constant_tsc rep_good nopl nonstop_tsc cpuid extd_apicid amd_dcm
                                  aperfmperf pni pclmulqdq monitor ssse3 fma cx16 sse4_1 sse4_2 movbe p
                                 opcnt xsave avx f16c rdrand lahf_lm cmp_legacy svm extapic cr8_legacy
                                 abm sse4a misalignsse 3dnowprefetch osvw skinit wdt tce topoext perfct
                                 r_core perfctr_nb bpext perfctr_llc mwaitx cpb hw_pstate sme ssbd sev
                                 ibpb vmmcall fsgsbase bmi1 avx2 smep bmi2 rdseed adx smap clflushopt s
                                 ha_ni xsaveopt xsavec xgetbv1 xsaves clzero irperf xsaveerptr arat npt
                                  lbrv svm_lock nrip_save tsc_scale vmcb_clean flushbyasid decodeassist
                                 s pausefilter pfthreshold avic v_vmsave_vmload vgif overflow_recov suc
                                 cor smca
```

### intel 8269CY

```
lscpu
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

[root@numaopen.cloud.et93 /home/ren/lmbench3]
#time for i in $(seq 0 8 51); do echo $i; numactl -C $i -m 0 ./bin/stream -W 5 -N 5 -M 64M; done
0
STREAM copy latency: 1.15 nanoseconds
STREAM copy bandwidth: 13941.80 MB/sec
STREAM scale latency: 1.16 nanoseconds
STREAM scale bandwidth: 13799.89 MB/sec
STREAM add latency: 1.31 nanoseconds
STREAM add bandwidth: 18318.23 MB/sec
STREAM triad latency: 1.56 nanoseconds
STREAM triad bandwidth: 15356.72 MB/sec
16
STREAM copy latency: 1.12 nanoseconds
STREAM copy bandwidth: 14293.68 MB/sec
STREAM scale latency: 1.13 nanoseconds
STREAM scale bandwidth: 14162.47 MB/sec
STREAM add latency: 1.31 nanoseconds
STREAM add bandwidth: 18293.27 MB/sec
STREAM triad latency: 1.53 nanoseconds
STREAM triad bandwidth: 15692.47 MB/sec
32
STREAM copy latency: 1.52 nanoseconds
STREAM copy bandwidth: 10551.71 MB/sec
STREAM scale latency: 1.52 nanoseconds
STREAM scale bandwidth: 10508.33 MB/sec
STREAM add latency: 1.38 nanoseconds
STREAM add bandwidth: 17363.22 MB/sec
STREAM triad latency: 2.00 nanoseconds
STREAM triad bandwidth: 12024.52 MB/sec
40
STREAM copy latency: 1.49 nanoseconds
STREAM copy bandwidth: 10758.50 MB/sec
STREAM scale latency: 1.50 nanoseconds
STREAM scale bandwidth: 10680.17 MB/sec
STREAM add latency: 1.34 nanoseconds
STREAM add bandwidth: 17948.34 MB/sec
STREAM triad latency: 1.98 nanoseconds
STREAM triad bandwidth: 12133.22 MB/sec
48
STREAM copy latency: 1.49 nanoseconds
STREAM copy bandwidth: 10736.56 MB/sec
STREAM scale latency: 1.50 nanoseconds
STREAM scale bandwidth: 10692.93 MB/sec
STREAM add latency: 1.34 nanoseconds
STREAM add bandwidth: 17902.85 MB/sec
STREAM triad latency: 1.96 nanoseconds
STREAM triad bandwidth: 12239.44 MB/sec
```

### Intel(R) Xeon(R) CPU E5-2682 v4

```
#time for i in $(seq 0 8 51); do echo $i; numactl -C $i -m 0 ./bin/stream -W 5 -N 5 -M 64M; done
0
STREAM copy latency: 1.59 nanoseconds
STREAM copy bandwidth: 10092.31 MB/sec
STREAM scale latency: 1.57 nanoseconds
STREAM scale bandwidth: 10169.16 MB/sec
STREAM add latency: 1.31 nanoseconds
STREAM add bandwidth: 18360.83 MB/sec
STREAM triad latency: 2.28 nanoseconds
STREAM triad bandwidth: 10503.81 MB/sec
8
STREAM copy latency: 1.55 nanoseconds
STREAM copy bandwidth: 10312.14 MB/sec
STREAM scale latency: 1.56 nanoseconds
STREAM scale bandwidth: 10283.70 MB/sec
STREAM add latency: 1.30 nanoseconds
STREAM add bandwidth: 18416.26 MB/sec
STREAM triad latency: 2.23 nanoseconds
STREAM triad bandwidth: 10777.08 MB/sec
16
STREAM copy latency: 2.02 nanoseconds
STREAM copy bandwidth: 7914.25 MB/sec
STREAM scale latency: 2.02 nanoseconds
STREAM scale bandwidth: 7919.85 MB/sec
STREAM add latency: 1.39 nanoseconds
STREAM add bandwidth: 17276.06 MB/sec
STREAM triad latency: 2.92 nanoseconds
STREAM triad bandwidth: 8231.18 MB/sec
24
STREAM copy latency: 1.99 nanoseconds
STREAM copy bandwidth: 8032.18 MB/sec
STREAM scale latency: 1.98 nanoseconds
STREAM scale bandwidth: 8061.12 MB/sec
STREAM add latency: 1.39 nanoseconds
STREAM add bandwidth: 17313.94 MB/sec
STREAM triad latency: 2.88 nanoseconds
STREAM triad bandwidth: 8318.93 MB/sec

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
```

### AMD EPYC 7T83

```
#time for i in $(seq 0 8 255); do echo $i; numactl -C $i -m 0 ./bin/stream -W 5 -N 5 -M 64M; done
0
STREAM copy latency: 0.49 nanoseconds
STREAM copy bandwidth: 32561.30 MB/sec
STREAM scale latency: 0.49 nanoseconds
STREAM scale bandwidth: 32620.66 MB/sec
STREAM add latency: 0.87 nanoseconds
STREAM add bandwidth: 27575.20 MB/sec
STREAM triad latency: 0.70 nanoseconds
STREAM triad bandwidth: 34397.15 MB/sec
8
STREAM copy latency: 0.52 nanoseconds
STREAM copy bandwidth: 30764.47 MB/sec
STREAM scale latency: 0.53 nanoseconds
STREAM scale bandwidth: 30056.59 MB/sec
STREAM add latency: 0.87 nanoseconds
STREAM add bandwidth: 27575.20 MB/sec
STREAM triad latency: 0.69 nanoseconds
STREAM triad bandwidth: 34789.45 MB/sec
16
STREAM copy latency: 0.53 nanoseconds
STREAM copy bandwidth: 30173.15 MB/sec
STREAM scale latency: 0.54 nanoseconds
STREAM scale bandwidth: 29895.91 MB/sec
STREAM add latency: 0.87 nanoseconds
STREAM add bandwidth: 27496.11 MB/sec
STREAM triad latency: 0.70 nanoseconds
STREAM triad bandwidth: 34128.93 MB/sec
24
STREAM copy latency: 0.78 nanoseconds
STREAM copy bandwidth: 20417.69 MB/sec
STREAM scale latency: 0.51 nanoseconds
STREAM scale bandwidth: 31354.70 MB/sec
STREAM add latency: 0.87 nanoseconds
STREAM add bandwidth: 27548.79 MB/sec
STREAM triad latency: 0.69 nanoseconds
STREAM triad bandwidth: 34589.22 MB/sec
32
STREAM copy latency: 0.60 nanoseconds
STREAM copy bandwidth: 26862.34 MB/sec
STREAM scale latency: 0.58 nanoseconds
STREAM scale bandwidth: 27376.00 MB/sec
STREAM add latency: 0.87 nanoseconds
STREAM add bandwidth: 27518.66 MB/sec
STREAM triad latency: 0.78 nanoseconds
STREAM triad bandwidth: 30779.17 MB/sec
40
STREAM copy latency: 0.59 nanoseconds
STREAM copy bandwidth: 27230.21 MB/sec
STREAM scale latency: 0.59 nanoseconds
STREAM scale bandwidth: 27284.18 MB/sec
STREAM add latency: 0.87 nanoseconds
STREAM add bandwidth: 27503.63 MB/sec
STREAM triad latency: 0.77 nanoseconds
STREAM triad bandwidth: 31242.48 MB/sec
48
STREAM copy latency: 0.59 nanoseconds
STREAM copy bandwidth: 27102.37 MB/sec
STREAM scale latency: 0.59 nanoseconds
STREAM scale bandwidth: 27164.08 MB/sec
STREAM add latency: 0.87 nanoseconds
STREAM add bandwidth: 27503.63 MB/sec
STREAM triad latency: 0.76 nanoseconds
STREAM triad bandwidth: 31422.90 MB/sec
56
STREAM copy latency: 0.92 nanoseconds
STREAM copy bandwidth: 17453.54 MB/sec
STREAM scale latency: 0.59 nanoseconds
STREAM scale bandwidth: 27267.55 MB/sec
STREAM add latency: 0.87 nanoseconds
STREAM add bandwidth: 27488.61 MB/sec
STREAM triad latency: 0.77 nanoseconds
STREAM triad bandwidth: 31169.92 MB/sec
64
STREAM copy latency: 0.88 nanoseconds
STREAM copy bandwidth: 18231.15 MB/sec
STREAM scale latency: 0.84 nanoseconds
STREAM scale bandwidth: 18976.06 MB/sec
STREAM add latency: 0.91 nanoseconds
STREAM add bandwidth: 26413.87 MB/sec
STREAM triad latency: 1.08 nanoseconds
STREAM triad bandwidth: 22310.12 MB/sec
72
STREAM copy latency: 0.86 nanoseconds
STREAM copy bandwidth: 18552.45 MB/sec
STREAM scale latency: 0.84 nanoseconds
STREAM scale bandwidth: 19113.88 MB/sec
STREAM add latency: 0.91 nanoseconds
STREAM add bandwidth: 26375.81 MB/sec
STREAM triad latency: 1.08 nanoseconds
STREAM triad bandwidth: 22151.79 MB/sec
80
STREAM copy latency: 0.89 nanoseconds
STREAM copy bandwidth: 18037.59 MB/sec
STREAM scale latency: 0.87 nanoseconds
STREAM scale bandwidth: 18398.59 MB/sec
STREAM add latency: 0.92 nanoseconds
STREAM add bandwidth: 26142.91 MB/sec
STREAM triad latency: 1.08 nanoseconds
STREAM triad bandwidth: 22133.53 MB/sec
88
STREAM copy latency: 0.93 nanoseconds
STREAM copy bandwidth: 17119.60 MB/sec
STREAM scale latency: 0.94 nanoseconds
STREAM scale bandwidth: 17030.54 MB/sec
STREAM add latency: 0.92 nanoseconds
STREAM add bandwidth: 26146.30 MB/sec
STREAM triad latency: 1.08 nanoseconds
STREAM triad bandwidth: 22159.10 MB/sec
96
STREAM copy latency: 1.39 nanoseconds
STREAM copy bandwidth: 11512.93 MB/sec
STREAM scale latency: 0.87 nanoseconds
STREAM scale bandwidth: 18406.16 MB/sec
STREAM add latency: 0.92 nanoseconds
STREAM add bandwidth: 25991.03 MB/sec
STREAM triad latency: 1.09 nanoseconds
STREAM triad bandwidth: 22078.91 MB/sec
104
STREAM copy latency: 0.86 nanoseconds
STREAM copy bandwidth: 18546.04 MB/sec
STREAM scale latency: 1.39 nanoseconds
STREAM scale bandwidth: 11518.85 MB/sec
STREAM add latency: 0.91 nanoseconds
STREAM add bandwidth: 26300.01 MB/sec
STREAM triad latency: 1.06 nanoseconds
STREAM triad bandwidth: 22599.38 MB/sec
112
STREAM copy latency: 0.88 nanoseconds
STREAM copy bandwidth: 18253.46 MB/sec
STREAM scale latency: 0.85 nanoseconds
STREAM scale bandwidth: 18758.59 MB/sec
STREAM add latency: 0.91 nanoseconds
STREAM add bandwidth: 26413.87 MB/sec
STREAM triad latency: 1.06 nanoseconds
STREAM triad bandwidth: 22648.95 MB/sec
120
STREAM copy latency: 0.86 nanoseconds
STREAM copy bandwidth: 18607.75 MB/sec
STREAM scale latency: 0.84 nanoseconds
STREAM scale bandwidth: 18957.30 MB/sec
STREAM add latency: 0.91 nanoseconds
STREAM add bandwidth: 26427.74 MB/sec
STREAM triad latency: 1.08 nanoseconds
STREAM triad bandwidth: 22313.83 MB/sec
128
STREAM copy latency: 0.82 nanoseconds
STREAM copy bandwidth: 19432.13 MB/sec
STREAM scale latency: 0.87 nanoseconds
STREAM scale bandwidth: 18421.31 MB/sec
STREAM add latency: 0.98 nanoseconds
STREAM add bandwidth: 24546.03 MB/sec
STREAM triad latency: 1.06 nanoseconds
STREAM triad bandwidth: 22702.59 MB/sec
136
STREAM copy latency: 0.74 nanoseconds
STREAM copy bandwidth: 21568.01 MB/sec
STREAM scale latency: 0.74 nanoseconds
STREAM scale bandwidth: 21668.99 MB/sec
STREAM add latency: 0.90 nanoseconds
STREAM add bandwidth: 26697.59 MB/sec
STREAM triad latency: 0.91 nanoseconds
STREAM triad bandwidth: 26320.64 MB/sec
144
STREAM copy latency: 0.79 nanoseconds
STREAM copy bandwidth: 20268.45 MB/sec
STREAM scale latency: 0.66 nanoseconds
STREAM scale bandwidth: 24279.61 MB/sec
STREAM add latency: 0.89 nanoseconds
STREAM add bandwidth: 26822.08 MB/sec
STREAM triad latency: 0.84 nanoseconds
STREAM triad bandwidth: 28540.76 MB/sec
152
STREAM copy latency: 0.85 nanoseconds
STREAM copy bandwidth: 18903.90 MB/sec
STREAM scale latency: 0.56 nanoseconds
STREAM scale bandwidth: 28734.25 MB/sec
STREAM add latency: 0.88 nanoseconds
STREAM add bandwidth: 27335.58 MB/sec
STREAM triad latency: 0.75 nanoseconds
STREAM triad bandwidth: 31911.01 MB/sec
160
STREAM copy latency: 0.64 nanoseconds
STREAM copy bandwidth: 25068.68 MB/sec
STREAM scale latency: 0.63 nanoseconds
STREAM scale bandwidth: 25550.68 MB/sec
STREAM add latency: 0.88 nanoseconds
STREAM add bandwidth: 27313.33 MB/sec
STREAM triad latency: 0.82 nanoseconds
STREAM triad bandwidth: 29416.50 MB/sec
168
STREAM copy latency: 0.61 nanoseconds
STREAM copy bandwidth: 26232.33 MB/sec
STREAM scale latency: 0.60 nanoseconds
STREAM scale bandwidth: 26717.96 MB/sec
STREAM add latency: 0.88 nanoseconds
STREAM add bandwidth: 27398.82 MB/sec
STREAM triad latency: 0.79 nanoseconds
STREAM triad bandwidth: 30411.86 MB/sec
176
STREAM copy latency: 0.58 nanoseconds
STREAM copy bandwidth: 27380.19 MB/sec
STREAM scale latency: 0.58 nanoseconds
STREAM scale bandwidth: 27740.96 MB/sec
STREAM add latency: 0.94 nanoseconds
STREAM add bandwidth: 25666.31 MB/sec
STREAM triad latency: 0.77 nanoseconds
STREAM triad bandwidth: 31150.63 MB/sec
184
STREAM copy latency: 0.90 nanoseconds
STREAM copy bandwidth: 17730.21 MB/sec
STREAM scale latency: 0.57 nanoseconds
STREAM scale bandwidth: 27918.40 MB/sec
STREAM add latency: 0.87 nanoseconds
STREAM add bandwidth: 27458.61 MB/sec
STREAM triad latency: 0.76 nanoseconds
STREAM triad bandwidth: 31457.27 MB/sec
192
STREAM copy latency: 0.91 nanoseconds
STREAM copy bandwidth: 17558.57 MB/sec
STREAM scale latency: 0.88 nanoseconds
STREAM scale bandwidth: 18115.49 MB/sec
STREAM add latency: 0.92 nanoseconds
STREAM add bandwidth: 26031.36 MB/sec
STREAM triad latency: 1.12 nanoseconds
STREAM triad bandwidth: 21443.95 MB/sec
200
STREAM copy latency: 1.34 nanoseconds
STREAM copy bandwidth: 11911.40 MB/sec
STREAM scale latency: 0.85 nanoseconds
STREAM scale bandwidth: 18893.26 MB/sec
STREAM add latency: 0.91 nanoseconds
STREAM add bandwidth: 26306.88 MB/sec
STREAM triad latency: 1.09 nanoseconds
STREAM triad bandwidth: 22013.73 MB/sec
208
STREAM copy latency: 1.36 nanoseconds
STREAM copy bandwidth: 11724.12 MB/sec
STREAM scale latency: 0.86 nanoseconds
STREAM scale bandwidth: 18631.00 MB/sec
STREAM add latency: 0.92 nanoseconds
STREAM add bandwidth: 26166.69 MB/sec
STREAM triad latency: 1.10 nanoseconds
STREAM triad bandwidth: 21763.86 MB/sec
216
STREAM copy latency: 0.88 nanoseconds
STREAM copy bandwidth: 18270.85 MB/sec
STREAM scale latency: 0.85 nanoseconds
STREAM scale bandwidth: 18848.15 MB/sec
STREAM add latency: 0.92 nanoseconds
STREAM add bandwidth: 26176.90 MB/sec
STREAM triad latency: 1.10 nanoseconds
STREAM triad bandwidth: 21799.20 MB/sec
224
STREAM copy latency: 0.89 nanoseconds
STREAM copy bandwidth: 18047.29 MB/sec
STREAM scale latency: 0.86 nanoseconds
STREAM scale bandwidth: 18677.66 MB/sec
STREAM add latency: 0.92 nanoseconds
STREAM add bandwidth: 26112.39 MB/sec
STREAM triad latency: 1.09 nanoseconds
STREAM triad bandwidth: 21966.89 MB/sec
232
STREAM copy latency: 1.35 nanoseconds
STREAM copy bandwidth: 11818.58 MB/sec
STREAM scale latency: 0.82 nanoseconds
STREAM scale bandwidth: 19568.11 MB/sec
STREAM add latency: 0.91 nanoseconds
STREAM add bandwidth: 26469.44 MB/sec
STREAM triad latency: 1.06 nanoseconds
STREAM triad bandwidth: 22702.59 MB/sec
240
STREAM copy latency: 0.87 nanoseconds
STREAM copy bandwidth: 18325.74 MB/sec
STREAM scale latency: 0.83 nanoseconds
STREAM scale bandwidth: 19331.37 MB/sec
STREAM add latency: 0.91 nanoseconds
STREAM add bandwidth: 26455.52 MB/sec
STREAM triad latency: 1.06 nanoseconds
STREAM triad bandwidth: 22580.37 MB/sec
248
STREAM copy latency: 0.87 nanoseconds
STREAM copy bandwidth: 18418.79 MB/sec
STREAM scale latency: 0.84 nanoseconds
STREAM scale bandwidth: 19019.09 MB/sec
STREAM add latency: 0.91 nanoseconds
STREAM add bandwidth: 26483.37 MB/sec
STREAM triad latency: 1.08 nanoseconds
STREAM triad bandwidth: 22148.13 MB/sec
```

### stream对比数据

总结下几个CPU用stream测试访问内存的RT以及抖动和带宽对比数据，重点关注带宽，这个测试中时延不重要

<table>
<tr class="header">
<th></th>
<th>最小RT</th>
<th>最大RT</th>
<th>最大copy bandwidth</th>
<th>最小copy bandwidth</th>
</tr>
<tr class="odd">
<td>申威3231(2numa node)</td>
<td>7.09</td>
<td>8.75</td>
<td>2256.59 MB/sec</td>
<td>1827.88 MB/sec</td>
</tr>
<tr class="even">
<td>飞腾2500(16 numa node)</td>
<td>2.84</td>
<td>10.34</td>
<td>5638.21 MB/sec</td>
<td>1546.68 MB/sec</td>
</tr>
<tr class="odd">
<td>鲲鹏920(4 numa node)</td>
<td>1.84</td>
<td>3.87</td>
<td>8700.75 MB/sec</td>
<td>4131.81 MB/sec</td>
</tr>
<tr class="even">
<td>海光7280(8 numa node)</td>
<td>1.38</td>
<td>2.58</td>
<td>11591.48 MB/sec</td>
<td>6206.99 MB/sec</td>
</tr>
<tr class="odd">
<td>海光5280(4 numa node)</td>
<td>1.22</td>
<td>2.52</td>
<td>13166.34 MB/sec</td>
<td>6357.71 MB/sec</td>
</tr>
<tr class="even">
<td>Intel8269CY(2 numa node)</td>
<td>1.12</td>
<td>1.52</td>
<td>14293.68 MB/sec</td>
<td>10551.71 MB/sec</td>
</tr>
<tr class="odd">
<td>Intel E5-2682(2 numa node)</td>
<td>1.58</td>
<td>2.02</td>
<td>10092.31 MB/sec</td>
<td>7914.25 MB/sec</td>
</tr>
<tr class="even">
<td>AMD EPYC 7T83(4 numa node)</td>
<td>0.49</td>
<td>1.39</td>
<td>32561.30 MB/sec</td>
<td>11512.93 MB/sec</td>
</tr>
<tr class="odd">
<td>Y</td>
<td>1.83</td>
<td>3.48</td>
<td>8764.72 MB/sec</td>
<td>4593.25 MB/sec</td>
</tr>
</table>

从以上数据可以看出这5款CPU性能一款比一款好，飞腾2500慢的core上延时快到intel 8269的10倍了，平均延时5倍以上了。延时数据基本和单核上测试sysbench TPS一致。性能差不多就是：常数*主频/RT

### lat_mem_rd对比数据

用不同的node上的core 跑lat_mem_rd测试访问node0内存的RT，只取最大64M的时延，时延和node距离完全一致

<table>
<tr class="header">
<th></th>
<th>RT变化</th>
</tr>
<tr class="odd">
<td>飞腾2500(16 numa node)</td>
<td>core:0 149.976<br/>core:8 168.805<br/>core:16 191.415<br/>core:24 178.283<br/>core:32 170.814<br/>core:40 185.699<br/>core:48 212.281<br/>core:56 202.479<br/>core:64 426.176<br/>core:72 444.367<br/>core:80 465.894<br/>core:88 452.245<br/>core:96 448.352<br/>core:104 460.603<br/>core:112 485.989<br/>core:120 490.402</td>
</tr>
<tr class="even">
<td>鲲鹏920(4 numa node)</td>
<td>core:0 117.323<br/>core:24 135.337<br/>core:48 197.782<br/>core:72 219.416</td>
</tr>
<tr class="odd">
<td>海光7280(8 numa node)</td>
<td>numa0 106.839<br/>numa1 168.583<br/>numa2 163.925<br/>numa3 163.690<br/>numa4 289.628<br/>numa5 288.632<br/>numa6 236.615<br/>numa7 291.880<br/>分割行<br/>enabled die interleaving <br/>core:0 153.005<br/>core:16 152.458<br/>core:32 272.057<br/>core:48 269.441</td>
</tr>
<tr class="even">
<td>海光5280(4 numa node)</td>
<td>core:0 102.574<br/>core:8 160.989<br/>core:16 286.850<br/>core:24 231.197</td>
</tr>
<tr class="odd">
<td>海光7260(1 numa node)</td>
<td>core:0 265</td>
</tr>
<tr class="even">
<td>Intel 8269CY(2 numa node)</td>
<td>core:0 69.792<br/>core:26 93.107</td>
</tr>
<tr class="odd">
<td>申威3231(2numa node)</td>
<td>core:0 215.146<br/>core:32 282.443</td>
</tr>
<tr class="even">
<td>AMD EPYC 7T83(4 numa node)</td>
<td>core:0 71.656<br/>core:32 80.129<br/>core:64 131.334<br/>core:96 129.563</td>
</tr>
<tr class="odd">
<td>Y7（2Die，2node，1socket）</td>
<td>core:8 42.395<br/>core:40 36.434<br/>core:104 105.745<br/>core:88 124.384<br/><br/>core:24 62.979<br/>core:8 69.324<br/>core:64 137.233<br/>core:88 127.250<br/><br/>133ns 205ns （待测）</td>
</tr>
</table>

测试命令：

```
for i in $(seq 0 8 127); do echo core:$i; numactl -C $i -m 0 ./bin/lat_mem_rd -W 5 -N 5 -t 64M; done >lat.log 2>&1
```

测试结果和numactl -H 看到的node distance完全一致，芯片厂家应该就是这样测试然后把这个延迟当做距离写进去了

AMD EPYC 7T83(4 numa node)的时延相对抖动有点大，这和架构多个小Die合并成一块CPU有关

```
#grep -E "core|64.00000" lat.log
core:0
64.00000 71.656
core:32
64.00000 80.129
core:64
64.00000 131.334
core:88
64.00000 136.774
core:96
64.00000 129.563
core:120
64.00000 140.151
```

AMD EPYC 7T83(4 numa node)比Intel 8269时延要大，但是带宽也高很多

### 龙芯测试数据

3A5000为龙芯，执行的命令为./lat_mem_rd 128M 4096，其中 4096 参数为跳步大小。其基本原理是，通过按 给定间隔去循环读一定大小的内存区域，测量每个读平均的时间。如果区域大小小于 L1 Cache 大 小，时间应该接近 L1 的访问延迟;如果大于 L1 小于 L2，则接近 L2 访问延迟;依此类推。图中横坐 标为访问的字节数，纵坐标为访存的拍数(cycles)。

![image-20220221113929547](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/7e8e69f06a276487-image-20220221113929547.png)

基于跳步访问的 3A5000 和 Zen1、Skylake 各级延迟的比较(cycles)

![image-20220221112527936](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/8a884461704cbf65-image-20220221112527936.png)

下图给出了 LMbench 测试得到的访存操作的并发性，执行的命令为./par_mem。访存操作的并 发性是各级 Cache 和内存所支持并发访问的能力。在 LMbench 中，访存操作并发性的测试是设计一 个链表，不断地遍历访问下一个链表中的元素，链表所跳的距离和需要测量的 Cache 容量相关，在 一段时间能并发的发起对链表的追逐操作，也就是同时很多链表在遍历，如果发现这一段时间内 能同时完成 N 个链表的追逐操作，就认为访存的并发操作是 N。

![image-20220221112727377](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/c0e2eb0683c856e8-image-20220221112727377.png)

下图列出了三款处理器的功能部件操作延迟数据，使用的命令是./lat_ops。

![image-20220221112853404](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/d6676e64035af58c-image-20220221112853404.png)

#### 龙芯stream数据

LMbench 包含了 STREAM 带宽测试工具，可以用来测试可持续的内存访问带宽情况。图表12.25列 出了三款处理器的 STREAM 带宽数据，其中 STREAM 数组大小设置为 1 亿个元素，采用 OpenMP 版本 同时运行四个线程来测试满载带宽;相应测试平台均为 CPU 的两个内存控制器各接一根内存条， 3A5000 和 Zen1 用 DDR4 3200 内存条，Skylake 用 DDR4 2400 内存条(它最高只支持这个规格)。

![image-20220221113037332](https://gitee.com/plantegg/shareImage/raw/_md2zhihu_blog_cee8f3b4/不同CPU性能大PK/236cdabe5cf78760-image-20220221113037332.png)

从数据可以看到，虽然硬件上 3A5000 和 Zen1 都实现了 DDR4 3200，但 3A5000 的实测可持续带宽 还是有一定差距。用户程序看到的内存带宽不仅仅和内存的物理频率有关系，也和处理器内部的 各种访存队列、内存控制器的调度策略、预取器和内存时序参数设置等相关，需要进行更多分析 来定位具体的瓶颈点。像 STREAM 这样的软件测试工具，能够更好地反映某个子系统的综合能力， 因而被广泛采用。

## 对比结论

-   AMD单核跑分数据比较好
-   MySQL 查询场景下Intel的性能好很多
-   xdb比社区版性能要好
-   MySQL8.0比5.7在多核锁竞争场景下性能要好
-   intel最好，AMD接近Intel，海光差的比较远但是又比鲲鹏好很多，飞腾最差，尤其是跨socket简直是灾难
-   麒麟OS性能也比CentOS略差一些
-   从perf指标来看 鲲鹏920的L1d命中率高于8163是因为鲲鹏L1 size大；L2命中率低于8163，同样是因为鲲鹏 L2 size小；同样L1i 鲲鹏也大于8163，但是实际跑起来L1i Miss Rate更高，这说明 ARM对 L1d 使用效率低

整体来说AMD用领先了一代的工艺（7nm VS 14nm)，在MySQL查询场景中终于可以接近Intel了，但是海光、鲲鹏、飞腾还是不给力。

## 附表

鲲鹏920 和 8163 在 MySQL 场景下的 perf 指标对比

<table>
<tr class="header">
<th>整体对比</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr class="odd">
<td>指标</td>
<td>X86</td>
<td>ARM</td>
<td>增加幅度</td>
</tr>
<tr class="even">
<td>IPC</td>
<td>0.4979</td>
<td>0.495</td>
<td>-0.6%</td>
</tr>
<tr class="odd">
<td>Branchs</td>
<td>237606414772</td>
<td>415979894985</td>
<td>75.1%</td>
</tr>
<tr class="even">
<td>Branch-misses</td>
<td>8104247620</td>
<td>28983836845</td>
<td>257.6%</td>
</tr>
<tr class="odd">
<td>Branch-missed rate</td>
<td>0.034</td>
<td>0.070</td>
<td>104.3%</td>
</tr>
<tr class="even">
<td>内存读带宽（GB/S)</td>
<td>25.0</td>
<td>25.0</td>
<td>-0.2%</td>
</tr>
<tr class="odd">
<td>内存写带宽（GB/S)</td>
<td>24.6</td>
<td>67.8</td>
<td>175.5%</td>
</tr>
<tr class="even">
<td>内存读写带宽（GB/S)</td>
<td>49.7</td>
<td>92.8</td>
<td>86.8%</td>
</tr>
<tr class="odd">
<td>UNALIGNED_ACCESS</td>
<td>1329146645</td>
<td>13686011901</td>
<td>929.7%</td>
</tr>
<tr class="even">
<td>L1d_MISS_RATIO</td>
<td>0.06055</td>
<td>0.04281</td>
<td>-29.3%</td>
</tr>
<tr class="odd">
<td>L1d_MISS_RATE</td>
<td>0.01645</td>
<td>0.01711</td>
<td>4.0%</td>
</tr>
<tr class="even">
<td>L2_MISS_RATIO</td>
<td>0.34824</td>
<td>0.47162</td>
<td>35.4%</td>
</tr>
<tr class="odd">
<td>L2_MISS_RATE</td>
<td>0.00577</td>
<td>0.03493</td>
<td>504.8%</td>
</tr>
<tr class="even">
<td>L1_ITLB_MISS_RATE</td>
<td>0.0028</td>
<td>0.005</td>
<td>78.6%</td>
</tr>
<tr class="odd">
<td>L1_DTLB_MISS_RATE</td>
<td>0.0025</td>
<td>0.0102</td>
<td>308.0%</td>
</tr>
<tr class="even">
<td>context-switchs</td>
<td>8407195</td>
<td>11614981</td>
<td>38.2%</td>
</tr>
<tr class="odd">
<td>Pagefault</td>
<td>228371</td>
<td>741189</td>
<td>224.6%</td>
</tr>
</table>

## 参考资料

[CPU的制造和概念](/2021/06/01/CPU%E7%9A%84%E5%88%B6%E9%80%A0%E5%92%8C%E6%A6%82%E5%BF%B5/)

[CPU 性能和Cache Line](/2021/05/16/CPU Cache Line 和性能/)

[Perf IPC以及CPU性能](/2021/05/16/Perf IPC以及CPU利用率/)

[Intel、海光、鲲鹏920、飞腾2500 CPU性能对比](/2021/06/18/%E5%87%A0%E6%AC%BECPU%E6%80%A7%E8%83%BD%E5%AF%B9%E6%AF%94/)

[飞腾ARM芯片(FT2500)的性能测试](/2021/05/15/%E9%A3%9E%E8%85%BEARM%E8%8A%AF%E7%89%87(FT2500)%E7%9A%84%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95/)

[十年后数据库还是不敢拥抱NUMA？](/2021/05/14/%E5%8D%81%E5%B9%B4%E5%90%8E%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%98%E6%98%AF%E4%B8%8D%E6%95%A2%E6%8B%A5%E6%8A%B1NUMA/)

[一次海光物理机资源竞争压测的记录](/2021/03/07/%E4%B8%80%E6%AC%A1%E6%B5%B7%E5%85%89%E7%89%A9%E7%90%86%E6%9C%BA%E8%B5%84%E6%BA%90%E7%AB%9E%E4%BA%89%E5%8E%8B%E6%B5%8B%E7%9A%84%E8%AE%B0%E5%BD%95/)

[Intel PAUSE指令变化是如何影响自旋锁以及MySQL的性能的](/2019/12/16/Intel PAUSE指令变化是如何影响自旋锁以及MySQL的性能的/)

[lmbench测试要考虑cache等](https://blog.csdn.net/xuanjian_bjtu/article/details/107178226)

comment：

Intel 8163 IPC是0.67，和在PostgreSQL下测得数据基本一致。Oracle可以达到更高的IPC。从8163的perf结果中，看不出来访存在总周期中的占比。可以添加几个cycle_activity.cycles_l1d_miss、cycle_activity.stalls_mem_any，看看访存耗用的周期占比。



Reference:

