<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [第3课-U-Boot工作流程分析](#第3课-u-boot工作流程分析)
	- [课程索引](#课程索引)
	- [Uboot分析](#uboot分析)
	- [S3C2440](#s3c2440)
		- [设置中断向量表](#设置中断向量表)
		- [设定SVC模式](#设定svc模式)
		- [刷新I/D cache](#刷新id-cache)
		- [关闭MMU和Cache](#关闭mmu和cache)
		- [初始化系统时钟，设定串口，初始化Nand](#初始化系统时钟设定串口初始化nand)
		- [代码从启动设备到内存的拷贝](#代码从启动设备到内存的拷贝)
		- [设置堆栈](#设置堆栈)
		- [清除BSS段](#清除bss段)
		- [总结](#总结)
		- [第二阶段 软硬件初始化](#第二阶段-软硬件初始化)
	- [S3C6410](#s3c6410)
		- [程序入口](#程序入口)
		- [第一阶段程序分析](#第一阶段程序分析)
		- [第二阶段程序分析](#第二阶段程序分析)
	- [S5PV210](#s5pv210)
		- [程序入口](#程序入口)
		- [第一阶段程序分析](#第一阶段程序分析)
		- [bootloader2从哪里拷贝到内存哪里？](#bootloader2从哪里拷贝到内存哪里)
		- [总结](#总结)
		- [第二阶段程序分析](#第二阶段程序分析)
		- [总结](#总结)

<!-- /TOC -->
# 第3课-U-Boot工作流程分析

模仿到设计的原则，模仿业界老大，要模仿必先了解它的工作原理

## 课程索引

![1526288986722.png](image/1526288986722.png)

      Bootloader0：芯片厂商提供好了的，固化不可变
      Bootloader1：BL1 垫脚石
      Bootloader2：BL2 内存SDRAM
      2440、6410启动代码比较接近

## Uboot分析

      每一个开发板在Uboot源码Makefile中都有一个配置文件

![1526289239326.png](image/1526289239326.png)

![1526289258883.png](image/1526289258883.png)

      找到名称，然后在board目录下找到smdk相对于的子目录
      存放的就是开发板相关的文件。
      关注Uboot链接器脚本

![1526289289922.png](image/1526289289922.png)

![1526289309756.png](image/1526289309756.png)



## S3C2440

![1526289314931.png](image/1526289314931.png)

![1526289358620.png](image/1526289358620.png)

      根据链接器脚本可以知道，代码段首是start.o

![1526289414573.png](image/1526289414573.png)

![1526289453358.png](image/1526289453358.png)

      但是哪一行代码优先运行？Entry表明程序入口

![1526289475607.png](image/1526289475607.png)

      也就是说，start.o中的Entry优先运行

![1526295708715.png](image/1526295708715.png)

![1526295722908.png](image/1526295722908.png)

			分析阶段只关心做了什么，不关心具体是怎么实现的。
			注释比较详细，不用担心

### 设置中断向量表

### 设定SVC模式

![1526296001233.png](image/1526296001233.png)

### 刷新I/D cache

![1526296062265.png](image/1526296062265.png)


### 关闭MMU和Cache
![1526296045031.png](image/1526296045031.png)

### 初始化系统时钟，设定串口，初始化Nand

![1526296151127.png](image/1526296151127.png)

			判断uboot是否在内存中，如果不在进行内存初始化

![1526296166372.png](image/1526296166372.png)

###	代码从启动设备到内存的拷贝

![1526296401911.png](image/1526296401911.png)

### 设置堆栈

![1526296478397.png](image/1526296478397.png)

### 清除BSS段

![1526296511126.png](image/1526296511126.png)


### 总结

1. NandFlash 开头4KB拷贝到垫脚石。start.S
2. CPU从start.S开始执行
3. 把NandFlash剩余代码复制到内存中的固定位置
4. 从垫脚石跳转到bootloader2阶段继续运行


![1526296696239.png](image/1526296696239.png)

			凭什么说start_armboot在内存中(bootloader2阶段)？以下分析

![1526296789325.png](image/1526296789325.png)

			先配置工程，然后make，会生成链接脚本

![1526296849929.png](image/1526296849929.png)

			u-boot是elf格式，u-boot.bin是二进制

![1526296891223.png](image/1526296891223.png)

			这个地址确实是内存中，但是怎么来的？？？

![1526296926609.png](image/1526296926609.png)

			第一阶段是从垫脚石开始，垫脚石是从0地址开始。看下链接器脚本

![1526296985178.png](image/1526296985178.png)

![1526297003503.png](image/1526297003503.png)

![1526297038741.png](image/1526297038741.png)

![1526297056836.png](image/1526297056836.png)

			指定使用链接器脚本，链接器脚本可以指定起始地址，但是-Ttext也可以指定地址。很明显-Ttext优先
			配置是在芯片的config里面
			修改配置文件的地址，妥妥生效

![1526297115558.png](image/1526297115558.png)

![1526297179684.png](image/1526297179684.png)

![1526297337826.png](image/1526297337826.png)

			为什么有些跳转没有跳到内存中，而是还在垫脚石。因为是相对跳转哦
			链接地址不代表PC的值。
			而LDR pc,0x30008000 就是绝对跳转了

![1526297534980.png](image/1526297534980.png)

![1526297867203.png](image/1526297867203.png)


### 第二阶段 软硬件初始化

**初始化一些环境变量**
**第二阶段基本都是start_armboot函数硬件纯软件初始化**

![1526297811493.png](image/1526297811493.png)

			for循环调用初始化函数

![1526297838250.png](image/1526297838250.png)

			初始化串口

![1526297908034.png](image/1526297908034.png)

			LCD初始化

![1526297940834.png](image/1526297940834.png)

			网卡初始化

![1526297960719.png](image/1526297960719.png)

			LED初始化

![1526297990099.png](image/1526297990099.png)

			执行用户输入的命令

![1526298036949.png](image/1526298036949.png)

* 总结

![1526298104058.png](image/1526298104058.png)


## S3C6410

### 程序入口

			通过Makefile找到程序入口

![1526298216113.png](image/1526298216113.png)

			打开链接器脚本
			段头文件以及程序入口

![1526298251032.png](image/1526298251032.png)

![1526298285905.png](image/1526298285905.png)

![1526298317514.png](image/1526298317514.png)


### 第一阶段程序分析

			设置中断向量表

![1526298341932.png](image/1526298341932.png)

			设置CPU到SVC模式

![1526298355124.png](image/1526298355124.png)

			刷新I/D cache

![1526298379643.png](image/1526298379643.png)

			关闭MMU和Cache

![1526298391740.png](image/1526298391740.png)

			外围设备基地址进行初始化，6410才有，2440没有

![1526298422020.png](image/1526298422020.png)

			lowlevel_init主要是点亮LED，帮助程序进行调试

![1526298453391.png](image/1526298453391.png)

![1526298461976.png](image/1526298461976.png)

![1526298477146.png](image/1526298477146.png)

			关闭看门狗

![1526298504004.png](image/1526298504004.png)

			关闭所有中断

![1526298643029.png](image/1526298643029.png)

			初始化系统时钟

![1526298666876.png](image/1526298666876.png)

			初始化串口

![1526298680125.png](image/1526298680125.png)

			初始化NandFlash

![1526298699249.png](image/1526298699249.png)

			初始化内存

![1526298726161.png](image/1526298726161.png)

			拷贝第二阶段到内存中

![1526298776571.png](image/1526298776571.png)

			堆栈初始化

![1526298795785.png](image/1526298795785.png)

			清除bss段

![1526298842166.png](image/1526298842166.png)

			跳转到start_armboot

![1526298865707.png](image/1526298865707.png)

![1526298872007.png](image/1526298872007.png)



### 第二阶段程序分析

第二阶段和2440一样

## S5PV210

### 程序入口

启动过程

1. IROM通过映射到0地址处，处理器从0开始执行
2. 首先IROM固话程序把bootloader1复制到IRAM中，16KB，然后执行
3. 执行过程中复制bootloader2，如果booloader2大于80KB拷贝内存，否则放在IRAM垫脚石
4. 然后跳转bootloader2执行

booloader1和bootloader2其实放在同一个bin中。也有划分两个bin文件的

![1526299092007.png](image/1526299092007.png)

![1526299106639.png](image/1526299106639.png)




### 第一阶段程序分析

			设置中断向量表

![1526299116272.png](image/1526299116272.png)

			设置SVC工作模式

![1526299164650.png](image/1526299164650.png)

			让L1 I/D cache失效、关闭

![1526299195839.png](image/1526299195839.png)

			关闭MMU和Cache

![1526299236990.png](image/1526299236990.png)

			lowlevel_init主要是点亮LED

![1526299270816.png](image/1526299270816.png)

			I/O引脚初始化

![1526299322701.png](image/1526299322701.png)

			关闭看门狗

![1526299302685.png](image/1526299302685.png)

			SRAM、SROM初始化

![1526299375581.png](image/1526299375581.png)

			判断是否在内存中运行

![1526299408972.png](image/1526299408972.png)

			时钟初始化

![1526299424253.png](image/1526299424253.png)

			内存才能初始化

![1526299433700.png](image/1526299433700.png)

			串口初始化

![1526299459997.png](image/1526299459997.png)

			NandFlash简单初始化

![1526299476547.png](image/1526299476547.png)

			关闭ABB

![1526299496956.png](image/1526299496956.png)

			设置堆栈

![1526299539893.png](image/1526299539893.png)

			把bootloader2拷贝第二阶段到内存中

![1526299587733.png](image/1526299587733.png)

### bootloader2从哪里拷贝到内存哪里？

![1526299770942.png](image/1526299770942.png)

![1526299780837.png](image/1526299780837.png)

![1526299799295.png](image/1526299799295.png)

![1526299806925.png](image/1526299806925.png)

			把bootloader2复制到内存中0x23E0000

![1526299890545.png](image/1526299890545.png)

			跳转到bootloader2
			简单粗暴直接运行函数

![1526299944685.png](image/1526299944685.png)

### 总结

![1526299973677.png](image/1526299973677.png)



### 第二阶段程序分析

			入口在哪里？？第二阶段连接器脚本。
			还是从start.S中执行？重复了也
			操作也是重复的

![1526300010189.png](image/1526300010189.png)

![1526300029260.png](image/1526300029260.png)

			跳转到boot_init_f。该函数才是210中的boot_init

![1526300101940.png](image/1526300101940.png)

![1526300120782.png](image/1526300120782.png)

			该函数没有函数体
			弱函数，如果有同名函数就失效了。
			但是它跟另外一个函数关联起来。实际调用defulat函数

![1526300211636.png](image/1526300211636.png)

### 总结

![1526300417395.png](image/1526300417395.png)
