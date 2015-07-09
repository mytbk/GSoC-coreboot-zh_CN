<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/06/18/gsoc-end-user-flash-tool-week-3/)    
作者: dmitro    
翻译: mytbk    

在第3周，我做`bios_extract`工具的整合。我分析了代码，明白了一些，想：“很好，这很快很简单，我只要做一些修改就行了。”是这样的吗？不完全是。

在分析之后，我知道我需要做三件事：
* 改变主函数为一个我可以从GUI调用的函数
* 重定向日志到GUI
* 提供选择用于存放提取出的独立模块的目录的功能

我实现了它并且决定最好的解决方法是打包目标文件到静态库。我编译并和我的应用链接，然后我尝试提取BIOS镜像然后——BAM!——段错误.Hmm,我没有改任何提取逻辑的东西——段错误，段错误，段错误。我还原了几乎所有的修改——还是段错误。我又下载了`bios_extract`并尝试先从未修改的代码创建目标文件，然后构建标准的`bios_extract`应用，然后一个一个地应用我的修改。我不带任何修改地编译，尝试跑`bios_extract`然后——段错误。我尝试用提供的Makefile编译——它work了。Whoops,我漏检查Makefile的内容了。这吸引了我的注意：

```
CFLAGS ?= -g -fpack-struct -Wall -O0
```
`fpack-struct`?这是什么巫术？我Google了它。Aha!明白了！这个编译选项(flag)把所有的结构体成员无洞(hole)地打包到一起，从而没有用结构体对齐。现在很明显为什么我段错误了，就算代码是一样的，因为结构体成员间不同的空间，工作起来不一样。从这刻开始就快速而且简单了:)

所以，`bios_extract`已经集成了，选择ROM文件，选择输出目录并在此提取子模块成为可能。当然了，`bios_extract`的日志输出被重定向到GUI.这很好，我可以用这周剩下的时间搞`libflashrom`,我的SOIC夹子还没到，所以我还不能测试操作相关的函数，但是已经有我在之前已有的`libflashrom`的patch的修改的反馈了，于是我可以开始改进它——对review表示大大的感谢！
