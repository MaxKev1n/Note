**现代计算机系统抽象**

1. Application
2. Algorithm
3. Programming language
4. Operating System/ Virtual Machine
5. Instruction Set Architecture
6. Register-Transfer Level
7. Gates
8. Circuits
9. Devices
10. Physics



* Instruction Level Parallelism
  * Superscalar
  * Very Long Instruction Word(VLIW)
* Long Pipelines
* Advanced Memory and Caches
* Data Level Parallelism
  * Vector
  * GPU
* Thread Level Parallelism
  * Multithreading
  * Multicore
  * Multiprocessor
  * Manycore



"Architecture"/Instruction Set Architecture:

* Programmer visible state(Memory & Register)
* Operations (Instructions and how they work)
* Exeutions Semantics (interrupts)
* Input/Ouput
* Data Types/Sizes

Microarchitecture/Organization:

* Tradeoffs on how to implement ISA for some metrics (Speed, Energy, Cost)

例如：Intel的X86指令集就是Architecture，而AMD和Intel对于X86指令集各自的实现方式，不同版本的芯片则是Microarchitecture



### Memory Technology

<img src="F:\PIC\Computer Architecture\1.png" style="zoom: 50%;" >

这是一个简单的Register File，寄存器的宽度都为32bits，同时它们的总线也为32bits。通过2位的Read Address信号读取四个寄存器值，而Write Address信号通过下方的译码器对相应的寄存器赋写使能。

<img src="F:\PIC\Computer Architecture\2.png" style="zoom: 50%;" >

Address通过row 译码器送入到word lines，包括read word lines和write word lines，最终进入到每一个单独的存储单元(single storage element)，即bit cell

<img src="F:\PIC\Computer Architecture\3.png" style="zoom: 50%;" >

 与Register file相比，SRAM通常被制作为单端口的内存，而Register file通过被制作为多端口的内存。Register File具有更高的速度以及更多的端口，而SRAM具有更高的密度

<img src="F:\PIC\Computer Architecture\4.png" style="zoom: 50%;" >

DRAM不使用CMOS Logic技术，它的每一个cell仅包含一个电容器通过晶体管与bit line相连接。DRAM的优点在于很容易集成大量存储单元在相同面积的区域内，但是DRAM具有一个问题，它的存储单元会持续地掉电，因此需要刷新DRAM来为存储单元上电

<img src="F:\PIC\Computer Architecture\5.png" style="zoom: 50%;" >

### Cache

<img src="F:\PIC\Computer Architecture\6.png" style="zoom: 50%;" >

* 容量: Register << SRAM << DRAM
* 延迟: Register << SRAM << DRAM
* 带宽: on-chip >> off-chip

使用cache的重要原因:**Temporal Localiy**和**Spatial Locality**



<img src="F:\PIC\Computer Architecture\7.png" style="zoom: 50%;" >

<img src="F:\PIC\Computer Architecture\8.png" style="zoom: 50%;" >

Cache映射策略:

* 全相联
* 组相联
* 直接映射



如何寻找cache中的block?

* 通过index和offset进行匹配,然后检查tag
* tag检查仅仅包括higher order bits
* 对于组相联cache,cache对所有符合匹配的block并行进行tag检查



如何进行块替换?

* 直接映射的cache没有其他选择
* 随机
* Least Recently Used(LRU)
  * LRU cache状态在每次访问时都应该被更新
  * 4-8路的cache通常使用Pseudo-LRU binary tree

* FIFO aka round-Robin
  * 在highly associativeg caches中使用
* Not Most Recently Used(NMRU)
  * 最近被使用过的block应用FIFO策略时会产生异常



写策略:

* Cache Hit
  * 写直通法:将数据写回cache和memory,通常具有更高的traffic
  * 写回法:只将数据写回cache,当cache中的block被淘汰时才将数据写入memory

* Cache Miss
  * No Write Allocate:只将数据写到主存中(容易经常造成Miss)
  * Write Allocate:先将block存入cache,然后写数据

* Write Through & No Write Allocate
* Write Back & Write Allocate



#### Cache performance

三种Missing:

* Compulsory: 第一次对一个block进行调用, occure even with infinite cache
* Capacity: cache容量太小以至于无法存储程序所需的数据

* Conflict: 多个block被映射到同一个位置上导致一部分数据被替换

**Average Access Time = Hit Time + (Miss Rate * Miss Penalty)**

<img src="F:\PIC\Computer Architecture\9.png" style="zoom: 50%;" >

<img src="F:\PIC\Computer Architecture\10.png" style="zoom: 50%;" >

<img src="F:\PIC\Computer Architecture\11.png" style="zoom: 50%;" >

**如果cache的大小变成双倍,那么miss rate通常会下降$\sqrt{2}$倍**

<img src="F:\PIC\Computer Architecture\12.png" style="zoom: 50%;" >

**大小为$N$的直接映射cache的miss rate与大小为$N/2$的二路组相联cache大致相等**

