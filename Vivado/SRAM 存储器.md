# SRAM 存储器

## 一、EGO1-SRAM 特性

### 1.1 功能框图

![](images/2024-05-25-13-48-55.png)

其中控制位如下：

表示|功能
:--:|:--:
$\overline{\text{CE}}$|片选，当其为高时进入待机模式，为低时选中
$\overline{\text{OE}}$|输出使能，为低时输出有效
$\overline{\text{WR}}$|写入使能，低为写，高为读
$\overline{\text{UB}}$|数据高八位
$\overline{\text{LB}}$|数据低八位

### 1.2 真值表

![](images/2024-05-25-13-54-08.png)

### 1.3 数据读取（READ）

![](images/2024-05-25-13-55-21.png)

1. $\overline{\text{WR}}$ 在数据读取期间一直为高电平
2. 先使能片选信号 $\overline{\text{CR}}$ （为低），然后使能输出 $\overline{\text{OE}}$ ，依次通过使能 $\overline{\text{UB}}$ $\overline{\text{LB}}$ 从高位到低位读取数据，读到后再失能上述端口即可
3. **地址**最先有效，最后失效

>如果要实现快速读取，需要注意各部分的时间
![](images/2024-05-25-14-09-07.png)
![](images/2024-05-25-14-09-15.png)

### 1.4 数据写入（WRITE）

采用 $\overline{\text{CE}}$ 控制的方式：

![](images/2024-05-25-14-10-32.png)

1. 使用 $\overline{\text{CE}}$ 控制，$\overline{\text{OE}}$可以为高也可以为低，不影响
2. 当 $\overline{\text{CE}}$ 与$\overline{\text{WE}}$ 同时为低时且 $\overline{\text{LB}}$ $\overline{\text{HB}}$ 中任意一个为低时，可以对地址进行写入
3. **地址**最先有效，最后失效

>如果要实现快速写入，需要注意各部分的时间
![](images/2024-05-25-14-13-42.png)
![](images/2024-05-25-14-13-55.png)

## 二、实战应用——Verilog实现SRAM储存器访问

![](images/2024-05-25-14-14-35.png)