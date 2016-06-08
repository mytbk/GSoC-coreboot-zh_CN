<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](https://blogs.coreboot.org/blog/2016/06/01/gsoc-better-risc-v-support-week-1/)    
作者: jneuschaefer    
翻译: mytbk    

Hi,我是Jonathan Neuschäfer (jn__ on IRC)，我今年的GSoC项目是改善coreboot对RISC-V平台的支持。[RISC-V](https://riscv.org/)是一个新的指令集架构(ISA)，它可以无授权费地实现，并且相对简单。

coreboot在2014年已经移植到RISC-V上了，并且从那时起收到了很多补丁，但是由于RISC-V的特权ISA规格说明(RISC-V Privileged ISA Specification,定义了中断处理和虚拟存储之类的东西)还在变迁之中，它变得又无法启动了。

我上周的第一个目标是在SPIKE,官方的RISC-V模拟器，上运行coreboot,并得到了一些控制台输出。我取出了[riscv-tools](https://github.com/riscv/riscv-tools)的commit 419f1b5f3 (当前master)并从这里构建了SPIKE.

当前coreboot中RISC-V binutils移植，是通过硬编码一些控制和状态寄存器(Control and Status Register)的数值，来面向一个新版的RISC-V特权规格的。在我修补了几个指令，并根据这个事实做了调整之后，我终于让coreboot启动至它会跳到我指定的payload的地方。

所有的补丁可以在[gerrit的riscv主题](https://review.coreboot.org/#/q/topic:riscv+after:2016-05-26+before:2016-06-30)下找到。

### 这周的计划

这周我会更新SPIKE到支持将要到来的特权规格1.9的版本，它[会在几周后发布](https://groups.google.com/a/groups.riscv.org/forum/#!topic/isa-dev/-7jrvxevEIM)。这样我就不需要修补指令，因为GCC和SPIKE编码它们的方式不同。此外，我会尝试在coreboot之下，让Linux在SPIKE下引导起来。
