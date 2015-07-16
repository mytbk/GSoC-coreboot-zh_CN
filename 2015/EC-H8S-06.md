<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/07/13/gsoc-ech8s-firmware-week-6/)    
作者: lynxis    
翻译: mytbk    

这周，我看了下EC H8S和PMH4的通信。PMH4(很可能是电源管理中心)是一个照顾电源控制的ASIC.它控制了谁得到电源，谁得不到。它不做任何的高阶工作，更像一个大的逻辑门(logic gatter).PMH4有从几个从不同的电源线路(power rail)和芯片的电源针脚的输入。在输出端，它控制着一些电源线路。它也可以复位H8S.PMH4也从一些芯片知道主板在哪个电源状态(ACPI S0,S4,S5).它不做任何高阶工作。它更像一个大逻辑门。在任何电源线上没有ADC.

PMH4通过4根针脚连接到H8S上。~OE LE DATA CLK.  
[图片](http://i0.wp.com/blogs.coreboot.org/files/2015/07/pmh4_connector_t40.jpg?resize=300%2C169)

我连接了一个在SPI嗅探模式(SPI sniffer mode)的总线直通车(buspirate)来调试这个协议。但是输出看上去有点奇怪。从PMH4到H8S(MISO)没有数据，并且数据突发地来。为了更多的了解这个协议，我用了一个数码示波器(digital oscilloscope).  
[PMH4示波器](https://own.fe80.eu/index.php/s/w7uLLz7Qh1AvLHs)

这协议看起来不像SPI.LE在每个传输后变低，~OE只是高，时钟和数据就传输数据。有时，当H8S得到一个中断时，时钟暂停一段时间，然后继续传数据。时钟在400kHz左右。

我通过示波器确认了协议，但还是没有从主板得到任何信号。没有风扇，没有其他东西。一定有比这单一的传输更多的东西。可能这块主板被损坏太大了。我的修改过的板子在我得到的时候已经坏了。有一个松散的连接和卡总线(cardbus)有关。也许这是我不知道的一个问题。

我有两块带有两个用于PMH4的连接的板子。为什么不用OEM的作为对另一块板的开始帮助呢？
[T42提供了一些开始的帮助](https://own.fe80.eu/index.php/s/1WPi7P3AIKBS7b7)

我觉得PMH4做了它该做的东西。H8S有一个数字-模拟转换(digital-analog-converter)针连到视频亮度。但是我还没有实现它。但是我不觉得设备引导了，因为CPU和芯片组都没有产生热量。OK,也许它做了，我只是用我的手指做温度计。一个热像仪(thermal camera)会有帮助。我会为此借一个热像仪。

有很多我忽略了的针脚，如A20针。在特定的时间系列会不会有一些关系呢？

### 下一步做什么？
* 用MSP430或者Stellaris ARM构建一个用于PMH4 XP的小的协议嗅探器
* 在引导加载器上取得进度
* 找个方法重新刷回OEM的H8S固件
* 找个方法通过OEM刷写工具刷我的引导加载器

我对引导加载器的要求是
* 通过XMODEM做UART刷写
* 一个简单的UART shell
* I2C作为恢复(recovery),同时也作为shell

I2C针脚比H8S UART更容易找到和修改。我还不肯定H8S应该作为主设备还是从设备，它应该用什么地址。多个？UART tx正在工作，rx是一个将要做的任务。

### PMH4/PMH7/Thinker通信
在新的主板上(>=X60,T60,...)PMH接口变了。它们合并了LPC接口和XP接口进一个SPI之上的协议。同时新的PMH也被用作GPIO扩展。
[PMH4/PMH7/Thinker通信](http://i2.wp.com/blogs.coreboot.org/files/2015/07/pmh4_pmh7_thinker_communication.png)
