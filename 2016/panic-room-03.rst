[GSoc] Panic Room 第3周
=======================

- 原文地址: https://blogs.coreboot.org/blog/2016/06/21/gsoc-panic-room-week-3/
- 作者: Antonello Dettori
- 翻译: mytbk

上周发生了什么？
----------------

在很多次修补和抓bug之后，我完成了编写和测试加入cbfstool用于在SELF和ELF之间转换的代码。

`代码已经合并了。 <https://review.coreboot.org/#/c/15139/>`_

一个最麻烦的事情是让GRUB在转为ELF之后工作，而其他payload是能很好地工作的。

看起来它是唯一（我测试）使用了多于2个段来描述程序的内存映像的payload.这同时也发现了一个在elf_writer代码中可能在主要的payload都只有一个段的情况下，从来没被触发的bug.(`commit <https://review.coreboot.org/#/c/15215/>`_)

我同时也收到了我用于对联想X60目标机器做替换的机器，所以我可以回到SerialICE这部分项目的轨道上来。

你下周的计划是什么？
--------------------

我正在调查一个使用最近的整合SerialICE到coreboot的 `patch <https://review.coreboot.org/#/c/14504/5>`_ 的时候，在QEMU和目标之间进行串口通信的bug.

我同时也在研究一些和selfboot.c和区域API相关的工作。目标是避免在执行的时候一次全部加载整个payload，并允许在需要的时候让不同部分的payload被加载。

我有希望可以在下周之前完成所有这些工作。（有时我明显感觉过于乐观）

什么？！你这周没有不幸事故吗？
------------------------------

.. class:: strike

真的非常有趣的是，我的装备不断在坏...这周轮到了我的树莓派。它不再启动了。幸运的是我有一块总线直通车和一个BeagleBone Black来做SPI刷写，所以一切都好。

EDIT:划掉它...显然你只需要等一会儿让它重启...奇怪。

下周再见！
