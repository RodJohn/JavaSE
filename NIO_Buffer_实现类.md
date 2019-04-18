

内部是一个数组

# 实现类




# HeapByteBuffer


    public static ByteBuffer allocate(int capacity) {
        if (capacity < 0)
            throw new IllegalArgumentException();
        return new HeapByteBuffer(capacity, capacity);
    }






# DirectByteBuffer

public static ByteBuffer allocateDirect(int capacity) {
    return new DirectByteBuffer(capacity);
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



# MappedByteBuffer

内存映射文件
直接在内存中修改文件 由操作系统进行同步到文件
也是堆外内存

速度快








 





