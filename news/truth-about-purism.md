<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

原文地址: [http://blogs.coreboot.org/blog/2015/08/09/the-truth-about-purism-behind-the-coreboot-scenes/](http://blogs.coreboot.org/blog/2015/08/09/the-truth-about-purism-behind-the-coreboot-scenes/)  
作者: [Alexandru Gagniuc](http://blogs.coreboot.org/blog/author/mrnuke/)  
日期: 2015-08-09  
翻译: mytbk  
同时感谢dp@bdwm等网友为翻译提供指导！

译者注: 在8月10日，Todd Weaver为此文留下了[他的回应](http://blogs.coreboot.org/blog/2015/08/09/the-truth-about-purism-behind-the-coreboot-scenes/#comments)。

# Purism的真相: 在(coreboot)幕后
自从[上次](http://blogs.coreboot.org/blog/2015/02/23/the-truth-about-purism-why-librem-is-not-the-same-as-libre/)我戳到Purism在coreboot笔记本的痛处后，很多事情发生了，包括搭载非自由BIOS的Librem 15的发行，同时推出了13寸版本的计划。我上篇文章好像在这主题上闪现出了好些争议，把我放在了很多不是很开心的支持者和Todd Weaver之间的邮件的抄送列表上。我已经避免在这个主题上再写文章了，因为我说不出任何关于Purism的好话。然而，考虑到近来新邮件的数量，我想我应该跟进地说一下，为什么Librem 15在自由的前沿失败，以及为什么另一台Purism笔记本同样很可能会失败。

### 十分遥远的路
自从我发了上一篇文章，很多事情已经发生，包括很多媒体在在这个问题上的报导。现在，我可以用FSP,ME,和EC这些缩写[1]，而不担心失去90%的听众。现在，在这个问题上有很多非技术性的报导，让一些不了解coreboot的人也可以很容易消化理解，这非常好。然而，我想要谈的内容，是一些同时在幕后发生的事情。

### 尝试亲自会见Todd
在我开始写的那篇文章，大量的邮件在愤怒的支持者，众筹机构，Purism和在邮件抄送列表的我，之间飞来飞去。通过这件事，我有机会和Todd Weaver直接交流，并向他表达，我对为什么Librem要失败的关心。我同时也提供赞助，和Stefan和Ron建立一个会议。我和Stefan谈过，而他好像对和Todd谈话和帮助把coreboot装上Librem很兴奋。Ron通过邮件主题(email thread)分享了以下内容：
> Todd,我想总体的对Librem和你的声明的关心，是从表面上看，你还没领悟到你正在走很多人走过的路，有很多经验教训，而我们可以把它们传递下去。
> 我看你正在犯超过15年来人们经常犯的错误，而学习我们已经领会到的经验教训会很有好处。

那是3月时候的事了。这个会议没开成。当我要Todd产生当前的用于将要到来的Librem的代码的时候，讨论没了。就在我最初的质询将近一个月后，在5月11日，Todd写道：
> 我们让coreboot在几周之内发行的第一版运行起来后，就会发布源码。

### Librem 15启动了
时间快进一个月，在邮件交换消失了之后，我获得了一个信息，Librem 15已经搭载了AMI的UEFI固件。尽管我没有Librem 15，[PC World确认了这个消息](http://www.pcworld.com/article/2960524/laptop-computers/why-linux-enthusiasts-are-arguing-over-purisms-sleek-idealistic-librem-laptops.html).虽然我讨厌就这样结束了，我还是喜欢说这个：[我已经告诉过你了](http://blogs.coreboot.org/blog/2015/02/23/the-truth-about-purism-why-librem-is-not-the-same-as-libre/).

### Todd声明他有在Librem上工作的coreboot开发者
一波新的愤怒的邮件最近跟着来。我又一次有机会和Todd直接交流。他声明有三位coreboot开发者为Purism工作，但他们要保持匿名。然而，他提供了一位开发者的名字，出于隐私我就不提了。当然了，我很好奇那个人做了什么：
```
[coreboot]$ git log |grep Author |grep -i <first name> -c
0
[coreboot]$ git log |grep Author |grep -i <last name> -c
0
[coreboot]$
```

我告诉Todd这个开发者不是coreboot贡献者，而为了我们的交谈，不能算作是一个coreboot“开发者”。我要求Todd产生另外两人中一个或两个人贡献的patch的git哈，他没有按我的要求做。

## Purism攻击Minifree(以前的Gluglug)
![](http://i0.wp.com/blogs.coreboot.org/files/2015/08/purism_attacks_minifree.png?resize=300%2C230)

就在这个早上，一则[推特](https://twitter.com/Puri_sm/status/629787848246341632)引起了我的注意，对我来说，它就像一个从Purism到Minifree的直接攻击。这个推拿“又老又重IBM ThinkPad”和Librem 15相比，展示了一张跑libreboot的T60的图片。这也是Gluglug(现在的Minifree)在他们销售T60型号用在它的产品页面的一张图片。

告诉不知情的你们，Minifree(以前的Gluglug)出售，上自OS下到固件，完全自由的笔记本系统，而且通过FSF的Respects Your Freedom认证，获得FSF的支持。这些Minifree笔记本是我们，作为社区，自从超过15年前的LinuxBIOS的诞生，一直致力于工作而得到的成果，也是我不管所有的困难和障碍，卡在这个项目超过5年的原因。攻击Minifree，是侮辱我们这些年来艰难的工作。而对我来说，象征着Purism并没有真正给你们带来一点自由，但他们真的喜欢你们的支持和金钱。

我知道，我承诺还要谈为什么另一个Purism笔记本会像Librem 15一样，很可能要失败，但我已经在一篇文章中咆哮够了。我会在下次更详细地描述它。

## Todd, 你欠我们一个抱歉！
UPDATE: 我刚刚从Todd Weaver收到了如下私信：
> Ouch, 那不是一个被认可的推特，我要求把它删掉了，因为我是对Gluglug所做的工作的一名粉丝,并且我们在我们自己的网站将它作为一种选择。

[1] 译者注: FSP=firmware support package, ME=management engine, EC=embedded controller
