### websockt
	websockt基于http协议，需要http协议与服务器传递webscokt信息，webscokt key，url地址，webscokt协议版本等等。
	需要通过http协议的帮助来完成升级到webscokt协议。
	webscokt主要解决的问题长连接，最常见的问题即使通信。
	在webscokt之前实现长连接的方式ajax轮询，long poll。
	ajax轮询方式：客户端每隔一段时间去访问服务器看有没有新的消息。
	long poll：客户端向服务器端发起请求，如果没有消息则挂起，直到有消息时进行返回。
	两种方式相对都不是完美的，因为他们都是客户端主动，服务器端被动。且他们也都是建立了多次的http连接。
	而webscokt这种方式则首先可以解决服务端被动问题。
	ajax与longpoll都是需要建立多次连接。
	只有等服务端有消息时才需要进行发送处理，而没有消息时不会占用过多服务端资源。
	
	open	Socket.onopen	连接建立时触发
	message	Socket.onmessage	客户端接收服务端数据时触发
	error	Socket.onerror	通信发生错误时触发
	close	Socket.onclose	连接关闭时触发
	Socket.send()	
	使用连接发送数据
	Socket.close()	
	关闭连接