

# 创建

作用
	
	创建选择器

代码

	Selector selector = Selector.open();
	
	
#  注册

作用

	将 Channel 注册到选择器中，并订阅关注的事件

代码

	SelectionKey key = channel.register(selector, SelectionKey.OP_READ);

说明

    1.如果一个 Channel 要注册到 Selector 中, 那么这个 Channel 必须是非阻塞的
	2.订阅的有OP_CONNECT（连接上服务端）、OP_ACCEPT（有客户端连接）、OP_WRITE（buffer 可写）、OP_READ（buffer 可读）
	事件可以一或多个，使用或运算|来组合


# 轮询

作用

	轮询发生了关注事件的channel
	返回的值表示有多少个 Channel 可操作

方法

	int count = Selector.select()

# 获取

作用

	获取轮询到的channel的selectedKey
	返回的值表示有多少个 Channel 可操作

方法

	Set<SelectionKey> selectedKeys = selector.selectedKeys();


# SelectionKey

如上所示, 当我们使用 register 注册一个 Channel 时, 会返回一个 SelectionKey 对象, 这个对象包含了如下内容:

    interest set, 即我们感兴趣的事件集, 即在调用 register 注册 channel 时所设置的 interest set.

    ready set

    channel

    selector

    attached object, 可选的附加对象

interest set

我们可以通过如下方式获取 interest set:

int interestSet = selectionKey.interestOps();

boolean isInterestedInAccept  = interestSet & SelectionKey.OP_ACCEPT;
boolean isInterestedInConnect = interestSet & SelectionKey.OP_CONNECT;
boolean isInterestedInRead    = interestSet & SelectionKey.OP_READ;
boolean isInterestedInWrite   = interestSet & SelectionKey.OP_WRITE;    

ready set

代表了 Channel 所准备好了的操作.
我们可以像判断 interest set 一样操作 Ready set, 但是我们还可以使用如下方法进行判断:

int readySet = selectionKey.readyOps();

selectionKey.isAcceptable();
selectionKey.isConnectable();
selectionKey.isReadable();
selectionKey.isWritable();

获取相对应的 Channel 和 Selector:

	Channel  channel  = selectionKey.channel();
	Selector selector = selectionKey.selector();  

获取相对应的Attaching

	Object attachedObj = selectionKey.attachment();



