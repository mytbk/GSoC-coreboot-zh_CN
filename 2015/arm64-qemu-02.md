<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/06/03/gsoc-coreboot-for-arm64-qemu-week-1/)
作者: Naman  
翻译: mytbk  

我上个星期在做QEMU移植的架构。我尝试深入ARMv8的内存映射的本质，然后明确地描述了一个初始的ARMv8移植的内存映射结构。之后，我开发了一些代码。最具挑战性的一方面是在现有的QEMU-ARMv7移植和chromium的基础ARMv8(foundation-armv8) patch(现在已经不推荐)之间移动，并且提取需要的修改。我现在的工作是在QEMU-ARMv7上构建，从64位需要的修改的基础ARMv8的一些方面获得灵感。

然后我继续开发默认的将会在我们的模拟中使用的`mem_uart`.在写完了这个新的移植的主干之后，Marc建议我push到gerrit并且在那上面寻找一些review.这是和我去年开发策略不同的一个重要的改变，当时我在本地做了所有的开发，然后把我的最终结果push到gerrit上。今年，我会用一个更动态的方法，用一个持续的复查-修改开发周期(review-and-modify development cycle).

我这周的计划涉及到开始构建到现在为止写好的固件。我将会在QEMU加载构建好的固件，并尝试在控制台(console)获取一些输出，最终让qemu-debug跑起来。
