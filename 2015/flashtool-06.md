<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/07/22/gsoc-end-user-flash-tool-week-6/)    
作者: dmitro    
翻译: mytbk    

大家好！在第6周我在做这两件事：  
* 在T60上libflashrom的功能测试
* GUI改进——过滤和搜索支持硬件的列表

#### 测试libflashrom
为了测试，我用装有Macronix芯片的T60和树莓派。我用SOIC夹子连接芯片并将它连在树莓派SPI上。我需要几乎完全拆解我的笔记本，因为在T60上，BIOS芯片被一块必须移去的镁框封住了。对我来说简易地每次不拆解全部东西来访问芯片很重要，所以我移除了一部分覆盖了芯片的框。

![图片](http://i2.wp.com/blogs.coreboot.org/files/2015/07/t60_SOIC.jpg)

测试的函数有这些：
* `fl_flash_probe`: 函数返回合适的刷写上下文(flash context)如果我们提供一个特定的芯片作为参数，如果我们探测所有已知的芯片并且有多个芯片被发现(就像用Macronix芯片的联想T60)，正确的错误码会被返回(我同时需要实现一个方法去输出多个闪存芯片到GUI然后选择合适的一个，我会在下篇文章描述它).
* `fl_image_read`: 正确的数据会加载到缓冲区
* `fl_flash_erase`: 芯片已经正确地擦除
* `fl_image_verify`: 验证成功
* `fl_image_write`: 数据被正确地写到芯片上

#### 过滤和搜索支持的硬件
现在，通过选择过滤选项，如厂家，大小或测试状态，在列表上找到一个特定的芯片，主板或者芯片组已经成为可能。你也可以通过输入一个特定硬件的名字(或者部分名字)来搜索。我计划扩展这个界面并提供更多的过滤选项。我现在要实现更重要的特性，例如自动化建立一个可用的coreboot镜像的过程，还有检查硬件兼容性。

![图片](http://i2.wp.com/blogs.coreboot.org/files/2015/07/supported_hardware.png)
