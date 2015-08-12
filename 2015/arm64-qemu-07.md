<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/07/28/gsoc-coreboot-for-arm64-qemu-week-7/)
作者: Naman
翻译: mytbk

这是艰难的一周。在通过了coreboot的构建阶段后，我想我的工作应该会简单一些了。但我又遇到另一件事。

就像我在上篇文章说的那样，我在QEMU引导的时候没有得到任何输出。所以，第一件事是要让QEMU的monitor工作。在一些调试后，我能让QEMU的monitor工作，并在我的终端(stdio)上打印东西了。

这给了我以下信息：
> qemu: fatal: Trying to execute code outside RAM or ROM at 0x0000000008000000
>
> R00=00000950 R01=ffffffff R02=44000000 R03=00000000
R04=00000000 R05=00000000 R06=00000000 R07=00000000
R08=00000000 R09=00000000 R10=00000000 R11=00000000
R12=00000000 R13=00000000 R14=40010010 R15=08000000
PSR=400001db -Z– A und32
s00=00000000 s01=00000000 d00=0000000000000000
s02=00000000 s03=00000000 d01=0000000000000000
s04=00000000 s05=00000000 d02=0000000000000000
s06=00000000 s07=00000000 d03=0000000000000000
s08=00000000 s09=00000000 d04=0000000000000000
s10=00000000 s11=00000000 d05=0000000000000000
s12=00000000 s13=00000000 d06=0000000000000000
s14=00000000 s15=00000000 d07=0000000000000000
s16=00000000 s17=00000000 d08=0000000000000000
s18=00000000 s19=00000000 d09=0000000000000000
s20=00000000 s21=00000000 d10=0000000000000000
s22=00000000 s23=00000000 d11=0000000000000000
s24=00000000 s25=00000000 d12=0000000000000000
s26=00000000 s27=00000000 d13=0000000000000000
s28=00000000 s29=00000000 d14=0000000000000000
s30=00000000 s31=00000000 d15=0000000000000000
s32=00000000 s33=00000000 d16=0000000000000000
s34=00000000 s35=00000000 d17=0000000000000000
s36=00000000 s37=00000000 d18=0000000000000000
s38=00000000 s39=00000000 d19=0000000000000000
s40=000000 Abort trap: 6

我做了些搜索，这表示引导加载器(bootloader)无法加载。并意识到可能QEMU正在被分配的ROM不够。`execute outside RAM or ROM`通常是一个到某个地方的跳转，QEMU没法识别这个位置是ROM/RAM.

因为我们期望
* `CONFIG_BOOTBLOCK_BASE` is 0x10000
* `CONFIG_ROMSTAGE_BASE`  is 0x20000
* `CONFIG_SYS_SDRAM_BASE` is 0x1000000

也就是说，ROM从64K开始。所以我通过给一个`-m 2048M`(用于测试)来运行QEMU,克服了这个致命的QEMU错误，但依然没法让coreboot引导起来(在串口上没有输出).这表明，需要更多的调试。

我开始用gdb调试。我在QEMU引导上创建了一个gdb stub(通过使用-s -S),但是运行gdb连接时，gdb给我这些信息：
```
(gdb) target remote localhost:1234
Remote debugging using localhost:1234
warning: Architecture rejected target-supplied description
0x40080000 in ?? ()
```

这可能表明我不得不构建一个交叉gdb(aarch64-linux-gnu-gdb)并使用它。

为了这个，在Linux上我们有叫做gdb-multiarch的东西，但这在Mac OS X上弄不到。

我转向用Valgrind.有可用的Valgrind工具，它们可以帮助检测很多内存管理bug.

这是我在Valgrind上获得的东西，
>
```
==2070== Memcheck, a memory error detector
==2070== Copyright (C) 2002-2013, and GNU GPL’d, by Julian Seward et al.
==2070== Using Valgrind-3.10.1 and LibVEX; rerun with -h for copyright info
==2070== Command: aarch64-softmmu/qemu-system-aarch64 -machine virt -cpu cortex-a57 -machine type=virt -nographic -m 2048 -kernel /Users/naman/gsoc/coreboot2.0/coreboot/build/coreboot.rom
==2070==
–2070– aarch64-softmmu/qemu-system-aarch64:
–2070– dSYM directory is missing; consider using –dsymutil=yes
UNKNOWN __pthread_sigmask is unsupported. This warning will not be repeated.
–2070– WARNING: unhandled syscall: unix:330
–2070– You may be able to write your own handler.
–2070– Read the file README_MISSING_SYSCALL_OR_IOCTL.
–2070– Nevertheless we consider this a bug.  Please report
–2070– it at http://valgrind.org/support/bug_reports.html.
==2070== Warning: set address range perms: large range [0x1053c5040, 0x1253c5040) (undefined)
==2070== Warning: set address range perms: large range [0x239e56040, 0x255e55cc0) (undefined)
==2070== Warning: set address range perms: large range [0x255e56000, 0x2d5e56000) (defined)
```

`2070 set address range perms`意味着在一大块内存上权限改变了。

当一大块内存分配或撤销——一个mmap或umap调用，发生的时候，那是可以发生的。这意味着我们正泄漏一些内存，但我们需要找到是在哪里。我读了一些文档，相信一个叫massif tool(在valgrind中)可以使用。我正在看怎样找到内存被吃掉的地方。

在目标端，如果可能，会在valgrind上得到一些答案。但如果我没得到足够的线索，我会改用gdb(在Mac OS X上的AArch64)并继续我的调试。
