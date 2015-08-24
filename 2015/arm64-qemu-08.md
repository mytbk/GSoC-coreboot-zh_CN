<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/07/28/gsoc-coreboot-for-arm64-qemu-week-8/)  
作者: Naman  
翻译: mytbk  

就像我在上篇文章说的，现在我要做QEMU引导的调试。我打算用Valgrind工具检测各种内存管理bug并把那些信息用到我的调试上。但可悲的是，Valgrind提供的那些信息没什么大用，因为它没有处理QEMU中coreboot代码的执行流。我最终不得不改用gdb并用它做更长远的调试。

这是个开始的小问题，因为，在我的上篇文章中，在Mac OS X上构建aarch64-linux-gnu-gdb并不直接，因为没有直接的"gdb-multiarch"的替代品。我能够把这个完成了。我在下面谈一下搭建这个的一些基本的步骤。

首先，我们需要一些包去构建gdb.它们是:  
expat guile texinfo

然后，从[这里](https://aur.archlinux.org/packages/aarch64-linux-gnu-gdb/)下载aarch64-gdb.现在，你需要配置CC为gcc(GNU gcc而不是原生的到clang的符号链接).然后继续执行:
```
$ ./configure --target=aarch64-linux-gnu
$ make
$ make install
```

如果这个成功地完成，你会在系统上正确地装好aarch64-gdb.重要的事是记住使用GNU gcc(>=4.9)而不是原生的Mac OS gcc.

要运行gdb,你必须执行:
```
$ aarch64-linux-gnu-gdb
```

输出如下:
```
GNU gdb (GDB) 7.9
Copyright (C) 2015 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "--host=x86_64-apple-darwin13.3.0 --target=aarch64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word".
```

现在我让gdb跑起来了。然后我通过在运行QEMU时传入参数"-s -S"开始调试。在此之后，我需要用下列命令远程连接到gdb:
```
(gdb) target remote : 1234
```

我收到的一些调试信息是这些:
>
```
(gdb) target remote :1234

Remote debugging using :1234

0x0000000000000200 in ?? ()

(gdb) run

The “remote” target does not support “run”.  Try “help target” or “continue”.

(gdb) continue

Continuing.

^C

Program received signal SIGINT, Interrupt.

0x0000000000000200 in ?? ()
```

在尝试在gdb单步执行的时候，我收到了:
>
```
(gdb) step

Cannot find bounds of current function
```

像这样的错误通常在我们溢出了一个缓冲区或者搞坏了栈的时候看到，合适的返回地址被破坏了。当调试器尝试给出地址在哪个函数内的时候，它失败了，因为地址不在程序的任何一个函数里面。

在gdb里运行where(where现实当前行和函数，以及带你到那个位置的调用栈)的时候我得到:
>
```
(gdb) where

#0  0x0000000000000200 in ?? ()
```

在使用gdb的信息还原一下源码之后，从gdb我们看到了一些在`src/arch/arm64/stage_entry.S`文件中`stage_entry`函数的问题。我会重置那些东西并继续调试。
