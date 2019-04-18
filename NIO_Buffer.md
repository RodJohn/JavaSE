
# buffer

buffer是一个可读写容器
本质上是一个数组  ？？ 
提供基本类型的
默认写 通过flip转换为读



wrapper 



# 属性

capacity
容量，
Buffer 能容纳的数据元素的最大值。
这一容量在 Buffer 创建时被赋值，并且永远不能被修改

limit 

有效容量

    写模式下，代表最大能写入的数据上限位置，这个时候 limit 等于 capacity 。
    读模式下，在 Buffer 完成所有数据写入后，通过调用 #flip() 方法，切换到读模式。
    此时，limit 等于 Buffer 中实际的数据大小。因为 Buffer 不一定被写满，所以不能使用 capacity 作为实际的数据大小。



position 
读写进度的标识位的
  下一个读写位
  位置，初始值为 0 。

    写模式下，每往 Buffer 中写入一个值，position 就自动加 1 ，代表下一次的写入位置。
    读模式下，每从 Buffer 中读取一个值，position 就自动加 1 ，代表下一次的读取位置。( 和写模式类似 )


mark 

  标记，通过 #mark() 方法，记录当前 position ；通过 reset() 方法，恢复 position 为标记。

    写模式下，标记上一次写位置。
    读模式下，标记上一次读位置。


mark <= position <= limit <= capacity



# 方法

flip

从写模式切换到读模式
如果要读取 Buffer 中的数据，需要切换模式，


public final Buffer flip() {
    limit = position; // 设置读取上限
    position = 0; // 重置 position
    mark = -1; // 清空 mark
    return this;
}



# ByteBuffer容器操作

3. 除了读写byte类型数据的函数，ByteBuffer的一个特别之处是它还定义了读写其它primitive数据的方法，如：

　　int getInt()       　　　　　　//从ByteBuffer中读出一个int值。
　　ByteBuffer putInt(int value)  // 写入一个int值到ByteBuffer中。


# slice 

不是新的数据  共享底层的数据

# 示例


使用 NIO复制文件 





线程安全





