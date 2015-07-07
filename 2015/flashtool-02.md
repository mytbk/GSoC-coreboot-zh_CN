<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/06/12/gsoc-end-user-flash-tool-week-2/)    
作者: dmitro    
翻译: mytbk    

这周我开始添加新功能到libflashrom.我加了3个函数，目的是返回支持的硬件列表：
* `int fl_supported_flash_chips(fl_flashchip_info_t *fchips)`
* `int fl_supported_boards(fl_board_info_t *boards);`
* `int fl_supported_chipsets(fl_chipset_info_t *chipsets);`

例如，要获得支持的主板列表，你可以创建一个大小合适的`fl_board_info`结构数组，然后传给`fl_supported_boards(fl_board_info_t *boards)`,并且你让它填上数据，但是你怎么知道你的数组应该有多大呢？

有另外3个函数返回支持的某种硬件的数量：
* `int fl_supported_flash_chips_number();`
* `int fl_supported_boards_number();`
* `int fl_supported_chipsets_number();`

在libflashrom的工作仍在进行，但是你可以看到，一些变化已经做好，所以我想发一个patch去做个初始的review,了解一下我是否在往一个好的方向走，是个不错的主意，于是我发了一个patch到flashrom的邮件列表。

在这些函数的作用下，我可以扩展我的coreboot用户端刷写工具的GUI部分，并且加一[窗口](http://s7.postimg.org/ghpd95p6j/Supported_hardware.jpg)显示支持的芯片，主板和芯片组列表。

我同时也开始写GUI部分的单元测试，我想用Google测试框架，但最后决定用QtTestLib,因为它提供了简单的对于Qt的信号(signal)和槽(slot)的introspection.

在这些工作之后，我更加熟悉flashrom的代码，但是还有很多去工作和学习，现在困难但令人激动的部分来了——测试并修复和操作相关的函数，比如读取，验证，擦除，刷写。有一些之前在libflashrom用到的flashrom的函数已经是静态的或者不再存在了。我买了台ThinkPad T60和SOIC测试夹用于测试，T60已经到了，于是让我们开始拆解它吧。
