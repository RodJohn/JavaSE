

# 实现类




# HeapByteBuffer


    HeadByteBuffer
    新生成了一个长度为capacity的数组

    public static ByteBuffer allocate(int capacity) {
        if (capacity < 0)
            throw new IllegalArgumentException();
        return new HeapByteBuffer(capacity, capacity);
    }

    HeapByteBuffer(int cap, int lim, boolean isReadOnly) {  
        // package-private
        super(-1, 0, lim, cap, new byte[cap], 0);
        this.isReadOnly = isReadOnly;
    }

    ByteBuffer(int mark, int pos, int lim, int cap, byte[] hb, int offset) {
        super(mark, pos, lim, cap, 0);
        this.hb = hb;
        this.offset = offset;
    }


# DirectByteBuffer

    DirectByteBuffer
    由runtime去申请了了一块内存。不是直接在堆内存中。


    public static ByteBuffer allocateDirect(int capacity) {
        if (capacity < 0) {
            throw new IllegalArgumentException("capacity < 0: " + capacity);
        }

        DirectByteBuffer.MemoryRef memoryRef = new DirectByteBuffer.MemoryRef(capacity);
        return new DirectByteBuffer(capacity, memoryRef);
    }

    MemoryRef(int capacity) {
        VMRuntime runtime = VMRuntime.getRuntime();
        buffer = (byte[]) runtime.newNonMovableArray(byte.class, capacity + 7);
        allocatedAddress = runtime.addressOf(buffer);
        // Offset is set to handle the alignment: http://b/16449607
        offset = (int) (((allocatedAddress + 7) & ~(long) 7) - allocatedAddress);
        isAccessible = true;
    }





反编译的sun公司的代码 openjdk

native jni方法

直接内存

分配内存
使用address记录内存的地址   

效率高

heatbytebuffer
操作系统
不是直接操作堆上的内存
而是先从堆上拷贝到 物理内存 ，然后和IO设备交互
directbytebuffer
操作系统可以直接操作
不需要拷贝  零拷贝


为什么要拷贝

操作系统如果直接操作java运行时数据区
不安全
GC  内存复制

只能将数据拷贝到非java内存

拷贝快 IO慢  安全点进行拷贝


0拷贝


# Direct Buffe
直接缓冲

零拷贝
减少内存 拷贝  





只读缓冲 HeapByteBufferR
直接缓冲
 直接 和 非直接 



# MappedByteBuffer

内存映射文件
直接在内存中修改文件 由操作系统进行同步到文件
也是堆外内存

速度快








 





