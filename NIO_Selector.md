# 概述

    Selector（选择器）
    用于轮询已注册的Channel，查看是否发生了关注的事件

# 好处

    使用一个线程就可以管理多个 Channel 
    避免了线程上下文切换带来的开销

# 流程

    1.将 Channel注册到Selector中并订阅事件
    2.Selector会不断地轮询已注册的Channel，查看是否发生了关注的事件
    3.通过 SelectionKey 可以获取就绪 Channel 的集合


