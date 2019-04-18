
# 概述

在 Java NIO 中，基本上所有的 IO 操作都是从 Channel 开始。
数据可以从 Channel 读取到 Buffer 中，也可以从 Buffer 写到 Channel 中

# 特点


NIO Channel 类似 Java Stream ，但又有几点不同：

    对于同一个 Channel ，我们可以从它读取数据，也可以向它写入数据。而对于同一个 Stream ，通畅要么只能读，要么只能写，二选一( 有些文章也描述成“单向”，也是这个意思 )。
    Channel 可以非阻塞的读写 IO 操作，而 Stream 只能阻塞的读写 IO 操作。
    Channel 必须配合 Buffer 使用，总是先读取到一个 Buffer 中，又或者是向一个 Buffer 写入。也就是说，我们无法绕过 Buffer ，直接向 Channel 写入数据。





