<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](https://blogs.coreboot.org/blog/2016/06/06/gsoc-better-risc-v-support-week-2/)    
作者: jneuschaefer    
翻译: mytbk    

上周，我更新了[SPIKE](https://github.com/riscv/riscv-isa-sim)(到commit [2fe8a17a](https://github.com/riscv/riscv-isa-sim/commit/2fe8a17abf9a3329f5314af89bce99385e5a16f6)),并熟悉了老版本和新版本的区别：

- 宿主-目标接口(Host-Target Interface,HTIF)不再通过mtohost和mfromhost这两个CSR访问。你需要定义两个[ELF](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format)符号(tohost和fromhost).通常这是通过声明两个用这些名字的全局变量来做的，但因为coreboot构建系统不会原生地产生一个ELF文件，这会变得有点复杂。    
- SPIKE没有实现经典的[UART](https://en.wikipedia.org/wiki/Universal_asynchronous_receiver/transmitter).    
- 内存布局变了。默认的入口点现在是0x1000,这是SPIKE放置一个小ROM的地方，这个ROM会跳到模拟RAM的起始位置0x80000000.一个运行coreboot的方法是在0x80000000上加载，但接着它就不能捕获异常了：异常向量在0x1010.
- 在SPIKE的引导ROM里面，还有一个基于文本的“[平台树](https://github.com/riscv/riscv-isa-sim/blob/7e5c1b420d0b332d6663a47182f9a472e400f663/riscv/sim.cc#L172)”，它描述了安装在上面的周边设备。

你可能会问：“为什么coreboot需要一个串行控制台？”coreboot用它记录它做的每件事（在一个可配置的细节级别上），并且对调试和开发非常有用。

我决定[实现](https://github.com/neuschaefer/riscv-isa-sim/commit/33e5b6f446cb21f54a04f6b7aec3cd2d81ab114f)一个最小的，[8250](https://en.wikipedia.org/wiki/8250_UART)兼容的UART，而不是折腾HTIF的问题。我还没做好，但目标是使用coreboot现有的8250驱动。

### 这周的计划

这周，我会重写bootblock和CBFS代码来让它能在RISC-V的新内存布局上工作，并确保SPIKE UART可以在coreboot的8250 UART驱动下工作。引导Linux可能还需要花一些时间。
