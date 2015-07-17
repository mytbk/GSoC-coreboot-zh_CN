<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/07/13/gsoc-coreboot-for-arm64-qemu-week-6/)  
作者: Naman  
翻译: mytbk  

这周我做完成构建的工作并梳理所有被它强加的复杂问题。就像我在上篇文章说的那样，我面对了一些和为这个移植启用SMP相关的问题。我通过添加一个声明了`smp_processor_id`的汇编文件，然后通过设置正确的寄存器来定义，解决了这个问题。我不得不做一些关于ARM64细节的背景阅读。[这个文档](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.ddi0488c/BABGDAIC.html)给我提供了我需要的信息。

下面是另外一个故障。在构建期间，`mmu_enable()`和`arch_secondary_cpu_init()`的函数调用在所有阶段都发生，但是这些函数的定义只在ramstage被编译。所以这个给出了反复发生的错误，因为编译器没法找到这些定义。在我尝试梳理这个问题的时候，我碰到了chromium源码树了的某个东西。有一个处理一些这个问题的[patch](https://chromium-review.googlesource.com/#/c/228389/),和我的问题很像。我不得不cherry pick并应用这个改变。

在调试和梳理更多问题之后，我最终让它成功地构建了。

> coreboot.rom: 4096 kB, bootblocksize 37008, romsize 4194304, offset 0x90c0 alignment: 64 bytes, architecture: arm64
> Name                            Offset     Type         Size
> fallback/romstage               0x90c0     stage        12108
> fallback/ramstage               0xc080     stage        17768
> config                          0x10640    raw          2034
> revision                        0x10e80    raw          577
> (empty)                         0x11100    null         4124312
> HOSTCC     cbfstool/rmodtool.o
> HOSTCC     cbfstool/rmodule.o
> HOSTCC     cbfstool/rmodtool (link)

完整的构建可以看[这里](http://pastebin.com/qFzBXYGL).我在这之后尝试在QEMU上引导。
```
$ qemu-system-aarch64 -machine type=virt -nographic -bios ~/coreboot/build/coreboot.rom
```

这没有给出任何输出，这意味着可能我不得不在UART的启动上给一些修改。我尝试通过在bootblock添加一些printk,就在`console_init`完成之后，来调试。这个过程正在进行，我希望能做完。问题另外一方面是bootblock的初始化。`src/arch/arm64/armv8/bootblock_simple.c`需要一个合适的`bootblock_cpu_init()`.这是我接下来的日子要做的另外一件事情。
