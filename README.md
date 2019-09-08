## 本周工作总结

​	本周是开学报道后的第一周，有一些像报道等手续要办理，因此占去了不少时间 ，但是这也是熟悉学校所应该花的时间，因此也很值得。

​	本周学习了关于计算机体系结构量化研究方法[^3]的基础内容，补充了关于RISC-V指令集[^2]下计算机体系结构的流水线设计，了解了关于蜂鸟E200的结构以及代码风格。其中，关于流水线的学习[^1]让我对流水线的认识更加深刻。

[^1]:Computer Organization and Design:The Hardware Software Interface [RISC-V Edition]
[^2]:教你设计CPU——RISC-V处理器
[^3]:计算机体系结构：量化研究方法

**************

####  1.对计算机体系结构采用量化研究方法

##### (1)计算机体系结构的定义：

1. 指令集设计

   指令集设计包括：

   | 设计内容 | 具体含义                                                     |
   | -- | -------- |
   | ISA分类 | 寄存器-存储器ISA      载入-存储ISA（仅在载入和存储时可以访问存储器） |
   | 存储器寻址 | 使用字节寻址来访问存储器操作数 |
   |    操作数类型和大小     |   浮点/整型  64位/32位   |
   |    操作指令     |   算术指令/逻辑指令/控制指令/浮点指令/....   |
   |    控制流指令     |   跳转指令   |
   |    ISA编码方式    |  指令格式  |

2. 组成 (microarchitecture, 微体系结构)

   例如，虽然AMD和Intel的芯片都是基于x86的指令集架构，但是它们的结构设计却不同

3. 硬件实现：

   即具体实现的不同，例如逻辑设计，封装设计等等

##### (2)计算机体系结构的性能趋势：

1. 带宽和吞吐量趋势：带宽胜过延迟

   带宽/吞吐量：单位时间内所完成的任务数

   延迟：一个任务完成所需要的时间

2. 晶体管的特征尺寸与连线：

   特征尺寸：晶体管或者连线在x或者y方向上的最小尺寸。

   特征尺寸越小，晶体管性能越高；

   连线延迟基本没有改进

##### (3)计算机体系结构的功耗和能耗趋势

1. 能耗和功耗的区别：

   功耗是单位时间的能耗，一般用能耗去对比处理器的好坏，功耗则更多的是作为一个处理器设计的限制条件，例如最大功率的限制，热设计功耗(TDP)对冷却的限制等等。

2. 动态能耗/功耗：

   主要来源：开关晶体管

   <img src="https://images.gitee.com/uploads/images/2019/0908/102958_cd76fae5_5221112.png" width="400" height="100" alt="示意图" align=center>

   其中，1/2表示的是晶体管从0->1或者1->0的过程

   可以看到，功耗大小和开关频率有关，因此降低时钟频率可以降低功耗大小

3. 功耗是限制计算机的主要因素

   通常的解决办法有：关闭非活动模块的时钟，进行动态电压-频率调整(DVFS)，针对PMD等空闲时间较长的设计，以及超频模式(turbo)
   
   *这里再多说一句超频模式：*
   
   *在我们的个人笔记本中也会有超频模式，一般会高于给出的工频，以我的thinkpad中所带的i7 8550U为例，一般情况下，它的工频是1.8GHz，但是在很多情况下，它可以实现超频。*
   
   <img src="https://images.gitee.com/uploads/images/2019/0908/104347_f2632104_5221112.png" width="200" height="100" alt="示意图" align=center><img src="https://images.gitee.com/uploads/images/2019/0908/104810_83bac07b_5221112.png" width="200" height="100" alt="示意图" align=center>
   
4. 其他处理办法：
   在晶体管尺寸较小的时候，静态功率影响也很大，这时候我们需要关闭电源。
   在多核CPU下，采用竞相暂停(race-to-halt)的方法
   
5. 对于系统性能的评价标准：
   
   每瓦所完成的任务数: ssj-ops/瓦
   ssj : server side Java operations per second
   

##### (4)计算机体系结构的成本趋势
1. 学习曲线： 使成品率提升一倍，就可以使价格减半
   
2. 集成电路的成本：
   分为可变成本和固定成本(掩模组成本)
   对可变成本而言，可以按照如下的量化方法进行考虑：
   

<img src="https://images.gitee.com/uploads/images/2019/0908/111119_4da2485b_5221112.jpeg" width="300" height="375" alt="示意图" align=center>

##### (5)计算机体系结构的可信任度
服务等级协议(SLA)用于判断系统是否正常运行，即判断满足SLA的时间，根据SLA可以将系统分为两种：服务实现，服务中断
由实现到中断的过程被称为故障，由中断到实现的过程被称为恢复。

有两种对故障和恢复进行量化度量的方法：

1. 模块可靠性：用MTTF(平均无故障时间)，或者故障率(1/MTTF)来表示
   故障率还可以用10亿小时中发生的故障数(FIT, failures in time)来表示
   服务中断用平均修复时间(MTTR)来表示
   
2. 模块可用性：
$$
模块可用性 = \frac {MTTF}{MTTF+MTTR}
$$

3. 如何降低故障？
   增加冗余，可以通过增加资源冗余和时间冗余，书中关于资源冗余举了一个例子，这里不再赘述

##### (6)计算机的性能测试和量化方法

1. 衡量计算机的性能可以采用执行时间来表示
   执行时间：可以被认为挂钟时间，响应时间(包括I/O运行时间)，或者CPU时间(不包括I/O运行时间)
   
2. 比较两个计算机的性能：
$$
n =  \frac {执行时间y}{执行时间x} =  \frac {性能x}{性能y}
$$

3. 基准测试，运用广泛的基准测试包括，桌面基准测试(典型的包括，SPEC，标准性能评估机构)，服务器基准测试(TPC)

4. 对性能衡量时，常使用的方法是和基准计算机进行比较，即
$$
   SPECRatio = \frac {执行时间_{基准}}{执行时间A}
$$
进行多个SPEC标准程序测试的SPECRatio计算是采用几何平均
##### (7)计算机设计中的量化原理
1. 采用并行
   指令级并行——采用流水线
   数据级并行——例如ALU中采用先行进位
   
2. 局域性原理
   包括时间局域性和空间局域性
   
3. 重点关注常见情形，即对最常见的某些情形进行优化

4. Amdahl定律的量化表示

   其定义了加速比，升级比例，升级加速比

   对于加速比的定义类似于(6)中对两种计算机的性能比较：
   <img src="https://images.gitee.com/uploads/images/2019/0908/141432_e6119e06_5221112.png" width="400" height="100" alt="示意图" align=center>

   对于升级比例以及升级加速比的定义是：
   升级比例是原任务中可以被升级的时间占总时间的比例
$$
   升级加速比 = \frac {原模式的执行时间}{现模式的执行时间}
$$
   这样得到的总加速比就可以用升级比例以及升级加速比来表示：
<img src="https://images.gitee.com/uploads/images/2019/0908/141719_1f7d7a17_5221112.png" width="400" height="115" alt="示意图" align=center>
   由于升级加速比不会比1小，因此总加速比不会比1/1-升级比例要大。

##### (8)处理器性能公式
1. 指令路径长度或者指令数(IC)
2. CPI(每指令时钟周期数)：
$$
CPI = \frac {1}{IPC(每周期指令数)} = \frac {程序的CPU时钟周期数}{指令数(IC)}
$$
3. CPU时钟周期数：
$$
CPU时间 = IC*CPI*时间周期
$$
因此对于CPU时间的影响因素包括IC,CPI,时钟周期数。其中，IC的大小受指令集体系结构和编译器技术的影响，CPI受组成和指令集体系结构的影响，时间周期受硬件技术和组成的影响

##### (9)性能、价格、功耗的平衡
书中举出的例子表示，仅考虑价格或则和仅考虑ssj都是不可取的，在衡量一个CPU好坏的时候，我们需要对其ssj以及功耗进行综合考虑，即考虑在单位瓦数下的每秒操作指令数

*************
#### 2.RISC-V以及蜂鸟E200的设计理论
##### (1)蜂鸟E200简介
1. 蜂鸟处理器系统如图所示：
<img src="https://images.gitee.com/uploads/images/2019/0908/145030_0c402816_5221112.png" width="300" height="200" alt="示意图" align=center>

采用ITCM(指令紧耦合存储)和DTCM(数据紧耦合存储)，实现数据和指令的分离存储
设计中断接口，调试接口，快速I/O，协处理器接口，以及总线接口
2. 书中表示该E200的处理器同时有配套的SoC：
   对于中断接口，书中设计了CLINT(Core Local Interupts Controller)用于管理局部中断，以及PLIC(Platform Level interpret Controller)用于管理系统中断
3. 书中表示这些处理器都具有一定的配置性，例如可以配置RISC-V是否支持原子(atom)操作，即是否支持"A"指令集的拓展。
##### (2)蜂鸟E200的RTL代码风格
1. 采用标准DFF模块例化生成`寄存器`
   
   例如：
   
```verilog
   sirv_gnrl_dfflr #(1) flg_dfflr(flg_ena,flg_nxt,flg_r,clk,rst_n);
```

   对flg_dfflr进行实例化，其中，使能信号flg_ena，时钟信号clk，复位信号rst_n，输出信号flg_r，输入信号flg_nxt。

2. 标准DFF模块内部：

   以sirv_gnrl_dfflr为例：

```verilog
   module sirv_gnrl_dfflr #(parameter DW = 32)
   (input lden, input  [DW-1:0]dnxt, output [DW-1:0]qout, input clk, input rst_n);
   reg [DW-1:0] qout_r;
   
   always @(posedge clk or negedge rst_n):
   	begin:
   		if(rst_n == 1'b0)
   		qout_r <= {DW{1’b0}};
   		else if(lden == 1'b1)
   		qout_r <= dnxt;
   	end
   assign qout = qout_r;
```

   *需要注意的是，这里使用的IEEE 2001标准下的Verliog实例化的模型调用方法，这种调用方法有两种，分别是*
   *(1)*

```verilog
   module_name #(parameter1,parameter2) inst_name(port_map);
```
   *例如，addr_16 #(4,8) AD1(sum1,a1,b1);*
   *(2)*
```verilog
   module_name #(.parameter_name(para_value),.parameter_name(para_value))
   inst_name(port_map);
```
   *例如：sirv_gnrl_xchecker #(.DW(1)) u_sirv_gnrl_xchecker(.i_dat(lden),.clk(clk));*


3. 由于Verilog语句中的if-else不能传播不定态，因此我们需要对lden处于不定态x的情况进行捕捉，这里不再赘述

4. ###### 推荐使用assign语句代替if-else以及case语句
1. if-else以及case语句的缺点：
   if-else语句以及case语句都不能传播不定态，即
```verilog
   if(a) out = in1;
   else out = in2;
```
   这样的语句在a为x的情况下，会使得out将结果in2传播出去，但是如何我们不希望a在不定态时有值输出，应该采用assign语句去代替：
```verilog
   assign out = a?in1:in2;
```
   此时，我们可以在a为不定态时，同时得到out也为不定态
2. if-else语句和case语句还有一个缺点是会被综合成优先选择电路，实际上，这和实际采用的综合软件有关系，有的资料就认为多if语句综合得到的是一个无优先级的电路:

   > https://mp.weixin.qq.com/s/WqD06bMPQ4c8hd9s2Bi4XQ

   而使用vivado时if-else得到的确实是优先选择电路，如下图所示是if-else语句：

<img src="https://images.gitee.com/uploads/images/2019/0908/161240_d77fc664_5221112.png" width="450" height="200" alt="示意图" align=center>

采用assign语句可以实现这样的优先级选择：
```verilog
assign out = sel1?:in1[3:0]:sel2?in2[3:0]:sel3?in3[3:0]:4'b0;
```
同时采用assign语句可以实现并行选择：
```verilog
assign out  = ({4{sel1}}&in1[3:0])|({4{sel2}}&in2[3:0])|({4{sel3}}&in3[3:0]);
```
3. 其他注意事项：
· 带有reset的寄存器的面积和时序会变差，因此仅在控制通路中使用含有reset的寄存器
·信号命名应当规范
·clock和reset仅仅被用在DFF和复位中

##### (3)RISC-V的指令列表
1. 指令存放在存储器中的地址位置：指令PC(instruction counter)
2. 寻址空间：取决于寄存器的XLEN，32位寄存器的寻址空间即为4GB
3. CSR寻址空间：处理器核内部的寄存器，使用专有的12位地址编码空间
4. Halt模式：当一个处理器核中设计多个硬件线程(hardware thread)技术的时候，将一个硬件线程定义为Halt
5. 复位与中断：*没有仔细研究*

##### (4)流水线结构
1. 经典的五级流水线，即5个clock为一个时钟周期
   分别是取指，译码，执行，访存，写回
2. 流水线和状态机的关系
   流水线结构：以面积换性能，以空间换时间     流水线结构是一种空间展开结构
   状态机结构：以性能换面积，以时间换空间，状态机比流水线结构的吞吐率低，运行速度较慢，是一种折叠结构
3. 流水线深度：
   现代计算机一般有极深的流水线，这是因为若两级流水线之间的逻辑硬件越少，那么就意味着时钟周期可以设计的越短，主频越高，而主频越高，则代表了吞吐率越高，性能越高
   但是流水线深度加深，会导致面积开销的增大，且各级流水线由于需要握手，后级反压串扰前级，会造成严重的时序问题
   除此之外，流水线取指时会带来control hazard的问题，这是需要流水线冲刷(pipeline flush)，此时流水线越深，冲刷带来的浪费的时序就越多。
   因此，流水线深度一般也不会太深。

************

#### 3. 处理器时序和流水线

#####  (1)处理器时序：
1. 对处理器而言，有三种主要的指令类型：memory-reference/arithmetic-logical/branches
其中，每一个instruction都要使用ALU:

| instruction class   | ALU functions                   |
| ------------------- | ------------------------------- |
| memory-reference    | address calculation             |
| arithmetric-logical | operation execution             |
| branches            | equality test(in ALU ,subtract) |

2. clocking methodology
clocking methodology用于解决寄存器同时读入和读出问题：
即对Reg而言，通过写入操作在上升沿触发，即写入Reg的操作总在一个时钟周期的前半段完成，而读出仅在一个周期的后半段完成，这样就可以避免读入和写入的冲突

*这里还需要补充一点:
signal called logically high : asserted 
signal called logically low: deasserted* 

##### (2)RISC-V指令格式
1. RISC-V的指令一般是32位，在了解时序之前必须要知道RISC-V的指令格式，这很重要，如下图所示：
<img src="https://images.gitee.com/uploads/images/2019/0908/172529_01f32e52_5221112.png" width="500" height="175" alt="示意图" align=center>

其中，rd的含义是目标寄存器(destination regitster)的地址，rs1和rs2表示的指令中提到的两种寄存器的地址。
###### 在Ioad指令中，funct7和rs2组成了立即数，并和rs1中的数组合成了存储器的地址，rd中存的是从存储器中取出的值要放到的寄存器的号
###### 在store指令中，funct7和rd组成了新的立即数，并和rs2中的数组合成了存储器的地址，rs1中存的是要存入的地址的数。
###### opcode这部分对load与store没有用，但是会影响R指令中采用何种计算方式
2.  register file
   register file的含义是a collection of registers in which any register can be written or read by specifying the number of the register.

3. beq 命令
   例如，beq x1,x2,offset：
   若x1 == x2，那么就跳转到PC+offset shifted left 1bit(2倍)
是否跳转可以被表示成为：
   branch is taken 或者 branch is not taken

4. 单个指令的datapath
如图所示，单个指令的datapath示意图如下所示：
<img src="https://images.gitee.com/uploads/images/2019/0908/180557_7c8be3aa_5221112.png" width="500" height="250" alt="示意图" align=center>

控制信号的来源就是instruction，我们对instruction进行分析：

<img src="https://images.gitee.com/uploads/images/2019/0908/192044_caa466f6_5221112.png" width="150" height="300" alt="示意图" align=center>

这张图表示，我们对于ALU的控制来源于ALUop和instruction的一部分，由这两部分共同决定，即我们需要构建一个译码器，将这两部分译成我们希望得到的ALU操作，ALU操作一共有四种:
add，sub，or，and，我们对其进行编码为：

| ALU操作  | 编码 |
| -------- | ---- |
| and      | 0000 |
| or       | 0001 |
| add      | 0010 |
| subtract | 0110 |

而我们的Load，Store，beq，R指令的opcode编码分别是00，00，01，10。R指令同时要考虑funct7和funct3。

因此我们获得的控制单元的真值表如图所示：
<img src="https://images.gitee.com/uploads/images/2019/0908/193047_3e23a2ae_5221112.png" width="300" height="100" alt="示意图" align=center>

且有，决定我们的OP的并不是instruction的每一位，可以看到第30位，第14到第12位和Op会影响取值，因此我们只需要给ALUcontrol传Op和30+[14:12]即可。

##### (3)RISC-V的pipeline
1. RISC-V一般也和MIPS等相同采用5级流水线。分别是
IF(instruction fetch)，ID(instruction decode)，EX(execution)，MEM(memory access)，WB(write back)

2. 流水线的好处和坏处
流水线的好处：增加吞吐率(throughput)
流水线的坏处：采用流水线容易导致三种hazard：struction hazard，data hazard，control hazard
###### struction hazard ：含义是硬件不能够支持指令的结合，例如存储器的读写冲突
###### data hazard：
```
add x19,x0,x1
sub x2,x19,x3
```
由于x19的值被下一条指令运用，因此我们如果想要获得正确的结果，应当在流水线中插入stall
或者采用forwarding(转发)来解决这个问题
转发技术的图示如下：
<img src="https://images.gitee.com/uploads/images/2019/0908/195603_e0860158_5221112.png" width="350" height="125" alt="示意图" align=center>
当然，转发技术也不能够解决所有的data hazard
load-use data hazard就不能够通过转发技术来解决，一般需要一个pipeline stall/bubble来占据流水线的一个周期
当然，在代码较长的情况下，我们也可以考虑采用调整代码顺序来解决pipeline stall的问题
###### control hazard:
control hazard出现在beq跳转指令中，由于跳转指令需要根据EX阶段的计算内容来决定是否改变PC的值，因此必然会出现流水线暂停的情况，我们的解决方案通常是：
直接默认条件分支不执行。

##### (4)RISC-V的pipeline结构
1. 采用流水线需要在结构中加入寄存器，对于五个stage，我们加入四个寄存器分别为ID/IF，IF/EX，EX/MEM，MEM/WB。其中，WB写回不需要寄存器，这是因为直接提取和写入即可，不会对流水线产生影响。

2. 对流水线的表述通常有两种，分别是multiple-clock-cycle pipeline diagrams和signle-clock-cycle pipeline diagrams

3. 流水线的pipeline control：对流水线的控制只需要将其在该用的位置拿出寄存器的值就可以了：其中IF和ID阶段不需要流水线控制，仅在EX，MEM，WB阶段需要。
在EX阶段中，ALUop和ALUsrc的值被用到。
在MEM阶段中，Branch，Memread和Memwrite被用到。
在WB阶段中，MemtoReg和regwrite被用到。
###### 在流水线结构中，我们需要将ID阶段所获得的WB/M/EX的寄存器中一直传输下去。
如图所示：
<img src="https://images.gitee.com/uploads/images/2019/0908/202630_c9407945_5221112.png" width="500" height="250" alt="示意图" align=center>

###### (5)RISC-V的pipeline结构解决data hazard
1. 一般的data hazard：
设置一个forwarding unit单元，用于比较流水线上的EX/MEM.RegisterRd和MEM/WB.RegisterRd和ID/EX.RegisterRs1和ID/EX.RegisterRs2的值不同。
当然，我们需要注意的是寄存器不可以是0寄存器，因为在RISC-V中0寄存器被保留，不可以被写。
除此之外，我们还需要考虑一种情况：
例如：
```
add x1,x1,x2
add x1,x1,x3
add x1,x1,x4
```
可见，x1的值在最后的一条指令中被用到，而前两条的指令都用到了x1这个寄存器。
这时，如何判断x1从EX还是MEM处取得结果：我们的依据是选取最近的结果。
因此我们得到的总的结果是：
```
if(MEM/WB.RegWrite and (MEM/WB.RegisterRd != 0) and not(EX/MEM.RegWrite and (EX/MEM.RegisterRd != 0) and (EX/MEM.RegWrite == ID/EX.RegisterRs1)) and (MEM/WB.RegisterRd == ID/EX.RegisterRs1))
ForwardA = 01;

if(EX/MEM.RegWrite and (EX/MEM.RegisterRd ! = 0) and (EX/MEM.RegWrite == ID/EX.RegisterRs1))
ForwardA = 10;
```
2. load-use data hazard:
对于load-use data hazard而言，不可以仅仅通过转发技术来解决，而是需要stall。
但是stall需要PC和IF/ID寄存器都保持不变，在EX后面的执行中也要求什么也不做：nops
根据前面的真值表，可见，只要将所有的control全部置为0，我们就可以实现nops的操作。
在电路结构上，我们需要采用hazard detection unit来解决这个问题

 



### 存在问题
1. 什么是流水线的握手？我还没有看到这方面的内容
2. 计算机体系结构内容较多，我觉得我可能需要3周左右的时间来补一下
3. 关于python的难点，我后来仔细想了一下，python语法中最麻烦的应该就是类class了，模块module本质上就是一个py文件，其实直接用python语言本身能够解决的问题并不多，在解决一些有算法思路的问题上，java和C/C++更有优势。且python有一个致命的问题就是解释型语言速度过慢的问题，deecamp夏令营的时候产业mentor曾经建议我们如果想要加速程序，采用Cpython，也就是基于C内核的python，当时时间紧张并没有学习，而是继续用python去写的。



### 下周工作计划

1.
2.
3.

### 需要支持

1.
2.
3.
