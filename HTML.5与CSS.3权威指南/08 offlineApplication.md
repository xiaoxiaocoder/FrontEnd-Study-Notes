#8 Offline Application

HTML 5中,提供了一个供本地缓存使用的API.使用该API,可以实现离线Web Application的开发.
学习内容:
	离线web 应用程序,为什么要开发离线 web应用程序
	本地缓存,本地缓存与网页缓存的区别,
	manifest文件,怎样在manifest文件中指定哪些内容需要进行本地缓存,哪些内容不需要

	首次访问网站后,再次访问网站而manifest文件没有更新,再次访问网站而manifest文件已经被更新时浏览器与服务器的交互过程,在这些过程中如何进行本地缓存的更新
	
	掌握进行本地缓存时所使用到的applicationCache对象,怎样利用这个对象的swapCache方法来手工更新本地缓存,控制什么时候进行本地缓存的更新,以及在浏览器与服务器进行交互的过程中依次触发的applicationCache对象的事件.

##8.1 Offline Web Application

###8.1.1 新增的本地缓存
 web应用程序已经变得越来越复杂,很多领域都在利用这web应用程序.但是,一个致命的缺点:如果用户没有和Internet建立链接,就不能用该web application.
	在HTML 5中,新增一个API,它使用一个本地缓存机制很好地解决了这个问题,为离线web应用程序的开发提供了可能性.
	为了让web application在离线状态下能正常工作,就鼻血要把所有构成web application的资源文件,诸如html文件,css文件,javascript文件放在本地缓存中,When server can't connect to thes browser,it will use local cache to make web application run as normal.

###8.1.2 localstorage与pageStorage的区别
	首先:本地缓存时整个web application服务的,而浏览器网页缓存只服务于单个网页.
	其次:网页缓存时不安全,不可靠的,因为我们不知道在网站中到底缓存了哪些网页,以及缓存了网页上的哪些资源.而本地缓存我们可以控制对那些内容进行内容缓存,不对哪些内容进行缓存,开发人员还可以用编程的手段来控制缓存得更新,利用缓存对象的各种属性,状态和事件来开发出更强大的离线 app.

##8.2	manifest文件
web application的本地缓存是通过每个页面的manifest文件来管理的.manifest文件中以清单的形式列举了需要被缓存或不需要被缓存的资源文件的文件名称,以及这些资源文件的访问路径.可以为每个页面单独指定一个manifest文件,也可以对整个web app指定一个总的manifest文件. 
manifest文件示例
`
	#文件的开头必须写cache manifest
	cache manifest
	#manifest 版本号
	#version 7
	cache:
	other.html
	hello.js
	images/myphoto.jpg
	NetWork:
	http://liulingniu/nooffine
	NotOffline.asp
	*
	FALLBACK:
	online.js locale.js
	CACHE:
	newhello.html
	newhellp.js
`
第一行"cache manifest"把本文件的作用告知浏览器,挤兑恩地缓存中的资源问津进行具体设置.
真正运行或测试离线web app时,需要对服务器进行配置,让服务器支持text/cache-manifest这个MIME类型(HTML 5中对公mainifest文件的MIME类型是text/cache-manifest).例如在对Apache服务器进行配置的时候,需要找到{apache_home}/conf/mime.types文件,并在文件最后追加:
text/cache-manifest manifest

在IIS服务器中配置:
1	右键选择默认网站或需要添加类型的网站,属性
2	选择"HTTP 头"标签
3	在MIME映射下,单机文件类型按钮
4	在打开的MIME类型的对话框中单机新建按钮.
5	在关联扩展名文本框中输入manifest,内容属性中输入text/cache-manifest,确定

资源文件分为三类:
cache:
	指定需要被缓存在本地的资源文件.为某一个页面指定需要本地缓存的资源文件时,不需要把这个页面指定在cache类别中,因为如果一个页面具有manifest文件,浏览器会自动对这个页面进行本地缓存
network:
	显示指定不进行本地缓存的文件,文件只有当客户端与服务端建立链接的时候才能访问.在该类别中的"*"为通配符,表示没有在本manifest文件中指定的资源文件都不进行本地缓存.
FALLBACK:
	每行自定两个资源文件,第一个资源文件表示在线访问使用的文件,第二个文件为不能离线时访问的备用资源文件.

每个类别都是可选的,但是如果文件开头没有指定类别值直接书写资源文件的时候,浏览器把这些资源文件都视为cache类别,知道看见稳重第一个被书写出来的类别为止.
允许在同一个manifest文件中重复书写同一类别
为了让浏览器能够正常阅读该文件,需要在web app在html标签manifest属性中指定manifest文件的url地址:
`<!--单独指定-->
<html mainifest="hello.manifest">
...
</html>

<!--一个总的manifest文件-->
<html manifest="global.manfiest">
	
</html>
`

##8.3 浏览器与服务器的交互过程
比如:http://xx,以index.html为主页,该主页使用index.manifest文件为manifest文件,在该文件中请求本地缓存index.html hello.js hello1.jpg hello2.jpg,首次访问http://xx时,交互过程如下:
1	浏览器请求访问http://xx
2	服务器返回index.html页面
3	浏览器解析index.html页面,请求页面上所有的资源文件,包括HTML文件,图像文件,css文件,javascript文件以及manifest文件
4	浏览器返回所有资源文件
5	浏览器处理manifest文件，请求manifest中所有要求本地缓存的文件，包括index.html页面本身,即使刚才已经请求过这些文件.如果你要求本地缓存所有文件,将时一个较大的重复请求过程
6	服务器返回所有要求本地缓存的文件
7	浏览器对本地缓存进行更新,存如包括页面本身在内的所有要求本地缓存的资源文件,病触发一饿事件,通知本地缓存更新.

现在浏览器已经把本地缓存更新完毕,如果再次大爱浏览器访问http://xx,而且manifest文件没有被修改过,它们的交互过程如下:
1	刘蓝溪再次请求访问http://xx
2	浏览器发现该页面被本地缓存,于是使用本地缓存的index.html页面
3	浏览器解析index.html页面,使用所有本地缓存中的资源文件
4	浏览器向服务器请求manifest文件
5	服务器返回304代码,通知浏览器manifest未发生变化.

如果再次打开浏览器时manifest文件已经被更新过了,那么浏览器与服务器之间的交互过程如下:
1	浏览器再次请求访问http://xx
2	浏览器发现这个页面被本地缓存,于是使用本地缓存中的index.html页面
3	浏览器解析index.html页面,使用所有本地缓存中的资源文件
4	浏览器向服务器请求manifest文件
5	服务器返回更新过的manifest文件
6	浏览器处理manifest文件,发现该文件已经被更新,于是请求所有进行本地缓存的资源文件,包括index.html
7	浏览器返回要求进行本地缓存的资源文件
8	浏览器对本地缓存进行更新,存入所有新的资源文件.并且触发一个事件,通知本地缓存被更新

##8.4 applicationCache对象
applocationCache对象代表了本地缓存,可以用他来通知用户本地混村中已经被更新,也允许用户手工更新本地缓存
当浏览器对本地缓存已经被更新,装入心的资源文件时,会触发applicationCache对象的updateready事件,通知本地缓存已被更新.可以利用这个事件告诉用户本地缓存已经被更新,用户需要手工刷新页面来割刀最新版本的应用程序.
`
applicationCache.onUpdateReady=function(){
	//本地缓存已被更新,通知用户
	alert("本地缓存已被更新,您可以刷新页面得到本地程序的最新版本.");
}
`
另外,您可以通过applicationCache的swapCache方法来控制如何进行本地缓存的更新几更新时机.

###8.4.1 swapCache方法
	swapCache方法用来手工执行本地缓存的更新,它只能在applicationCache对象的updateReady事件被触发时调用,updateReady事件只有在服务器上的manifest文件被更新,并且吧manifest文件中所要求的资源文件下载到本地后触发,===本地缓存准备被更新. 该事件触发后,可以用swapCache方法来手工进行本地缓存的更新.
那些场合需要使用该方法:
	首先,如果本地缓存的容量非常大(超过100MB),本地缓存的更新工作将需要相对较长的时间,而且会把浏览器锁住.此时,最好又一个提示,告知用户正在进行本地缓存的更新.
`
applicationCache.onUpdateReady=function(){
	//本地缓存已更新,通知用户.
	alert('正在更新本地缓存');
	applicationCache.swapCache();
	alert('本地缓存已被更新,您可以书安心页面来得到本程序的最新版本');
}
`
	如果不调用swapCache(),本地缓存会在再次打开页面时更新,如果调用swapCache,会立即更新.因此可以使用confirm通知用户选择更新时机

applicationCache.update,该方法的作用时检查服务器上的manifest文件是否有更新,如果又更新,浏览器会自动下载manifest文件中所有请求本地缓存的资源文件,当这些资源文件下载完毕时,会触发updateReady事件

### 8.4.2 applicationCache对象的事件
	applicationCache对象除了具有update,swapCache方法外，还具有一系列事件
	以下，是在浏览器与服务器的交互过程的中，触发的一系列事件：
首次访问http://xx网站
1.	浏览器请求访问http：//xx
2.	服务器返回index.html网页
3.	浏览器发现该网页具有manifest属性，触发checking事件，检查manifest文件是否存在。不存在时，触发error事件，表示manifest文件未找到，不执行步骤6开始的交互过程
4.	浏览器解析index.html网页，请求页面上的所有资源文件
5.	服务器返回所有资源文件
6.	浏览器处理manifest文件，请求manifest中所有要求本地缓存的文件，包括index.html页面本身，即使刚才已经请求国该文件，如果要求本地缓存所有文件，将是一个比较大的重复请求过程
7.	服务器返回所有要求本地缓存的我呢见
8.	浏览器触发downloading事件，然后开始下载这些资源。在下载的同时，周期性的触发progress事件，开发人员可以用编程手段获取多少文件已经被下载，多少文件仍然处理下载队列
9.	下载结束后触发cached事件，表示首次缓存成功，存入所有要求本地缓存的资源文件。

再次访问http://xx网站时，步骤1~5同上，在步骤5执行之后，浏览器将核对manifest文件是否被更新，若没有被更新，触发noupdate事件，步骤6开始的交互过程不会被执行，如果被更新了，将继续执行后面的步骤，在步骤9中不处罚cached事件，而是触发updateready事件，表示下载结束，可以通过刷新页面来使用更新后的本地或调用swapCache方法来立刻使用更新后的本地缓存
另外，在王文缓存名单时如果返回一个HTTP 404（未找到）错误，或410（永久消失），则触发obsolete事件

整个过程中，如果任何与本地缓存有关的处理中发生错误的话，都会触发error事件。可能会触发error事件的情况分为以下几种：
	缓存名单返回一个HTTP404错误，或410错误
	缓存名单被找到且没有更改，但引用缓存名单的html页面不能正确下载
	缓存名单被找到切被更改，但浏览器不能下载某个花黁名单中列出的资源
	开始更新本地缓存时，缓存名单再次被更改