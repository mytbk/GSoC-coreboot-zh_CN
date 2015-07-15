<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/07/06/gsoc-ech8s-firmware-week-5/)    
作者: lynxis    
翻译: mytbk    

T40的[LED闪了](https://own.fe80.eu/index.php/s/V8bzLGTxYDjnzp0)!工具链还是有点tricky.我在用Debian的[gcc-h8300-hms](https://packages.debian.org/jessie/gcc-h8300-hms)包，写了一个小链接脚本并从[Johann Gysin的LED发光器](http://www.jogy.ch/files/Circuit_Cellar_Design_Contest_H3210/H3210.zip)拿了启动的汇编例程。

现在我可以让LED闪了。但是引导主板呢？我会说这样就足够了：
* (!MAINOFF) = high
* FAN ON = high
* (!PWRSW_H8)用高脉冲

但这不够。同时，风扇也没有开始转。我会在这周尝试调试每个针脚并且为第二个EC(PMHx)焊接一些调试针脚到我修改过的T40和修改过的T42p.H8S和PMHx通过SPI交流，而H8S是主方，并且在软件中做bit banging SPI,因为它没有为此拥有一个硬件单元。我也会用这些针脚测试我的SPI实现。我会尝试重用一个开源的SPI实现。

我也问过自己在继续在EC上努力前，为T40移植coreboot是否是个好主意，但这又更难一些，因为T40使用一个用[TSOP40](https://en.wikipedia.org/wiki/Thin_Small_Outline_Package)封装的LPC/FWH闪存。另一个选择是把硬件改到一个已经有coreboot支持的主板，如X60/T60或X201.但在这些主板上访问刷写EC的8根针要难很多。

在换到另一块主板前，上电序列(powersequencing)一定要work,并且我需要一个健壮的恢复方式，因为当你刷一个新的固件干掉EC后，你就没有第二次机会了，除非你焊接很多次。Chrome EC通过拆分EC为两部分，解决了这个问题。一个只读的部分和一个可读写的部分。只有第二部分升级，而只读的部分至少可以引导设备。

在开始为Chrome EC开始H8S移植之前，我要有一个引导加载器。因为它可以改善开发速度。我想完成这个会比做完全Chrome EC支持会快很多，并且大多数的引导加载器代码可以在Chrome EC重用。

我也不是很确定Chrome EC是不是最好的解决方案。它的特殊用况是EC,这是完美的。但是文档(我觉得那不止[一页](https://en.wikipedia.org/wiki/Thin_Small_Outline_Package))和bugtracker都不是公开的。从而用它会变得困难。我也不确定Chrome EC会不会把我的H8S移植用到他们的代码仓库里面。
