# FriendlyARM-Tiny6410


![TinyADK-1312-S70-B01b](image\tinyadk-1312-s70-b01b.png)


>Tiny6410简介

* Tiny6410是一款以ARM11芯片(三星S3C6410)作为主处理器的嵌入式核心板，该CPU基于ARM1176JZF-S核设计，内部集成了强大的多媒体处理单元，支持Mpeg4, H.264/H.263等格式的视频文件硬件编解码，可同时输出至LCD和TV显示；它还并带有3D图形硬件加速器，以实现OpenGL ES 1.1 & 2.0加速渲染，另外它还支持2D图形图像的平滑缩放，翻转等操作。

* Tiny6410采用高密度6层板设计，尺寸为64 x 50mm，它集成了256M Mobile DDR RAM，256M/1GB SLC Nand Flash存储器，采用5V供电，在板实现CPU必需的各种核心电压转换，还带有专业复位芯片，通过2.0mm间距的排针，引出各种常见的接口资源，以供不打算自行设计CPU板的开发者进行快捷的二次开发使用。

*  Tiny6410SDK是采用Tiny6410核心板的一款参考设计底板，它主要帮助开发者以此为参考进行核心板的功能验证以及扩展开发。该底板具有三LCD接口、4线电阻触摸屏接口、100M标准网络接口、标准DB9五线串口、Mini USB 2.0接口、USB Host 1.1、3.5mm音频输入输出口、标准TV-OUT接口、SD卡座等常用接口；另外还引出4路TTL串口，另1路TV-OUT、SDIO2接口(可接SD WiFi)接口等；在板的还有蜂鸣器、I2C-EEPROM、备份电池、AD可调电阻、8个中断式按键等。

*  充分地发挥了6410支持SD卡启动这一特性，使用精心研制的Superboot-6410，无需连接电脑，只要把目标文件拷贝到SD卡中(可支持高达32G的高速大容量卡)，你就可以在开发板上极快极简单地自动安装各种嵌入式系统(WindowsCE6/Linux/Android/Ubuntu/uCos2等)；甚至无需烧写，就可以在SD卡上直接运行它们！配合MiniTools，开发者还可以十分方便地通过USB下载单个文件到内存运行，并且通吃各种Windows/Linux平台环境，非常便于调试之用！

---

![topside](image\topside.png)
![backside](image\backside.png)
![tiny6410-android](image\tiny6410-android.png)
![coreboard](image\coreboard.png)

- ##### CPU处理器
  - ###### Samsung S3C6410A，ARM1176JZF-S核，主频533MHz，最高667Mhz
- ##### DDR RAM内存
  - ###### 256M Mobile DDR RAM, 32bit数据总线
- ##### FLASH存储
  - ###### 标配为256M SLC NAND Flash
  - ###### 可选1GB SLC NAND Flash（500PCS起订）
- ##### 接口资源
  - ###### 2 x 60 pin 2.0mm space DIP connector
  - ###### 2 x 30 pin 2.0mm space DIP connector
- ##### 在板资源
  - ###### 4 x User Leds (Green)
  - ###### 10 pin 2.0mm space Jtag connector
  - ###### Reset button on board
  - ###### Supply Voltage from 4.75V to 5.25V
- ##### PCB规格尺寸
  - ###### 6层高密度电路板，采用沉金工艺生产
  - ###### 64 x 50 x 12 (mm)
- ##### 操作系统支持
  - ###### U-boot
  - ###### Linux2.6.38 + Qtopia2+ QtE4.8.5
  - ###### WindowsCE 6.0
  - ###### Android 2.3.4
  - ###### Ubuntu-0910
  - ###### 详细的6410裸机教程
  - ###### uCos2-6410
