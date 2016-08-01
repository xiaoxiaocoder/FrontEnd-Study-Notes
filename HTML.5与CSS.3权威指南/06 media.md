#第6章  多媒体播放
-	6.1 video元素与audi元素的基础知识
-	6.2属性
-	6.3 方法
-	6.4 事件

##	6.1 video元素与audio元素的基础知识

###6.1.1 HTML 4页面中播放视频或音频的方法
`
	<object classid="clsid:d27cdb6e-ae6d-1lcf-96b8-444553540000" width="400" height="344" codebase="swflash.cab#version=6,0,40,0">
	<param name="allowFullScreen" value="true"/>
	<param name="allowscriptaccess" value="always"/>
	<param name="allowfullscreen" value="true"/>
	<embd type="application/x-shockwave-flash"
		width="425"
		height="344"
		src="p.swf"
		allowscriptaccess="always"
		allowfullscreen="true">
	</embd>
</object>
`
缺点:
冗长并且丑陋
需要安装Flash插件
需要结合object元素和embd元素,并且为object元素添加许多属性和参数

###6.1.2 HTML 5页面中播放视频或音频的方法  video,audio
-	video 专门用来播放视频或电影
-	audio 专门用来播放网络上的音频数据
播放视频
`
	<audio src='http://test.mp3'>
	your browser don't support audio element.
	</audio>

`
video 设置长,宽属性
`
	<video width="640" height="360" src="http://lulingniu/test.mp3">
		you browser don't support video element.
	</video>
`
还可以通过source元素来为同一个媒体数据指定多个播放格式与编码方式,以确保浏览器可以从中选择一种自己支持的播放格式进行播放,浏览器的选择顺序为代码的书写顺序,浏览器会从上往下判断自己对该播放器格式是否支持,直到选择自己支持的播放格式.
`
	<video>
		<!---ogg the ora,quicktime,mp4->
		<source src="sample.ogv" type='video/ogg;codecs="theora,vorbis"'>
		<source src="sample.mov" type="video.quicktime">
	</video>
`
src属性是指播放媒体的url地址,type表示媒体类型,属性值为播放文件的mime类型,codes参数表示所使用的媒体的编码格式.type属性是可选的,但最好不要省略type,否则浏览器会从上往下是选择时无法判断自己能不能播放而先行下载一小段视频,造成浪费宽带和时间.[具体浏览器支持格式请 参考文档]

## 6.2 属性
`	audio,video元素都具有的属性
-	src: 指媒体播放诗句的url地址
-	autoplay: 在该属性指定媒体是否在页面加载后自动播放.  <video src="sample.mov" autoplay></video>
-	preload: 指定视频或音频是否预加载,如果使用,浏览器会预先缓冲,有三个可选值:none(不预加载),metadata(预加载媒体元数据[字节数,第一帧,播放列表,持续时间])与auto(全部预加载)    <video src="sample.mov" preload="auto"></video>
-	poster(video元素独有属性) 视频不可用是,替代的图片  <video src="sample.mov" poster="cannotuse.jpg"></video>
-	loop:是否循环播放		<video src="sample.mov" autoplay loop></video>
-	controls 指定是否为视频或音频添加浏览器自带的播放用的控制条.控制条具有播放,暂停,按钮 <video src="sample.mov" controls></video>
-	width与height(video元素独有属性)		<video src="sample.mov" width="500" height="500"></video>
-	error属性  在读取,使用媒体数据的过程中,在正常情况下,video元素或audio元素的error属性为null,但只要出现错误,error属性将放回一个MediaError对象,该对象的code返回对应的错误状态.code
	MEDIA_ERR_ABORTED( 1 ): 媒体数据的下载过程由于用户的操作原因被终止
	MEDIA_ERR_NETWORK (2):  媒体资源可用,在下载过程中出现网络错误,媒体数据的下载过程被终止
	MEDIA_ERR_DECODE(3): 媒体资源可用,在解码时出错
	MEDIA_ERR_SRC_NOT_SUPPORTED (4) 媒体格式不支持
	error属性为只读属性

-	networkState属性 可以使用video元素或audio元素的networkState属性读取当前网络状态[只读属性]
		NETWORK_EMPTY(0): 元素处于初始状态
		NETWORK_IDLE(1): 浏览器已选择好用什么编码格式来播放媒体,但尚未建立网络连接
		NETWORK_LOADING(2):媒体数据加载中
		NETWORK_NO_SOURCE(3):没有支持的编码格式,不执行加载
-	currentSrc属性  可以使用video元素或audio元素的currentSrc属性来读取播放中的媒体数据的URL地址[只读属性]
-	buffered属性	可以使用video或audio元素的buffered属性来返回一个对象,该对象实现TimeRangers接口,以确认浏览器是否已缓存媒体数据.TimeRanges对象表示一段时间.大多数情况下,TimeRanges对象表示的时间范围是一个单一的以0开始的范围,但是如果浏览器发出Range Requests请求,这是Time Ranges对象表示的时间范围是多个时间范围.	 TimeRanges对象具有一个length属性,表示有多个时间范围,大多数情况下存在时间范围时,该值为1,不存咋时间范围时,为0.TimeRanges对象还具有两个方法,TimeRanges.start(index)与TimeRanges.end(index)[第几个时间范围]. 大多数情况下,将index值设为0就可以.当用videoElement.buffered语句来实现TimeRanges接口时,TimeRanges.start(0)表示当前缓存区内从媒体数据的什么时间开始进行缓存,TimeRanges.end(0) 表示当前缓存区内的结束时间.  [只读属性]
-	readyState[只读属性]
	-	HAVING_NOTHING (0) :没有获取到媒体的任何信息,当前播放位置没有可播放数据
	-	HAVING_METADATA(1) :已经获取到足够媒体数据,但是当前播放位置没有有效的媒体的数据(获取到的媒体数据无效,不能播放)
	-	HAVING_CURRENT_DATA(2):当前播放位置已经有数据可以播放,但没有获取到可以让播放器前进的数据.当媒体为视频时,当前帧的数据已经获得,但还没有获取到下一帧的数据,或者当前帧已经是播放的最后一帧
	-	HAVING_FUTURE_DATE(3):当前播放位置已经有数据可以播放,而且也获取到了可以让播放器前进的数据.当媒体为视频时,意思是当前帧的数据已经获得,而且也获得到了下一帧的数据,当前帧是播放的最后一帧,readyState属性不可能为HAVING_FUTURE_DATA
	-	HAVING_ENOUGH_DATA(4):当前播放器位置已经有数据可播放,同时也获取到了可以让播放器前进的数据,而且浏览器确认媒体数据以某一种速度进行加载,确保有足够的后续数据进行播放.
-	seeking属性与seekable属性[只读属性]
	-	可以使用video,audio元素的seeking属性返回一个布尔值,表示浏览器是否正在秋影某一特定播放位置的数据,true表示浏览器正在请求的数据,false表示浏览器已停止请求.
	-	可以使用video元素或audio元素的seekable属性来返回一个TimeRanges对象,该对象表示请求到的数据的时间范围,当媒体为视频时,开始时间为请求到视频数据第一帧的时间,结束时间为请求到的视频数据最后一帧的时间

-	currentTime,startTime,duration属性(currentTime 为可读写属性,其余为只读属性)
	-	可以使用video元素或audio元素的currentTime属性来读取媒体的当前播放位置,也可以通过修改currentTime属性来修改当前播放位置.如果修改的位置上没有可用的媒体数据时,将抛出INVALID_STATE_ERR异常;如果修改的位置超出了浏览器在一次请求中可以请求的数据范围,将抛出INDEX_SIZE_ERR异常
	-	可以使用video,audio元素的startTime属性来读取媒体播放的开始时间,通常为0.
	-	可以使用video,audio元素的duration属性来读取媒体文件纵的播放时间.

-	played,paused,ended[只读属性]
	-	使用video,audio元素的played属性返回一个TimeRanges对象,从该对象中可以读取媒体文件的已播放部分的时间段.开始时间为已播放部分的开始时间,结束时间为已播放部分的结束时间.
	-	使用paused属性返回一个boolean值,表示是否处于暂停
	-	使用end输习惯返回一个boolean值,表示是否播放完毕

-	defaultPlaybackRate属性与playbackRate属性
	-	defaultPlaybackRate属性修改或读取媒体默认播放速率
	-	使用playbackRate属性读取或修改媒体当前的播放速率

-	volumn属性与muted属性
	-	使用volumn属性读取或修改媒体的播放音量,范围0到1,0为静音,1为最大
	-	使用muted属性读取或修改媒体的静音状态,布尔值,true表示静音,false表示非静音

##6.3 方法
video元素和audio元素都具有以下四种方法:
-	play方法   使用play方法来播放媒体,自动将元素的paused属性的值变为false
-	pause方法	暂停播放,自动将元素的paused属性设置为true
-	load方法		使用load方法来重新载入媒体进行播放,自动将元素的playbackRate属性值变为defaultPlaybackRate属性的值,自动将元素的error的值变为null
-	canPlayType 使用canPlayType方法来测试浏览器是否支持指定的媒体类型,该方法的定义如下:
		var support=videoElement.canPlayType('type');
	videoElement表示页面上的video元素或audio元素. 该方法使用一个参数type,该参数的指定方法与source元素的type参数的指定方法相同,都用播放文件的MIME类型来指定,可以在指定的字符串中加上表示媒体编码格式的codes参数.
	该方法返回3个可能值:
	空字符串:表示浏览器不支持
	maybe:表示浏览器可能支持
	probably:确定支持

## 6.4 事件

### 6.4.1 事件处理方式
	video元素或audio元素读取或播放媒体数据时,胡触发一系列事件,如果Javascript脚本来捕捉这些时间,
	事件的捕捉有两种方式:
1. 监听方式  使用video元素或audio元素的addEventListener方法来对事件的发生进行监听,定义:
	videoElement.addEventListener(type,listener,useCapture);
	type:事件名称
	listener:表示绑定的函数
	capture:布尔值,表示该事件响应顺序,如果为true,浏览器采用capture响应方式.为false,浏览器采用bubbing响应方式,一般采用false,默认情况下为false.

`
	video.addEventListener('error',function(){},false);
`
2. 第二种事件处理方式为javascript脚本常见的获取事件句柄的方式.
`
<video id="video1" src="sample.mov" onplay="begin_play();"</video>
function begin_play(){
}
`

### 6.4.2 事件介绍
浏览器在请求媒体数据,下载媒体数据,播放媒体数据一直到播放结束这一系列过程中,会触发哪些事件.
loadstart		浏览器开始在网上寻找媒体数据
progress		浏览器正在获取媒体数据
suspend			浏览器暂停获取数据,但是在下载过程并没有正常结束
abort			浏览器正在下载完全部媒体数据之前终止获取媒体数据,但并不是有错误引起
error			获取媒体数据过程中出错
emptied			video,audio所在网络突然变为未初始化状态(可能引起的原因:
				1. 载入媒体过程中突然发生一个致命错误
				2. 在浏览器正在选择支持的播放格式时,又调用了load方法重新载入媒体)
stalled			浏览器尝试获取媒体数据失败
play			即将开始播放,当执行了play方法时触发,或数据下载后元素被设为autoplay(自动播放)属性
pause			播放暂停,当执行了pause方法时触发
loadedmetadata	浏览器获取完毕媒体的事件长和字节数
loadeddata		浏览器已加载完毕当前播放位置的媒体数据,准备播放
waiting			播放过程由于得不到下一帧而暂停播放(如下一帧尚未加载完毕),但很快就能得到下一帧
playing			正在播放
canplay			浏览器能够播放媒体,但估计以当前播放速率不能直接将媒体播放完毕,播放期间需要缓冲
canplaythrogh	浏览器能够播放媒体,而且以当前播放速率能够将媒体播放完毕,不需要进行缓冲
seeking			seeking属性变为true,浏览器正在请求数据
seeked			seeking属性变为false,浏览器停止请求数据
timeupdate		当前播放位置被改变,可能是播放过程中的自然改变,可能是被人为改变,或由于播放不能连续而发生的跳变
ended			播放结束后停止播放
ratechange		defaultplaybackRate属性()默认播放速率或playbackRate属性(当前播放速率)被改变
durationchange	播放时被改变
volumnchange	volumn属性(音量)被改变或muted属性(静音状态)被改变