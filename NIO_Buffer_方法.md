



# 向 Buffer 写入数据

每个 Buffer 实现类，都提供了 #put(...) 方法，向 Buffer 写入数据。以 ByteBuffer 举例子，代码如下：

// 写入 byte
public abstract ByteBuffer put(byte b); 
public abstract ByteBuffer put(int index, byte b);
// 写入 byte 数组
public final ByteBuffer put(byte[] src) { ... }
public ByteBuffer put(byte[] src, int offset, int length) {...}
// ... 省略，还有其他 put 方法

对于 Buffer 来说，有一个非常重要的操作就是，我们要讲来自 Channel 的数据写入到 Buffer 中。在系统层面上，这个操作我们称为读操作，因为数据是从外部( 文件或者网络等 )读取到内存中。示例如下：

int num = channel.read(buffer);

    上述方法会返回从 Channel 中写入到 Buffer 的数据大小。对应方法的代码如下：

    public interface ReadableByteChannel extends Channel {

        public int read(ByteBuffer dst) throws IOException;
        
    }

    注意，通常在说 NIO 的读操作的时候，我们说的是从 Channel 中读数据到 Buffer 中，对应的是对 Buffer 的写入操作，初学者需要理清楚这个。
    
    
    
# 5. 从 Buffer 读取数据

每个 Buffer 实现类，都提供了 #get(...) 方法，从 Buffer 读取数据。以 ByteBuffer 举例子，代码如下：

// 读取 byte
public abstract byte get();
public abstract byte get(int index);
// 读取 byte 数组
public ByteBuffer get(byte[] dst, int offset, int length) {...}
public ByteBuffer get(byte[] dst) {...}
// ... 省略，还有其他 get 方法

对于 Buffer 来说，还有一个非常重要的操作就是，我们要讲来向 Channel 的写入 Buffer 中的数据。在系统层面上，这个操作我们称为写操作，因为数据是从内存中写入到外部( 文件或者网络等 )。示例如下：

int num = channel.write(buffer);

    上述方法会返回向 Channel 中写入 Buffer 的数据大小。对应方法的代码如下：

    public interface WritableByteChannel extends Channel {

        public int write(ByteBuffer src) throws IOException;
        
    }
    
 
 
 
 如果要读取 Buffer 中的数据，需要切换模式，从写模式切换到读模式。对应的为 #flip() 方法，代码如下：

public final Buffer flip() {
    limit = position; // 设置读取上限
    position = 0; // 重置 position
    mark = -1; // 清空 mark
    return this;
}


大多数情况下，该方法主要针对于读模式，所以可以翻译为“倒带”。也就是说，和我们当年的磁带倒回去是一个意思。代码如下：

public final Buffer rewind() {
    position = 0; // 重置 position
    mark = -1; // 清空 mark
    return this;
}


#clear() 方法，可以“重置” Buffer 的数据。因此，我们可以重新读取和写入 Buffer 了。

大多数情况下，该方法主要针对于写模式。代码如下：

public final Buffer clear() {
    position = 0; // 重置 position
    limit = capacity; // 恢复 limit 为 capacity
    mark = -1; // 清空 mark
    return this;
}



    






