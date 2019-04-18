# 概述

Selector ， 一般称为选择器
用于轮询一个或多个 NIO Channel 的状态是否处于可读、可写。
如此，一个线程就可以管理多个 Channel ，也就说可以管理多个网络连接。也因此，Selector 也被称为多路复用器。

那么 Selector 是如何轮询的呢？

    首先，需要将 Channel 注册到 Selector 中，这样 Selector 才知道哪些 Channel 是它需要管理的。
    之后，Selector 会不断地轮询注册在其上的 Channel 。
    如果某个 Channel 上面发生了读或者写事件，这个 Channel 就处于就绪状态，
    会被 Selector 轮询出来，然后通过 SelectionKey 可以获取就绪 Channel 的集合，进行后续的 I/O 操作。


