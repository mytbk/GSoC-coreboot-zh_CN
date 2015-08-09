<meta http-equiv='Content-Type' content='text/html; charset=utf-8' />

原文地址: [http://blogs.coreboot.org/blog/2015/08/09/the-truth-about-purism-behind-the-coreboot-scenes/](http://blogs.coreboot.org/blog/2015/08/09/the-truth-about-purism-behind-the-coreboot-scenes/)  
作者: [Alexandru Gagniuc](http://blogs.coreboot.org/blog/author/mrnuke/)  
翻译: mytbk

# Purism的真相: 在(coreboot)的幕后

从[上次](http://blogs.coreboot.org/blog/2015/02/23/the-truth-about-purism-why-librem-is-not-the-same-as-libre/)我谈到Purism在coreboot笔记本的刺痛后，发生了很多事情，包括非自由的Librem 15的发行，同时有13寸版本的计划。我上篇文章好像在这主题上闪现出了好些争议，把我放在了很多不是很开心的支持者和Todd Weaver之间的邮件的CC域上。我已经避免在这个主题上再写文章了，因为我说不出关于Purism的任何好话。然而，考虑到近来新邮件的数量，我想我应该跟进为什么Librem 15在自由的前沿失败，以及为什么另一台Purism笔记本同样很可能会失败。

### 十分遥远的一条路
自从我上篇文章，很多事情已经发生，包括很多在这个问题上的媒体覆盖。现在，我可以用FSP,ME,和EC这些缩写[1]，而不担心失去90%的听众。现在在这个问题上有很多非技术的覆盖，一些非coreboot人可以很容易消化的东西，这非常好。然而，一些事情同时也在幕后发生，这是我想要谈论的内容。

### 尝试亲自会见Todd
在我起始的那篇文章，大量的邮件在愤怒的支持者，众筹，Purism和在CC域的我之间飞来飞去。通过这个，我有机会和Todd Weaver直接交流，并向他表达我在为什么Librem要失败的担心。我同时也赞助和Stefan和Ron建立一个会议。我和Stefan谈过，而他好像对和Todd谈话和帮助把coreboot装上Librem很兴奋。Ron通过邮件主题(email thread)分享了以下内容：
> Todd, I think the overall concern with librem and your statements is the seeming lack of realization that you're walking over very well trod ground, and there are lessons learned, and we might as well pass them on.  
> I for one seeing you making the same mistakes that have been frequently made over 15 years, and there's benefit to learning what we've learned.

那是3月时候的事了。这个会议没开成。会议死了，当我要Todd产生当前的用于要到来的Librem的代码。在从我的最初质询将近一个月后，在5月11日，Todd写道：
> We will be releasing the source code once we get coreboot working on our rev1 which will be shipping within the next few weeks.

### Librem 15启动了
快速前进一个月后，在邮件交换死了之后，我获得了一个信息，Librem 15已经搭载了AMI UEFI固件。在我没有Librem 15的情况下，这已经[由PC World确认](http://www.pcworld.com/article/2960524/laptop-computers/why-linux-enthusiasts-are-arguing-over-purisms-sleek-idealistic-librem-laptops.html).虽然我讨厌就这样了，我喜欢说这个：[我已经告诉过你了](http://blogs.coreboot.org/blog/2015/02/23/the-truth-about-purism-why-librem-is-not-the-same-as-libre/).

### Todd声明他有在Librem上工作的coreboot开发者
一波新的愤怒的邮件最近跟着来。我又一次有机会和Todd直接交流。他声明有三位coreboot开发者为Purism工作，但他们要保持匿名。然而，他提供了一位开发者的名字，为了隐私原因我不提了。当然了，我很好奇那个人做了什么：
```
[coreboot]$ git log |grep Author |grep -i <first name> -c
0
[coreboot]$ git log |grep Author |grep -i <last name> -c
0
[coreboot]$
```

我告诉Todd这个开发者不是coreboot贡献者，而为了我们交谈的目的，不能算作是一个coreboot“开发者”。我要求Todd产生另外两人中一个或两个人贡献的patch的git哈希。他没有这么做。

## Purism攻击Minifree(以前的Gluglug)
![](http://i0.wp.com/blogs.coreboot.org/files/2015/08/purism_attacks_minifree.png?resize=300%2C230)

就在这个早上，一个[推](https://twitter.com/Puri_sm/status/629787848246341632)引起了我的注意，对我来说，就像一个从Purism到Minifree的直接攻击。这个推拿“老的重的IBM ThinkPad”和Librem 15相比，展示了一张跑libreboot的T60的图片。这也是Gluglug(现在的Minifree)在他们销售T60型号用在它的产品页面的一张图片。

对那些没有意识到的你们来说，Minifree(以前的Gluglug)出售，从OS下到固件，完全自由的笔记本系统，而且获得了FSF的支持，通过他们的Respects Your Freedom认证。这些Minifree笔记本是我们，作为社区，自从超过15年前的LinuxBIOS的诞生，一直为它们的实现而工作得到的，也是我卡在这个项目超过5年的原因，尽管有所有的困难和障碍。攻击Minifree是侮辱我们这些年来艰难的工作，而对我来说，象征着Purism并没有真正的带来一点你们的自由，但他们真的喜欢你们的支持和金钱。

我知道我承诺会谈及为什么另一个Purism笔记本会像Librem 15一样很可能要失败，但我已经在一篇文章咆哮够了。我会在下次更详细地描述它。

## Todd, 你欠我们一个抱歉！
UPDATE: 我刚刚从Todd Weaver收到了如下私信：
> Ouch, that is not an approved tweet. I asked to have it removed, since I am a big fan of what Gluglug did/does. And we provide it as an alternative from our own website.

[1] 译者注: FSP=firmware support package, ME=management engine, EC=embedded controller
