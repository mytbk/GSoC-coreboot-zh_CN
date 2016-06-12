<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](https://blogs.coreboot.org/blog/2016/06/02/gsoc-panic-room-week-1/)    
作者: Antonello Dettori    
翻译: mytbk    

### 你是谁？

大家好，我是Antonello Dettori (avengerf12 on IRC). 我是现在在做改善SerialICE工作的学生。

### 你正在做什么？

我很高兴你问这个问题。

在我说我在做什么之前，我先说几句关于SerialICE的东西。SerialICE是一种用于逆向OEM BIOS，从而了解coreboot需要正常在目标机器上执行的初始化过程，的主要工具。

我原来提议的想法是做这些：

- 往coreboot合并SerialICE的功能    
- 允许在没有正在运行的操作系统的环境下，提供刷写正在运行coreboot的目标机器的方式

提议写好之后的几个月，情况有点变化了，部分目标已经有一些优秀的coreboot社区贡献者做了。不过我还有足够的活干，我的指导老师已经指出这个项目的一些我可以投入时间的方向。

### 你第一个星期做得怎么样？

Oh boy，你已经到那里了，不是吗？

我在这个项目上上手有点晚了，因为从这星期开始，我才开始真正欣赏所有这些让coreboot和SerialICE跑起来的工作。因此，我现在还在漫长的学习过程之中，但不用担心，在这前方正在取得进展。不幸的是，这也意味着我还没有真正达到我目标的代码。

### 你下周要做什么？

我会，很有希望地，成功地把我的SerialICE学习环节做好，并最终写出一些真正（可能有用）的代码。

特别地，我希望修复关于coreboot初始化目标的过程中发生的，关于管理缓存和与其相关的寄存器的冲突，但SerialICE是作为romstage使用的。

这是我现在所有要说的了，下周再见！
