<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/08/13/gsoc-end-user-flash-tool-week-7-8-9/)  
作者: dmitro  
翻译: mytbk  

大家好！

在第7,8,9周我做了:
* 收集硬件特定数据的函数
* 扩展libflashrom
* GUI改进
* 测试

#### 收集硬件特定数据
终端用户刷写工具的主要目的是提供一个简单的方法去构建和刷写coreboot ROM.为了达到这个目标，有必要收集硬件特定数据，比如:
* `lspci -nn`输出: 关于系统中所有PCI总线和设备的信息，可以识别一个显卡，厂商和设备代码
* 原厂BIOS的dump: 为了防止出错，做一份原厂BIOS的拷贝很重要，但不只是在这种情形，通常有必要使用从原厂BIOS提取出来的VGABIOS，如果系统中有特定的显卡或显示面板。最好做两次dump并检查文件是否相同(比如比较哈希)

#### 扩展libflashrom
有时当flashrom探测所有已知的芯片，可以找到多个芯片。我需要实现一个会返回所有芯片到GUI的函数。现在在这种情形下，可以通过在对话框点击一个合适的芯片来选择正确的芯片。
![图片](http://i1.wp.com/blogs.coreboot.org/files/2015/08/choose_chip.png)

#### GUI改进
我决定改一下这个app的视觉设计。有一个新的(但对这个项目最重要的)标签——'Auto'.在这个标签页中，可以收集硬件特定数据，它可以用到自动构建coreboot镜像和刷写的过程中。我也决定把编程器选择的组合框(combobox)从'Flash tab'移到主程序窗口，并添加一个参数编辑文本框。

#### 寻找测试者
下周GSoC要结束了，应用基本完成(但也会在GSoC之后改进和扩展)，所以这是做些测试的时候了！如果你们有人可以帮助我的花，我会非常感谢。如果你有联想T60和外置编程器就最好。首先，有必要收集一些硬件特定数据，然后可以检查应用是否根据这些数据建立可用的coreboot镜像。所以这不只是关于测试，也是关于做一个更大的硬件配置的白名单，让更多用户容易地在他们的硬件上刷上coreboot!

请通过以下方式联系我:
* IRC: #flashrom #coreboot: lukaszdm
* e-mail: lukasz.dmitrowski@gmail.com

十分感谢！
