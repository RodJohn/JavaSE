
# ScatterGather


## 概述

	Scatter和Gather就是一个通道对应多个缓存

Scattering Reads

    scatter是将数据从一个channel读取到多个buffer中。
    read()方法按照buffer在数组中的顺序将从channel中读取的数据写入到buffer，
	当一个buffer被写满后，channel紧接着向另一个buffer中写。
       
Gathering Writes

    gather是指多个buffer的数据写入到同一个channel。
	write()方法会按照buffer在数组中的顺序，将数据写入到channel，
	注意只有position和limit之间的数据才会被写入。


## 示例


			SocketChannel socketChannel = serverSocketChannel.accept();
			ByteBuffer[] byteBuffers = new ByteBuffer[3];
			byteBuffers[0] = ByteBuffer.allocate(2);
			byteBuffers[1] = ByteBuffer.allocate(3);
			byteBuffers[2] = ByteBuffer.allocate(4);
			long capacity = byteBuffers[0].capacity()+byteBuffers[1].capacity()+byteBuffers[2].capacity();
			System.out.println(capacity);
			//这样写主要是为了保持连接不断开，并且能一直读取channel的数据
			while(true){
				long i = 0;
				for(int j=0;j<byteBuffers.length;j++){
					byteBuffers[j].clear();
				}
				while(i<capacity){
					i += socketChannel.read(byteBuffers);
					System.out.println(i);
				}
				for(int j=0;j<byteBuffers.length;j++){
					byteBuffers[j].flip();
				}
				System.out.println("开始写入");
				socketChannel.write(byteBuffers);
				
			}



put

    每个Buffer实现类，都提供了 #put(...) 方法，向 Buffer 写入数据。

    public abstract ByteBuffer put(byte b); 
    public abstract ByteBuffer put(int index, byte b);


get


    每个 Buffer 实现类，都提供了 #get(...) 方法，从 Buffer 读取数据。

    public abstract byte get();
    public abstract byte get(int index);



    
wrapper 






# ByteBuffer容器操作

3. 除了读写byte类型数据的函数，ByteBuffer的一个特别之处是它还定义了读写其它primitive数据的方法，如：

　　int getInt()       　　　　　　//从ByteBuffer中读出一个int值。
　　ByteBuffer putInt(int value)  // 写入一个int值到ByteBuffer中。


# slice 

不是新的数据  共享底层的数据




线程安全
