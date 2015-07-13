<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/07/01/gsoc-coreboot-for-arm64-qemu-week-4-and-5/)  
作者: Naman  
翻译: mytbk  

从这周开始，我开始处理AArch64设计的核心方面。我继续构建ARMv8的过程，同时处理需要的patching-up,interfacing,hook-up.在我的上一篇文章里，我已经提到了我在OSX上面对的AArch64工具链构建的错误(在binutils上).我不得不从binutils去除`-enable-gold`的选项。在做了这个小的更新后，`build_BINUTILS`就像这样，我可以让工具链work了。
```
build_BINUTILS() {
if [ $TARGETARCH == "x86_64-elf" ]; then
ADDITIONALTARGET=",i386-elf"
fi
CC="$CC" ../binutils-${BINUTILS_VERSION}/configure --prefix=$TARGETDIR \
--target=${TARGETARCH} --enable-targets=${TARGETARCH}${ADDITIONALTARGET} \
--disable-werror --disable-nls --enable-lto \
--enable-plugins --enable-multilibs CFLAGS="$HOSTCFLAGS" || touch .failed
$MAKE $JOBS || touch .failed
$MAKE install DESTDIR=$DESTDIR || touch .failed
}
```

在修复工具链后，工作才开始。在尝试构建的时候，我得到的错误是：
> toolchain.inc:137: *** building bootblock without the required toolchain. Stop.

这是因为某些Kconfig中错误的CONFIG选项导致的。在这阶段后，ARM64的初始bring-up看起来稳定了。往前走，我遇到了在`src/arch/arm64/c_entry.c`的一个错误：
> src/arch/arm64/c_entry.c: In function ‘arm64_init': src/arch/arm64/c_entry.c:52:2: error: implicit declaration of function ‘cpu_set_bsp’ [-Werror=implicit-function-declaration] cpu_set_bsp();

所包含的需要的文件和结构是正确的，但我一直得到这个错误。Furquan最终指向了[change 257370](https://chromium-review.googlesource.com/#/c/257370/),跟随这个，我通过了这事。在此之后，我不得不解决另外一个BSD/OSX在`genbuild_h.sh`关于日期的问题让我的构建继续进行。

随后，一些对ARMv8做的架构的决定要做了。在初始的版本中，我一直指望`media.c`中基于`cbfs_media`的结构，用于创建读，写，映射的功能。但老的形式(在`cbfs_core.c`和`cbfs_core.h`里)已经变了。为了跟进，和一致的维护，我们决定像在ARMv7一样处理它，也就是说，建立一个映射到QEMU内存映射空间。现在已经像ARMv7一样带起来了。这在未来可能会改变。同时，UART的组织已经完成了。`plo11.c`被包含，像在`src/drivers/uart/Makefile.inc`一样，通过在ARMv8 Kconfig里面设置`DRIVERS_UART_PL011`.

下一个故障是处理SMP.在我的计划里，我已经提出，合并SMP进模拟会是一个长期的目标。但因为SMP是ARM64核心逻辑的一部分，在这阶段不能完全忽视。我遇到了这个：
> coreboot/src/arch/arm64/stage_entry.S:94: undefined reference to \`smp_processor_id’  
build/arch/arm64/c_entry.romstage.o: In function \`seed_stack':

这时，需要一个添加了`smp_processor_id()`的CPU文件，我现在正在做这个。下周的计划是通过这些(并且接触新的无法预料的问题)并在QEMU中引导起来。
