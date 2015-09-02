<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/08/25/gsoc-coreboot-for-arm64-qemu-week-9-10/)  
作者: Naman  
翻译: mytbk  

在上篇文章，我说到用aarch64-linux-gnu-gdb和在QEMU中调试。在这两周，我热心地投入到在gdb中单步调试，反汇编并在QEMU的移植中调试。我在下面总结主要的亮点。

首先，正确调用QEMU的命令如下:
```
./aarch64-softmmu/qemu-system-aarch64 -machine virt -cpu cortex-a57 -machine type=virt -nographic -smp 1 -m 2048 -bios ~/coreboot/build coreboot.rom -s -S
```

在调用gdb后，我转到一步一步跟踪指令的执行来确定代码执行失败的位置和原因。代码执行的一个概要如下:

> (gdb) target remote :1234
> 
> Remote debugging using :1234
> 
> (gdb) set disassemble-next-line on
> 
> (gdb) stepi
> 
> 0x0000000000000980 in ?? ()
> 
> => 0x0000000000000980: 02 00 00 14 b 0x988
> 
> (gdb)
> 
> 0x0000000000000988 in ?? ()
> 
> => 0x0000000000000988: 1a 00 80 d2 mov x26, #0x0                    // #0
> 
> (gdb)
> 
> 0x000000000000098c in ?? ()
> 
> => 0x000000000000098c: 02 00 00 14 b 0x994
> 
> (gdb) c
> 
> Continuing.
> 
> ^C
> 
> Program received signal SIGINT, Interrupt.
> 
> 0x0000000000000750 in ?? ()
> 
> => 0x0000000000000750: 3f 08 00 71 cmp w1, #0x2

详细的版本可以看[这里](http://pastebin.com/EPM6RkYA)。

第一个错误的信号可以在这里看到，指令为0而且地址错得厉害。
```
0x64672d3337303031 in ?? ()
=> 0x64672d3337303031: 00 00 00 00 .inst 0x00000000 ; undefined
```

要看到为什么发生这件事的原因，我求助于在gdb里跟踪。这个可以通过添加以下内容到QEMU的命令中完成。这个可以创建一个可以读出来断定合适信息的日志文件在/tmp下。
```
-d out_asm,in_asm,exec,cpu,int,guest_errors -D /tmp/qemu.log
```

看反汇编，可以看出直到0x784的指令执行是正确的，在这之后就立刻不正常了。查看跟踪，这是代码中止的地方。

> IN:
> 
> 0x0000000000000784:  d65f03c0      ret

ret去向了一个坏的地方。所以栈被毁了或者执行进了一个之前应该执行的区域。接着，我在bootblock.debug文件做了个objdump.联系在这个地址的代码，可以断定代码在"ret in 0000000000010758 <raw\_write\_sctlr>:"执行失败。

下面要断定栈是在哪里被搞坏的。为了这个，在步进每条指令的时候，我看着栈指针。[这里](http://pastebin.com/5mXGmfZx)的输出展示了细节。每件事看上去都正常工作，直到0x00000000000007a0 (0x00000000000007a0: f3 7b 40 a9 ldp x19, x30, [sp] ),然后下面是0x00000000000007a4: ff 43 00 91 add sp, sp, #0x10.这是已保存的PC被破坏的地方。这个代码在"raw\_write\_sctlr\_current"(使用objdump)被调用。

从这个跟踪，我们可以得到以下信息:返回在0000000000010758 <raw\_write\_sctlr>这个地方变坏:

> 0x0000000000000908:  97fffe06      bl #-0x7e8 (addr 0x120)
> 
> …
> 
> 0x0000000000000120:  3800a017      sturb w23, [x0, #10]
> 
> 0x0000000000000124:  001c00d5      unallocated (Unallocated)
> 
> …

> Taking exception 1 [Undefined Instruction]
> 
> …from EL1
> 
> …with ESR 0x2000000

这是这里:

> 0000000000010908 <arm64_c_environment>:
> 
> 10908: 97fffe06  bl 10120 <loop3_csw+0x1b>
> 
> 1090c: aa0003f8  mov x24, x0

这最终给出一些在QEMU调试中的指引。似乎在smp\_processor\_id有一些不对齐。

在gdb中跟踪的时候，我们有
```
0x0000000000000908 in ?? ()

=> 0x0000000000000908: 06 fe ff 97   bl  0x120
```

(实际上是bl smp\_processor\_id(在src/arch/arm64/stage_entry.S))

在arm64\_c\_environment(在objdump)下我们有:
```
10908:       97fffe06        bl      10120 <loop3_csw+0x1b>
```

在跟踪里我们有:
```
IN:
0x0000000000000908:  97fffe06      bl #-0x7e8 (addr 0x120)
```

现在loop3\_csw在这里被定义(从objdump中得到):
```
0000000000010105 <loop3_csw>:
```

所以0x10105+0x1b=0x10120.从而它想bl到0x120但`smp_processor_id`在0x121.

`smp_processor_id`在(从objdump中得到):
```
0000000000010121 <smp_processor_id>:
```

这给出了代码执行失败的地方。接下来是找出这个不对齐的原因并纠正它。
