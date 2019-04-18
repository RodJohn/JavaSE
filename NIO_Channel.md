
# 概述

    Channel相当于数据管道
    数据可以从Channel 读取到 Buffer 中，也可以从 Buffer 写到 Channel 中


# 特点

可读写

    对于同一个 Channel ，可以从它读取数据，也可以向它写入数据。
    而对于同一个 Stream ，通畅要么只能读，要么只能写
 
非阻塞 
    
    Channel 可以非阻塞的读写 IO 操作，
    而 Stream 只能阻塞的读写 IO 操作。
    




