


# FileChannel


	FileChannel是一个连接到文件的通道。可以通过文件通道读写文件。
	FileChannel无法设置为非阻塞模式，它总是运行在阻塞模式下。

获取channel


	通过InputStream、OutputStream或RandomAccessFile来获取一个FileChannel实例。
	
	RandomAccessFile aFile = new RandomAccessFile("data/nio-data.txt", "rw");
	FileChannel inChannel = aFile.getChannel();
  
读取数据
 

	read()方法可以将数据从FileChannel读取到Buffer中。
	read()方法返回的int值表示了有多少字节被读到了Buffer中。如果返回-1，表示到了文件末尾。

	ByteBuffer buf = ByteBuffer.allocate(48);
	int bytesRead = inChannel.read(buf);

写入数据


	write()方法将Buffer中的数据写入到FileChannel
	因为无法保证write()方法一次能向FileChannel写入多少字节，
	因此需要重复调用write()方法，直到Buffer中已经没有尚未写入通道的字节。


	String newData = "New String to write to file..." + System.currentTimeMillis();
	ByteBuffer buf = ByteBuffer.allocate(48);
	buf.clear();
	buf.put(newData.getBytes());
	buf.flip();
	while(buf.hasRemaining()) {
		channel.write(buf);
	}



关闭Channel

	channel.close();

# ServerSocketChannel

	ServerSocketChannel用于监听socket连接
	可以设置为阻塞和非阻塞模式

获取Channel

	ServerSocketChannel serverSocketChannel = ServerSocketChannel.open()
	
模式设置

	serverSocketChannel.configureBlocking(false);
	
	
监听新进来的连接


	accept()方法会返回一个SocketChannel。
	默认情况下，方法会阻塞并等待新进来的连接的 
	在非阻塞模式下，accept() 方法会立刻返回，如果还没有新进来的连接,返回的将是null
	SocketChannel socketChannel = serverSocketChannel.accept();	 

关闭Channel

	serverSocketChannel.close()


实例

	ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();

	serverSocketChannel.socket().bind(new InetSocketAddress(9999));
	serverSocketChannel.configureBlocking(false);

	while(true){
		SocketChannel socketChannel =
				serverSocketChannel.accept();

		if(socketChannel != null){
			//do something with socketChannel...
		}
	}

	serverSocketChannel.close()




  
  
# 参考

http://ifeve.com/file-channel/

