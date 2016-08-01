#9 通信API (跨文档消息传输与使用web sockets API来通过socket端口传递数据)

跨文档消息传输：可以在不同 网页文档、不同端口、不同域之间进行消息传递
web sockets api:可以让客户端与服务器端通过socket端口来传递数据，好处：实现数据推送技术。服务器不再被动等待客户端发出请求，自要客户端与服务器端建立了一次链接之后，服务器就可以在需要的时候，主动将数据推送到客户端

## 9.1 跨文档消息传输

###9.1.1 跨文档消息传输的基本知识
HTML 5提供了在网页文档之间互相接收与发送消息的功能。使用该功能，只要获取网页所在窗口对象的实例，不仅同源（域+端口号）的web网页之间可以互相通信，甚至可以实现跨域通信

首先，要想接受从其他窗口发过来的消息，就必须对窗口对象的message事件进行监视：
window.addEventListener('message',function(){},flase)

使用window对象的postMessage方法向其它窗口发送消息，该方法定义如下：
	otherWindow.postMessage(message,targetOrigin)//发送的消息文本内容，也可以是任何javascript对象（通过json转换对象为文本），第二个参数为接收消息的对象窗口的URL地址（http://localhost:8080/），可以url地址字符串中使用通配符”*“ 指定全部地址，建议使用准确的URL地址。 otherWindow为要发送窗口对象的引用，可以通过window.open返回该对象，或通过window.frames数组指定序号（index）或名字来返回单个frame所属的窗口对象

### 9.1。2 跨文档消息传输示例
主页面与主页面中的iframe子页面之间的互相通信。首先，主页面向iframe子页面发送消息，iframe子页面接受消息，显示在本页面中，然后小主页面返回消息。最后，主页面接受消息，然后将该消息alert弹出
另外，主页面与iframe子页面被配置在不同域中，以实现跨域通信

	通过window对象的message事件进行监听，可以接收消息
	通过访问message的origin属性，可以获取消息的发送源（http://127.0.0.1）为了不接收其他源恶意发送来的消息，最好对发送给源做个检查
	通过访问message事件的data属性，可以取得消息内容（可以是任何javascript对象）
	使用postMessage方法发送消息
	通过访问message事件的source属性，可以获取消息发送源的窗口对象（准确的说，应该是窗口的代理对象）

如果使用json对下岗的stringify方法将javascript对象转换为文本，使用json对象的parse方法将文本还原为javascript对象，则任何javascrip对象都可以通过这种方式在网页文档与网页文档之间、端口与端口之间、域与域之间互相传递。
现在 Firefox 4，safari 4，chrome 8，opera 10都支持这种跨文档的消息传递方式

#9.2 web Sockets通信

###9.2.1 web sockets通信基础知识
	web sockets是HTML 5提供的在Web 应用程序中客户端与服务器端之间进行的非HTTP的通信机制。它实现了用HTTP不容易实现的服务器端的数据推送等只能通信技术
	web sockets API可以在服务器与客户端之间建立一个非HTTP的双向连线，这个连线是实时的，也是永久的，除非显示关闭。这意味着服务器相向客户端发送数据时，可以立即将数据推送到客户端的浏览器中，无须重新建立连接。只要客户端有一个被打开的socket并且与服务器建立了连接，服务器就剋吧数据推送到这个socket上，服务器不再需要轮询客户端的请求，从被动转为主动

### 9.2.2 使用web sockets api
	web Sockets的API本身非常简单。将URL字符串作为参数，然后调用websocket对象的构造器来建立与服务器之间的通信连接，如：
	var webSocket=new WebSocket("ws://localhost:8005/socket");

	url字符串必须以ws或wss（加密通信时）文字作为开头，这个URL字符串被设定好之后，在javascript脚本中可以通过访问WebSocket对象的url属性来承诺更新获取。
	通信连接建立好之后，就可以进行客户端与服务器端的双向通信了。使用websocket对象的send方法对服务器发送数据，只能发送文本数据，蛋壳使用json甲肮任何javascript对象转换为本本数据进行发送，使用send方法如下：
		webSocket.send('data');

	通过获取onmessage事件句柄来接受服务器传过来的数据，
		webSocket.onmessage=function(event){
			var data=event.data;
		}
	通过获取onopen事件句柄来监听socket打开事件，如下：
	webSocket.onopen=function(event){
		
	}

	通过获取onclose事件句柄来监听socket的关闭事件，如下：
	webSocket.onclose=function(event){
		.....
	}

	通过close方法来关闭socket，切断通信连接
		webSocket.close();
	另外，可以通过读取readyState的属性值来读取WebSocket对象的状态。readyState属性存在以下几种属性值：
	connection (数字值为0)，表示正在连接
	open（数字为1），表示已建立连接
	closing（数字值为2），表示正在关闭连接
	closed（数字职位3）  连接已经关闭或不可用

### 9.2.3 web sockets api使用示例

WebSocket connection to 'ws://localhost/9_websocket.html' failed: Error during WebSocket handshake: Unexpected response code: 404
WebSocket connection to 'ws://localhost/' failed: Error during WebSocket handshake: Unexpected response code: 200