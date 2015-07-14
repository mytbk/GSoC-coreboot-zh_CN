<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

原文地址: [http://www.coreboot.org/pipermail/coreboot/2015-July/080120.html](http://www.coreboot.org/pipermail/coreboot/2015-July/080120.html)
翻译: mytbk

亲爱的coreboot社区,

从“发行“coreboot '4.0'到现在已经超过5年了。上个发行版本标志着一些非常重要的在LinuxBIOS v3建立了原型的里程碑，像coreboot文件系统(CBFS),Kconfig支持，和（严格）分离的设备树，构建逻辑和配置。

从那以后，有很多重要的原始开发，例如对很多新架构的支持(ARM,ARM64,MIPS,RISC-V),和相关的架构修改，如到非内存映射SPI闪存的访问，通过cbmem控制台更好的在运行时查看coreboot内部，时间戳收集，代码覆盖支持。

可以很清楚的看出，一个新的发行版本来迟了。在我们的新的发行版本的过程慢慢成型的时候，我决定给一个随机的提交并称它为'4.1'.

这个发行版本自身发生在一个任意的时间点，但是将会成为其他在下描述的需要一个起点开始的活动的起点。

未来的发行版本会更频繁地发生，并对发行的状态有更多的保证，例如在主板可被测试时有一个冷却状态。我计划每3个月创建一个发行版本，从而每两个发行版本间的改变不会太太密集。

在coreboot 4.1发行的时候，你收到一个公告(这封邮件)，一个git标签(4.1),和在[http://www.coreboot.org/releases/](http://www.coreboot.org/releases/)的tar归档，里面有coreboot的源码和可分发的二进制blob.

从coreboot 4.1开始，我们会维护一个高级的changelog和'flag days'文档。后者会提供一个简明的列表，里面有进入coreboot的修改，它们要求芯片组或主板的代码的修改，使它能在最新的上游coreboot中继续work.

到现在为止，我会继续努力，但我会高兴地和他人分享文档义务——这是一个跟踪事情，学习这个项目和它的设计，以及各种内部结构的机会，同时又能为项目做贡献而无需写代码。

请联系我(比如通过email或IRC)如果你感兴趣，我们会制定出在这上面合作的方法。

这个过程会让coreboot用户跟随发行版本，如果他们需要一个构建在一个更加静态的基础之上，同时通过提供升级的文档使得跟随新的开发更加简单。

因为从一个滚动发行模型离开对coreboot来说是一个新的尝试，事情会有点艰难，但是我会对所有在发行过程中遇到的问题提供支持。

Patrick
