
# 作用

    Buffer相当于一个容器
    用于缓存读写间的数据

# 结构

    Buffer内部是一个数组
    通过position,limit,capacity记录当期操作的状态


## 属性

capacity

    容量，能容纳的数据元素的最大值。

limit 

    有效容量
    写模式下，代表最大能写入的数据上限位置，这个时候 limit 等于 capacity 。
    读模式下，等于 Buffer 中实际的数据大小。因为 Buffer 不一定被写满

position 

    下一个可读写位
    写模式下，每往 Buffer 中写入一个值，position 就自动加 1 ，代表下一次的写入位置。
    读模式下，每从 Buffer 中读取一个值，position 就自动加 1 ，代表下一次的读取位置。( 和写模式类似 )


mark 

    标记位
    通过 #mark() 方法，记录当前 position ；通过 reset() 方法，恢复 position 为标记。


mark <= position <= limit <= capacity






# 基本操作


读操作

    将Channel中的数据写入到 Buffer 中
    会返回写入的数据大小

    public interface ReadableByteChannel extends Channel {
        public int read(ByteBuffer dst) throws IOException;
    }

   
    
写操作

    将Buffer中的数据写入Channel
    会返回写入的数据大小

    public interface WritableByteChannel extends Channel {
        public int write(ByteBuffer src) throws IOException; 
    }
    
 
flip 

    将Buffer从写模式切换到读模式

    public final Buffer flip() {
        limit = position; // 设置读取上限
        position = 0; // 重置 position
        mark = -1; // 清空 mark
        return this;
    }

rewind

    大多数情况下，该方法主要针对于读模式，所以可以翻译为“倒带”。

    public final Buffer rewind() {
        position = 0; // 重置 position
        mark = -1; // 清空 mark
        return this;
    }


clear

    clear() 方法，可以“重置” Buffer 的数据。因此，我们可以重新读取和写入 Buffer 了。

    大多数情况下，该方法主要针对于写模式。代码如下：

    public final Buffer clear() {
        position = 0; // 重置 position
        limit = capacity; // 恢复 limit 为 capacity
        mark = -1; // 清空 mark
        return this;
    }


# 示例


    文件复制

    public static void main(String[] args) throws Exception {
        ByteBuffer buff = ByteBuffer.allocate(6);
        FileChannel fin = null;
        FileChannel fout = null;
        try {
            fin = new RandomAccessFile("in.txt", "rw").getChannel();
            fout = new RandomAccessFile("out.txt", "rw").getChannel();
            while (fin.read(buff) != -1) {
                buff.flip();
                fout.write(buff);
                buff.clear();
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fin != null) {
                    fin.close();
                }
                if (fout != null) {
                    fout.close();
                }
            } catch (IOException e) {
                throw e;
            }
        }
    }
    
    




