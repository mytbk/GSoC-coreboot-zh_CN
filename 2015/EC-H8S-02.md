<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/06/10/gsoc-ech8s-firmware-week-2/)    
作者: lynxis    
翻译: mytbk    

上星期有点沉闷。我做了些重焊接(resoldering).针脚P90没有和需要用来进入闪存引导模式(flash boot mode)的3.3V连接。它被焊接到串行电位切换器(serial level shifter)MAX3243的VCC上。在用万用表搜索一个更好的电源几分钟后，我决定用H8S附近的3.3V.现在是一个穿过板子的非常长的电线。

现在看看，这工作得多好呢？什么都没有:(.用电压表(voltmeter)重新检查一下，发现P91(/SUS\_STAT)的另外一个问题。当用一个1k电阻(resistor)连接SUS\_STAT和3.3V的时候，电压表显示0.04V.这表明它被别的东西驱动到0V.我的希望是芯片直到上电(powered)都不驱动它。但是它正驱动它到0V.SUS\_STAT是什么？SUS\_STAT可以被用作LPCPD(LPC power down)并用来通知设备进入一个低功率状态。挂起状态(Suspend Status)是低电位触发(active low)的，意味着所有的设备都应该在低功率模式。

我现在应该做什么？我需要那条线上有3.3V.

有几个解决方法：
* 移除1k并且烧了它(burn it to death).但是这很可能干掉芯片或者至少这根特定的针脚或者多个针脚
* 剪掉针脚
* 焊下(desoldering)的时候把针脚往上弯
* 焊下整个芯片之后弯曲，重焊接
* 用一个插座替换芯片(昂贵而且罕见)

这个决定不好做，特别是因为大多数的事情我都没做过。这卡了我一段时间，直到Peter帮助了我，他把一个针脚往上弯了。谢谢！

下周的里程碑还是刷新EC——自从上星期以来同样的目标。所以时间安排会有点混乱。可能我可以尽快并快于一周达到另一个周计划。

因为我有点卡在那上面，我又看了下ebay并买了一个有H8S/2633的开发板。2633比联想笔记本用的2199系列新一些。这个开发板应该会在一周内到(but atm it's in german customs).这样的开发板很难在一个好价上拿到。全新的板子几百欧元或美金起，比如调试器E10(USB设备)要大约1000欧元，它还只是个傻傻的USB设备。我已经在ebay上买了个E8,上一代的调试器，但是它不能调试这芯片，只有用瑞萨(Renesas)的软件/IDE刷写它们。

除了我的项目之外，我做了一些其他coreboot的工作。我帮助Holger Levsen在[reproducible.debian.net](https://reproducible.debian.net/coreboot/coreboot.html)创建了一个可重现的构建工作。更多关于可重现构建在他们的[wiki](https://wiki.debian.org/ReproducibleBuilds)上。为了改善可重现性，我创建了两个patch[#10448](http://review.coreboot.org/#/c/10448/)[#10449](http://review.coreboot.org/#/c/10449/).它们在没构建payload的情况下清理了coreboot的可重现bug,现在大多数的目标已经可重现。为Holger Levsen的这些工作表示非常的感谢！
