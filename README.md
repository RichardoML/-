# -
组原课设-mips CPU ACM1601江易星

基本内容一：24基本指令 + 4扩展指令 logisim工程

cpu-28是24条基本指令 + 4条随机抽取的扩充指令的完整版本

这个数据通路画得丑的一批，我自己都看不下去自己画的这玩意，等我重新画好了再传一个吧

cpu-28中需要用到：

1）cs3410.jar库（整个实验都需要）

2）expand.circ:此子电路用于在24条基本指令的基础上扩充4条指令

之前做完24条基本指令是忘记上传了，等到想起来时已经加了4条扩充指令了

3）expand.hex文件时用来测试28条指令的代码，可以直接加载到logisim 的存储器中运行。


基本内容二：单周期mips 24基本指令+4扩展指令 vivado上板

使用方法和步骤：
A.  cpu28_vivado.zip文件，此文件来自于 ACM1601朱博抡  小组成员可以公用一个基础模板

每个人根据自己的扩展指令对代码稍微修改就可以了。

使用之前请一定先仔细阅读cpu28_vivado.zip内的readme文件。！！！！

使用之前请一定先仔细阅读cpu28_vivado.zip内的readme文件。！！！！

使用之前请一定先仔细阅读cpu28_vivado.zip内的readme文件。！！！！！

解压后用vivado打开目录下名为 cpu24的vicado project，每个人只需要修改 CPU_in_DDR4 -> mCPU_SingleCycleCPU 以及其下的ROM 和 Controller文件即可，其他任何文件都不需要修改。

举例说明如何修改

1）ROM：只需要修改测试代码文件所在路径：即 readmemh函数中的路径，指向你的测试代码文件
（把基本内容一中的测试代码中第一行“v2.0 raw”去掉即可） 。注意路径中不能有中文名，并且由于字符的转义问题，路径中的 \ 应该改为\\

2) Cntroller:在module Controller（）那一行的bltz之后加上你的4条扩展指令新加入的控制信号。

例如我新加入了 shamtsrc 信号和 ByteOrWord。相应的，

在紧接着下面output reg那一行中 bltz后面加入你加入的新的控制信号。

然后在下面的case（op） case（func）代码部分中对应的增加。（不知道怎么增加的请先看看已有代码）

例如我有一条扩充指令  XORI op ：14 func：xx

控制信号为  alu_op = 9 alu_src =1  regWrite = 1

我就在对应的case op下加入 14: begin alu_op = 9; alu_src =1 ; regWrite = 1; end

3) SingleCycleCPU

59 60 行：wire变量的声明以及 Controller的调用 需要加上你新加上的 信号

下面的代码是构建的数据通路，如果扩充指令没有增加新的数据通路就不需要修改

如果有扩充指令改变了数据通路，找到数据通路对应的位置，修改最后一个变量的部分就可以了（可能没说清楚）

举例说明一下：
例如 我的扩充指令 SLLV 改变了 shamt的来源：24条基本指令的shamt都是直接来自指令中的 imm。SLLV来自rs寄存器的第五位。这条指令新增了一个 shamtsrc信号。这个信号只在sllv指令的时候为1

找到 assign shamt所在的位置，前面都一些意义你可能看不懂，但是没关系，你只需要在最后一个变量ins[10:6] 改为（shamtsrc？ rd1[4]:ins[10:6]）

这些都修改完之后你的代码就修改忘了：直接 run implement  -> generate bitstream 开始上板子。

B.expand_vivado.hex文件的路径是需要添加到 ROM readmemh中的，该文件直接将 基本内容一中的测试文件expand.hex第一行“v2.0 raw” 去掉即可


在你知道怎么修改之后，算上debug 的时间，最多半小时就可以解决单周期上板。
