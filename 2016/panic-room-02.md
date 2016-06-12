<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](https://blogs.coreboot.org/blog/2016/06/11/gsoc-panic-room-week-2/)    
作者: Antonello Dettori    
翻译: mytbk    

### 上周怎么样？

说起来有点没预料到。

我花了主要的时间在ELF(Executable Linkable Format)[规格](https://refspecs.linuxfoundation.org/elf/elf.pdf)上面。我用这新得到的知识来改进cbfstool工具，让它可以提取CBFS中的payload为ELF，而不是SELF.

为了做到这点，cbfstool要做几件事：

- 从coreboot镜像中提取payload    
- 解析SELF payload中的段表(segment table)，从而找出有多少，有哪些段是存在的    
- 用elf_writer API来生成一个符合标准的ELF头    
- 从每个段提取内容，并复制到相应的ELF节头(section header)，并根据这个来配置    
- 在节表填好之后，用elf_writer生成程序头表，并写到最终的ELF文件中

最后的结果会允许，例如说，容易地把一个CBFS中的payload移动到另外一个CBFS里面，而不用重建这个payload,coreboot ROM，或者搞乱构建系统的配置。

现在，这个实现还没完成，但它已经对coreboot中很多常用的payload工作得很好了，例如SeaBIOS,coreinfo,nvramcui还有很多其他的。主要的障碍在于让GRUB payload工作，并增加一个方法来处理压缩过的payload的提取。

### 等等！你不是在做SerialICE吗？

你是很最跟究底的，不是吗？

是的，我的主要目标还是继续集成SerialICE和coreboot.

不幸的是，这周有一些让我的工作停下来的事。第一是我唯一的夹子坏了，现在我的目标，联想X60停止了工作，我没办法刷它的BIOS芯片了。我已经下单了一个替代品，但它可能需要花一周多点的时间到达。

现在，我的指导老师(adurbin)和蔼地指出了上述让我在等待的时候，还能忙于工作的任务。

### 你下周的计划是什么？

我计划完成实现上述功能，并测试所有剩余的payload.

幸运的是，我可以开始看看其他的一些我指导老师给我提出的任务。

这是我今天要说的了，下周再见！
