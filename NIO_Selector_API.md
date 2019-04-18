

# open

作用
	
	创建选择器

代码

	Selector selector = Selector.open();
	
	
#  register

作用

	将 Channel 注册到选择器中，并订阅关注的事件

代码

	SelectionKey key = channel.register(selector, SelectionKey.OP_READ);

说明

    1.如果一个 Channel 要注册到 Selector 中, 那么这个 Channel 必须是非阻塞的
	2.订阅的有OP_CONNECT（连接上服务端）、OP_ACCEPT（有客户端连接）、OP_WRITE（buffer 可写）、OP_READ（buffer 可读）
	事件可以一或多个，使用或运算|来组合


# select

作用

	阻塞到至少有一个通道发生了关注的事件
	返回的值表示有多少个 Channel 可操作

方法

	int count = Selector.select()

# selectedKeys

作用

	获取轮询到的channel的selectedKey
	返回的值表示有多少个 Channel 可操作

方法

	Set<SelectionKey> selectedKeys = selector.selectedKeys();


# SelectionKey

作用

	记录了channel和selector之间的注册关系

获取相对应的 Channel 和 Selector:

	Channel  channel  = selectionKey.channel();
	Selector selector = selectionKey.selector();  

获取订阅事件

	int interestSet = selectionKey.interestOps();
	boolean isInterestedInAccept  = interestSet & SelectionKey.OP_ACCEPT;
	boolean isInterestedInConnect = interestSet & SelectionKey.OP_CONNECT;
	boolean isInterestedInRead    = interestSet & SelectionKey.OP_READ;
	boolean isInterestedInWrite   = interestSet & SelectionKey.OP_WRITE;    

获取事件就绪状态

	selectionKey.isAcceptable();
	selectionKey.isConnectable();
	selectionKey.isReadable();
	selectionKey.isWritable();

获取相对应的Attaching

	Object attachedObj = selectionKey.attachment();



