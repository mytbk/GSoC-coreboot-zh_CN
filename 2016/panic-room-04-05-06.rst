[GSoc] Panic Room 第4/5/6周
===========================

- 原文地址: https://blogs.coreboot.org/blog/2016/07/21/gsoc-panic-room-week-456/
- 作者: Antonello Dettori
- 翻译: mytbk

这几周你都怎么了？
------------------

我的滑稽改变自我，我必须承认我有罪，我从我原有的时间线偏离了。（如果你看了我之前的帖子会很明显）

从我上篇博文开始，我主要关注 region_device API. 我尝试走进我第3周的计划来继续从selfload代码梳理出 rdev_mmap_full().（在之前的博文中提到的）

我想我高估了在那个项目上我可以完成的东西。我尝试修改现有的在coreboot里面用的LZMA API来让它容易地从内存/芯片加载每块数据并逐步解压它。

理论上它不应该很困难，实际上解压代码很难看并且看起来是一个定制版本的LZMA SDK代码。

我，并不是很快地，意识到我没有时间去深入学习它，所以我很快地扔掉了这个想法。（在GSoc结束后这会是一个好的项目。）

我还花了一块时间在移植cbmem工具中的时间戳读取功能到coreinfo payload中。(`commit`__)

__ https://review.coreboot.org/#/c/15600

现在已经可以在没有正在工作的操作系统的环境下，读出coreboot启动序列花了多少时间。我需要测量在 rdev_mmap_full 提交后，是否有任何启动时间的改进。（尽管还没有做那个...）

那么你现在在做什么呢？
----------------------

我正在从coreboot移植region_device API到libpayload,现在正在替代所有从它的前身cbfs_media提供的功能。

今天我完成了（之前我的代码不得不整体地重做）替代libpayload里面的老API并开始转换唯一一个用着cbfs_media的payload: depthcharge,用来引导ChromeOS的payload.

希望这是个没有痛苦的过程。

下一步是什么？
--------------

于是，在我完成当前的patch之后（可能在周末），我需要重新集中精力在我真正的时间线上。

我计划在（不久的？）未来最终发行的新版本之前，测试当前的FILO主分支来检查可能的问题和障碍。

这会很有用，因为当前的稳定版一点也不稳定，它甚至无法编译，master也是。（除非你打上了这个 `patch`__ ）

__ https://review.coreboot.org/#/c/14952/

这也同时是第一个包含了新的flashupdate命令，并且允许做一些有趣的事情（如从一个坏掉的coreboot刷写/配置恢复的路径）的发行版本。

进而，我会让SerialICE固件端的代码合并到主coreboot代码库中（ `commit`__ ），所以我计划用我还有的时间解决当前的问题。

__ https://review.coreboot.org/#/c/14504/

下次再见！
