



　所谓的Scatter和Gather就是 通道对应多个缓存：将一个通道 读出到多个缓存；将多个缓存写入到一个通道


# Scattering Reads

    scatter（分散）是指数据从一个channel读取到多个buffer中。
    
    read()方法按照buffer在数组中的顺序将从channel中读取的数据写入到buffer，当一个buffer被写满后，channel紧接着向另一个buffer中写。
    
    
 Gathering Writes

    gather（聚集）是指多个buffer的数据写入到同一个channel。
    
    
write()方法会按照buffer在数组中的顺序，将数据写入到channel，注意只有position和limit之间的数据才会被写入。
因此，如果一个buffer的容量为128byte，但是仅仅包含58byte的数据，那么这58byte的数据将被写入到channel中。
因此与Scattering Reads相反，Gathering Writes能较好的处理动态消息。



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



