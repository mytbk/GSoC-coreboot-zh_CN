<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](https://blogs.coreboot.org/blog/2016/06/13/gsoc-better-risc-v-support-week-3/)    
作者: jneuschaefer    
翻译: mytbk    

上周，在升级了GCC并注释掉过时指令和CSR(最值得注意的是eret和HTIF CSR)的使用后，我注意到coreboot在它尝试访问任何全局变量的时候崩掉了。这是因为coreboot构建系统认为coreboot会存在于地址空间开始的位置附近。

我找到了``spike-riscv/memlayout.ld``，并[调整了起始偏移地址](https://review.coreboot.org/#/c/15149/1/src/mainboard/emulation/spike-riscv/memlayout.ld)。但接下来我得到了一个链接器错误：

```
build/bootblock/arch/riscv/rom_media.o: In function `boot_device_ro': [...]/src/arch/riscv/rom_media.c:26:(.text.boot_device_ro+0x0): relocation truncated to fit: _RISCV_HI20 against `.LANCHOR0'
```

我折腾了一下起始地址，并注意到0x78000000之下的地址可以工作，但如果我选择了一个太接近0x80000000的地址，就会坏掉。这是，事实上，因为指向全局变量的指针是由像这样的指令序列决定的``lui a0, 0xNNNNN; addi a0, a0, 0xNNN``.在32位的RISC-V下，lui指令把它的参数加载到一个寄存器的高20位，而addi加一个12位的数。在64位的RISC-V系统下，``lui a0, 0x80000``把0xffffffff80000000加载到a0,因为这个数是[符号扩展的](https://en.wikipedia.org/wiki/Sign_extension).

在反汇编了一些coreboot和[RISC-V proxy kernel](https://github.com/riscv/riscv-pk)的.o文件后，我最后注意到我必须[用--mcmodel=medany编译器选项](https://review.coreboot.org/#/c/15148/1)，它会让数据访问使用PC相对的方式。

既然coreboot最后跑起来了并可以访问它的数据段，我完成了调试我上周承诺的[UART块](https://github.com/riscv/riscv-isa-sim/pull/53)。coreboot现在可以打印东西了，但它很快就停止运行了。

### 这周的计划

这周我要调试为什么coreboot挂了，并希望最终会让它再启动到"Payload not found"那行，它会在更老版本的spike下工作。

同时，Ron Minnich会[在几小时之后](https://calendar.google.com/calendar/event?eid=cWxhOXZnYWI5cmZoNW0zY2gwcTRvZ3Zxc2dfMjAxNjA2MTNUMTg0NTAwWiA2YjF1OGlxMTNqajhjcDZrZm9rcTR2bG8yMEBn)在San Francisco的coreboot会议上做一个关于RISC-V上coreboot的演讲。
