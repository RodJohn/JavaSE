

# Server

	ServerSocketChannel ssc = ServerSocketChannel.open();
	ssc.configureBlocking(false);
	ssc.socket().bind(new InetSocketAddress("127.0.0.1", 8000));
	
	Selector selector = Selector.open();
	ssc.register(selector, SelectionKey.OP_ACCEPT);
	
	
	int nReady = selector.select();
	Set<SelectionKey> keys = selector.selectedKeys();
	Iterator<SelectionKey> it = keys.iterator();
	

	while (it.hasNext()) {
		SelectionKey key = it.next();
		it.remove();

		if (key.isAcceptable()) {
			SocketChannel socketChannel = ssc.accept();
			socketChannel.configureBlocking(false);
			socketChannel.register(selector, SelectionKey.OP_READ);
		}
		else if (key.isReadable()) {
			SocketChannel socketChannel = (SocketChannel) key.channel();
			readBuff.clear();
			socketChannel.read(readBuff);

			readBuff.flip();
			System.out.println("received : " + new String(readBuff.array()));
			key.interestOps(SelectionKey.OP_WRITE);
		}
		else if (key.isWritable()) {
			writeBuff.rewind();
			SocketChannel socketChannel = (SocketChannel) key.channel();
			socketChannel.write(writeBuff);
			key.interestOps(SelectionKey.OP_READ);
		}
	}
	}


# Client



			
			
