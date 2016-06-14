<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](https://blogs.coreboot.org/blog/2016/06/07/gsoc-multiple-status-registers-bp-otp-support-week-1-and-2/)    
作者: hatim    
翻译: mytbk    

Hi,我是来自印度的Hatim Kanchwala (hatim on IRC).我是今年GSoC做flashrom项目的学生。Stefan Tauner (stefanct)和David Hendricks (dhendrix)会指导我（非常感谢有这个机会）。我的项目的中期前部分有3个子项目——多状态寄存器(multiple status registers),块保护(block protection)和OTP支持。每个项目都处理SPI闪存芯片。

在写这篇博文的时候，flashrom支持超过300种SPI闪存芯片。大约10%有多状态寄存器（大多数有两个但有一个有3个）。几乎所有的都有某种块保护。大约40%有某种OTP或者安全寄存器。一个BP(Block Protect, 第一个状态寄存器)和SRP位(通常是第一个，但有时会是第二个状态寄存器)的状态寄存器组合在效果上决定了保护的范围和类型。一些在BP位之外，有TB(Top/Bottom)位。一些还有一个CMP(Complement Protect,第二或第三个寄存器)位，对可用范围增加了灵活性。少数芯片有WPS(Write Protect Scheme,第二或第三个寄存器)位，定义了哪种访问保护方案正在被使用。有安全寄存器的芯片有相应的LB(Lock Bits, 第二个状态寄存器)位，它是一次编程(one-time programmable)的，并且在设置的时候，让相应的安全寄存器只读。带有分离的OTP扇区(sector)的芯片会有操作码(opcode)来进入/推出OTP模式，并且在OTP模式中，可以使用通常的读取，页编程和扇区擦除操作码。

在之前，flashrom只能读写第一个状态寄存器。对于写操作，如果保护类型允许，所有的块保护位会被清除（这个配置对应块保护）。在清除了之后，flashrom无法恢复BP位的配置。[Chromium OS分支的flashrom](https://chromium.googlesource.com/chromiumos/third_party/flashrom/)有原地的锁定/解锁块访问保护的支持。在特定家族的芯片中，已经有很多工作完成了，但是他们还在让它变得通用化。对于有OTP支持的芯片，flashrom只是简单地打印一个警告。

在这两周中，我认真地看了大约5到6打数据表和多状态寄存器，块保护和OTP/安全寄存器的开发模型。我通过邮件列表和指导老师及社区讨论([主题链接](https://www.flashrom.org/pipermail/flashrom/2016-May/014658.html))架构更改和这些模型相应的用况。为了证实这些想法，我分别写了原型代码。在这个过程中，Stefan介绍给我一个强大的工具，[Coccinelle](http://coccinelle.lip6.fr/).这个工具可以在保持安全的情况下对大型结构的闪存芯片做出更改。作为学习现有flashrom架构的副产品，我有机会通过git log探索flashrom的历史——flashrom从它在``coreboot/util``卑微的开始到``flash_and_burn``到``flash_rom``最终到今天的flashrom的进化！

我接下数周的大目标会是结束剩下的一两打数据表，推敲这些模型并把原型代码转化为值得merge的代码。跟随着架构的变化，我会更新现有的芯片来用上新的架构，并支持一堆新的芯片，并最终在真实的硬件上测试。

谢谢。再见！
