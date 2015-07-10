<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/06/23/gsoc-coreboot-for-arm64-qemu-week-3/)  
作者: Naman  
翻译: mytbk  

在第3周，我开始开始构建在目标ARM64 QEMU运行的coreboot镜像。我刚刚开始的时候，就不得不面对一个大大的红旗，一个工具链构建错误！新的工具链(v1.31)在MacOS X上构建失败。尽管尝试了几次，`binutils`构建失败，我没法让工具链跑起来。`/buildgcc`在OSX上也失败了，因为缺失clang的字符串搜索。Mark帮助了我，在这个前端引入了一个小[patch](http://review.coreboot.org/#/c/10569/).我同时也尝试用真实的gcc和g++(不是OSX上原生的链接到clang的gcc和g++),但不能改正工具链构建错误。但奇怪的是，当在同一机器我尝试构建一个老工具链(v1.24)时，它work了。在疑惑中，我决定用老的工具链去继续这个构建。

我不得不在`src/mainboard/emulation/Kconfig`上做一些修改，去带起新的模拟主板。在此之后，我可以成功地用`make menuconfig`生成一个配置。

下一步是生成`build/coreboot.rom`.在构建的时候，我一直在面对一些我一路在解决的错误。这周的计划是结束构建。我也同时要看看解决工具链构建问题的办法。Stepan说新的工具链还没在OSX上测试，所以这应该会提供一些有用的见识。
