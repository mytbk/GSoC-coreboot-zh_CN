<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/05/28/progress-gsoc-week-1/)  
作者: lynxis  
翻译: mytbk  

我的项目的第一周是在一块开发板上工作。开发板的意思是，我有串口通讯，并且我可以刷写新的固件，芯片和整块主板不引导。芯片是一个H8S 16位微控制器，有64kb~128kb EEPROM,有不同的封装:BGA和TQFP. BGA的意思是针脚在芯片的底部，TQFP的针脚在周围。TQFP比较好hack,但多数现代的ThinkPad用BGA.但是T40,T42用TQFP封装。一个朋友捐了他的老T42给我，非常感谢！现在，有一台可以hack的T42,我可以用它开始创建我的开发板。像很多其他的微控制器一样，这块芯片有一个可编程的引导器(bootloader)在ROM里面(叫rom loader).引导器可以引导至不同的状态，由5根针配置(MD0 MD1 P90 P91 P92). P90到P92在MD0和MD1在特殊的引导状态下才能被读取。

读过文档之后，我发现针脚要符合这些电压来选择flash引导模式(flash boot mode):
MD0-MD1 = 0V,  P90-P92 = 3.3V

除此之外，我们需要一些额外的线连接到这些针脚：
* /RES - 重置,低电位触发(active low)
* UART RX - 串口通讯
* UART TX - 

现在就有趣了。MCU(微控制器单元)可以用一根针脚做不同的用途，由PCB的设计者决定。那些针脚叫多功能针脚。幸运的是，我们不会被无法访问的针脚封锁。读了更多的文档之后，在板上使用万用表(multimeter)，我发现/RES,RX,TX,MD1需要焊接(soldering)，但是可以很容易地访问到。MD0已经在一个好的状态。P90通过一个电阻(resistor)接地，但是我需要它在3.3V.

让我们找到这个电阻，焊接3.3V...Mhh.tricky!3小时后我发现它在板上，藏在PMH4(2代EC/GPIO扩展器)下面。非常的不寻常。

P91叫做/SUS. 挂起，低电位触发，可以被多个控制器驱动(芯片组+H8S). 因为我们之后在项目中要引导Linux在主CPU上，我们不能干掉芯片组。我加了一个针脚连接器在这根针脚上。

最后一根针P92连接在SuperIO UART的电位切换器(level shifter)MAX3242上。我不得不拔下芯片，因为P92被这个电位且换器驱动。

在EC的附近是两个连接到一个I2C总线的测试点(testpoints).我也焊接了这些，因为一个I2C会有用。

1 P91
2 GND
3 md1
4 /RESET
5 RX
6 TX
7 I2C SDA
8 I2C SCL
+
1 patch cable with a 3.3V + 1k Resistor (for P91).

到现在还好。但是，它不work.一些针脚没有正确的电位。P92没有3.3V.为什么呢？

P92通过一个电阻拉高到TTL切换器(TTL shifter)的VCC上。VCC没有上电(powered).我需要重新焊接到另一个某个地方的3.3V针脚，看看其他的电位。

PS. 一些工作在GSoC开始前就已经做了。我把第一部分的发在了我的[博客](http://lunarius.fe80.eu/blog/coreboot-t40-ec-soldering.html)上。
