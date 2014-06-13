---
layout: post
title: "GCC notebook"
description: ""
category: toolchain
tags: [GCC, toolchain, debug]
---

# Debug

## Compile

各种姿势，本地编译，交叉编译。有足够的耐心这些都不难。比如完全的from scratch的编译包括glibc，但是glibc的编译又需要交叉编译的GCC，这时引入了mini GCC这么一个中间状态的GCC来做glibc的编译。观察GCC的社区他们对build farm的建设似乎不是很上心，印象中这个应该是比较重要的一条，快速迭代和验证，加上GCC软件本身的复杂度，非常有效。可能他们有更好的办法。

看编译期的原理和GCC的时候发现，gcc只是一个driver，而真正的是cc1这种，因此在出现问题的时候需要

	gdb cc1
	run xargs

在GCC的collection中的一些小工具来自binutils，值得好好研究比如addr2line readelf objdump这些ELF相关的小工具，尤其在调试的时候panic的时候CPU一般会dump出寄存器的值，比如x86的RIP或者MIPS上面的EPC，根据这些值找到PC，然后用addr2line来根据PC来和代码对应起来，调试起来简直事半功倍！至于readelf objdump在对付怀疑是编译器的问题的时候查找不同之处的时可以说是最有效的工具了。

## HOWTO Debug

GCC由前端语言的parser和中间的GIMPLE和后端的RTL组成，当然现在前端已经非常成熟已经很少有人去碰了，一般都在中端和后端捣鼓，还好虽然GNU C的风格看的人眼睛要瞎，总算是有一个pass的概念，结构化很好，终于肉眼可以看懂在讲什么。

不做这个一般不知道，等真的开始动起来，吃了亏，问了老司机才发现还有好多的小技巧。简单的列几个我所知道的：

1. 在Gentoo或LFS这样的发行版的Guide中他们编GCC的时候一般要求你新建一个目录，比如gcc_build，然后这个目录里面使用绝对路径指向需要编的工具链的configure，这样中间文件和最后的文件自动按照目录结果mirror到新的目录，这么一说，懂的人自然就懂了，会心一笑哈。当然还有个问题，同一个目录你需要不同优化级别的target，因为最后衡量opt是否做的好还得请SPEC来评评理，而O0和O2的差距有点大，因此一般用O2来跑SPEC，但是O2出来的东西很难debug，很多被opt out了，这个时候需要调O0的。因此两个目录build_O0和build_O2顿时让世界美好起来！

2. 做编译的时候肯定有人会问我该怎么去评估这个编译器确实正确呢？当然会有很多学术性的解答，但是GCC里面应该就是自举这一项了。Wiki上说因为GCC本身是一个复杂的软件，能够自举恰恰说明其正确性，因此出现了编译3遍的现象。但是问题来了，我为一段代码做一个优化，这个优化在GCC编译自举的时候出现问题了，并且问题相当的诡异，怎么办？之所以说pass好肯定是有原因的，pass一般有一个execute和gate，因此在你添加一个pass或者修改一个pass的时候，将其默认的gate设置成0,然后在编译自己的代码的时候通过参数将这个gate设置成1，这样会方便很多。

3. 当然如果只是编译测试一段代码的时候问题简单很多，但是当编译一个大型工程的时候编译器报internal error了呢？确定了具体那个问题出了问题，但是各种make依赖怎么破？难道肉眼去翻看Makefile然后写gcc的参数？这个当然有机智的解法啦！最开始看gcc社区的时候他们会给你两个参数，-v这个显式的打印出编译的步骤，就是怎样针对每个源文件处理依赖和Makefile的语法的，然后-save-temps保存中间文件。找出出问题的那一步，然后gdb

4. 最后添加或者修改了pass后怎么验证我的代码确实能够work？当然接触这个的肯定明白SPEC2000这些了。不多谈，算数平均数提高1个点，都要高兴的直跳了。

5. 有人肯定要说了，我只想搞一个能够hotplug的东西，然后交出binary，大家都知道GPL传染性，但这码事绝对存在！于是就有了plugin了，最开始社区不喜欢这个，可是这个东西有些时候非常有效。将一些pass做成so的plugin编译的时候调用。

# Register Allocation


