# 为chromium对接自己的输入法

按照一般情况，基于chromium［content－shell］开发是不需要考虑输入法的对接的，chromium project中早就为各个平台做了对接；每个平台都通过 **InputContext** 实现了与各个系统的衔接；

万事都有例外，比如说工作中的情况；因为我们的产品是跑在**裸奔**状态下的linux；没有DE（desktop environments）没有**gtk、x11；** 所以自然而然没有`gtk_im_context_focus_in(gtk_context_);`来让直接使用输入法; 在图形的backend也没有glwindow之类的东西啦，使用的是directfb这个轻量级硬件图形加速库，所以我们真实的环境是：

* 没有Desk-environment
* 没有现有的输入法framework
* 没有现有的ime 输入法引擎

篇幅有限，此文仅仅介绍输入法的对接，也就是说事件怎么给输入法，以及输入法怎么和整个事件分发过程结合起来；开始之前，我们看一下整个框架的介绍，以便我们之后结合代码进行分析；

![](/meet_chromium/img/chromium-ime_context.001.png)


