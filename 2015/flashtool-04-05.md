<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

[原文](http://blogs.coreboot.org/blog/2015/06/29/gsoc-end-user-flash-tool-week-4-5/)    
作者: dmitro    
翻译: mytbk    

大家好！在第4和第5周，我做了以下几件事：  
1. `cbfs_tool`特性的整合  
2. 改进`libflashrom`的查询函数/整合已有的patch  
3. 拓展和改善GUI  

#### cbfs_tool特性的整合
`cbfs_tool`是个比`bios_extract`更大的项目，所以我花了比整合`bios_extract`更多的时间去整合它，因为我需要调查它是怎样工作的。我导入了和以下相关的代码：
* 创建ROM文件
* 添加组件，例如`stage`,`payload`,`option ROM`,`bootsplash`
* 打印ROM的内容
* 删除组件

和`libflashrom`和`bios_extract`的情况一样，我创建了一个静态库并把它和刷写工具链接起来。

#### 改进libflashrom查询函数/整合patch
往flashrom的邮件列表发了我的草稿patch做初始的review后，我得到了给了我很多帮助的很好的反馈。Urja Rannikko和Stefan Tauner之处了我的错误并给出了改进的建议，此外，Anton Kochkov分享了他在做相似的事情时的`libflashrom`的修改。这个社区真的很有帮助。谢谢！

所以，我在查询函数做了改进。现在我们有：
* `const char** fl_supported_programmers(void);`
* `const char* fl_version(void);`
* `fl_flashchip_info *fl_supported_flash_chips(void);`
* `fl_board_info *fl_supported_boards(void);`
* `fl_chipset_info *fl_supported_chipsets(void);`
* `int fl_supported_info_free(void *p);`

不必要的返回支持某类硬件数量的函数已经去掉了。现在我们可以调用函数去分配表并获得指向它的指针。当然了，我会创建一个patch并发上去做第二次review.

我有一个问题，因为我的SOIC夹子没有按时到，我没法在我的T60上测试操作相关的函数。事实上，这是我的错误，因为我没有注意到一个关于可能来自中国的因特网拍卖的评论。我已经等了3周了。现在我已经得到了，所以我这周的主要焦点是在真实的硬件上测试`libflashrom`.

#### 扩展和改善GUI
我扩展了一个GUI部分来允许用户操作ROM的内容，例如添加payload,bootsplash等等，还有移除这样的组件。当然这并不是我的项目的主要目的。主要的焦点在创建一个允许用户不关注哪些选项是正确的的工具。我会向这个方向走，我要尽量多的自动化建立coreboot镜像的过程。所以现在这是一种“高级模式”。我实现它有几个原因：
* 在整合`bios_extract`,`cbfs_tool`和`libflashrom`之后，做这个就不难了
* 实现和测试它帮助我更好地理解整合的代码和它的特性
* 也有想做一些手动修改的高级用户

当前这周的任务：
* 在T60(通过`linux_spi`)上测试用`libflashrom`探测，读取，刷写
* 由Stefan Tauner提出的GUI改进(在支持硬件的窗口搜索，排序和过滤)
* 代码清理
