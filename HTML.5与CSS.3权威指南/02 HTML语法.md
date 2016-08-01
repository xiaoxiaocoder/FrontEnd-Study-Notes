#HTML5 语法
***
>	**DOCTYPE 及编码字符**
>	
>	**大小写都可以**
>	
>	**布尔值**
>	
>	**省略引号**
>	
>	**可以省略的标签**

---
1. DOCTYPE:	`<!DOCTYPE html>`
2. 字符编码: `<meta charset="utf-8">`
3. 大小写(兼容其他版本): `<input type="text" value="text"/>;	<Input type="Text" value="" />`
4. 布尔值:`<input type="checkbox" checked value="true"/>ON`
5. 省略引号:`<input type=checkbox value='te'/>`
6. 可以省略的标签
	1. 不允许写结束符的标签(当前标签结束时以/结尾,而不是成对出现):area,basebr,col,command,embed,hr,input,keygen,link,meta,param,source,track,wbr
	2. 可以省略结束符的标签(可以只写前面,不成对出现): li,dt,dd,p,rt,optgroup,colgroup,tread,tbody,tr,td,th
	3. 可以完全省略的标签(可删除标签):html,head,body,colgroup,tbody

---
##新增和删除标签
**新增标签**

*	结构标签
	1.	section 表示页面中的一个内容区块,比如章节,页眉,页脚或页面的其他部分,可以和h1,h2...等标签结合起来使用,表示文档结构.如`<section>xxx</section> 等同于html4中的 <div>xxx</div>`
	2.	article 表示页面中一块和上下文不相关的独立内容,比如一篇文章
	3.	aside	表示和article标签内容之外的,与article内容相关的辅助信息
	4.	header	表示页面中的一个内容区块或整个页面的标题	
	5.	nav		表示页面中的导航链接部分
	6.	footer  表示页面或页面中一个内容区块的注脚,一般来说,也会包含创作者的姓名,创作日期,联系方式等.
	7.	hgroup	表示对整个页面或页面中的一个区块的标签进行组合.
	8.	figure	表示一段独立的流内容,一般表示文档主题内容中的一个独立单元,使用figcaption为figure标签添加标题.例如:`<figure><figcaption>title</figcaption><p>....</p></figure> 等同于HTML4中 <dl><h2>title</h2><p>.....</p></dl>`

*	表单标签
*	媒体标签

		*	video标签 定义视频,电影或其他视频流.例如:<video src="video.ogg" controls="controls">videp</video>
		HTML4中可以这样写 `<object type="video.ogg" data="move.ogv"><param name="src" video="movie.ogg"><object>`		
		
		*	audio标签 定义音频. 如音乐或其他音频流. <audio src="someaudio.mav">audio</audio> HTML4中`<object type="application/ogg" data="someaudio.mav"><param name="src" value="someaudio.mav"></object>`
		
		*	embed标签  嵌入内容(包括各种媒体:mdi,mav,au,aiff,mp3,flas等)例: <embed src="flash.swf"/>HTML4中<object data="flash.swf" type="application/x-shocwave-flash"></object>
		
*	其他功能标签

		mark标签			标注,显示成高亮样式
		command 标签		命令菜单(浏览器支持不太好)
		progress标签		进度条<progress max="100" value="85"><span>85</span>%<progress>
		details标签		点击标题,内容收缩
		time标签			标注标签,给搜索引擎使用,datetime中t表示时间和日期分隔符,z表示utc标准时间				格式, publishdate文章发布时间
		datalist标签		input 通过id和datalist结合,实现autocomplete效果
		ruby标签			注释,rt注释内容,rp浏览器不支持显示该内容<ruby>繁 <rt></rt><rp>xx</					p></ruby>
		keygen标签		加密,可以选择加密方式
		rt标签
		output标签		显示结果计算的输出值
		wbr标签			(软)换行,足够宽是不换行,窄时换行
		source标签
		canvas标签		标签,结合js进行图形绘制
		menu标签 


	


**废除标签**

*	可以用css代替的标签
*	不在使用frame
*	只有个别浏览器支持的标签
*	其他不常用标签

---
##新增属性
表单属性
链接属性
其他属性
删除属性
多余属性

* <html manifest="cache.mainfest"> manifest 表示该页面应用的离线应用文件的文件名
* <meta charset="utf-8"/> charset 新增属性
* <meta http-equiv="param" content="no-cache"/> 不是新增属性,表示禁用页面缓存
* <link rel="icon" href="demo_icon.gif" type="image/gif" sizes="16*16" /> 定义页面的favicon图标  新增属性 sizes 可设置图标大小
* <base href="http://localhost/" target="_blank"/> 新增target属性,  表示页面内所有的链接都会已元地址 href开头, 点击链接时,打开新的页面
* <script defer src="jquery.js" onload="alert('a')"> </script> defer之前版本中已有,如果script中不添加defer时,页面脚本从上往下先下载,再执行,然后接着下一个下载,执行....  如果 加上defer后表示该脚本下载完后并不立即执行,而是等到页面加载完成后才开始执行
*defer 推迟执行  async 异步执行
<script async  src="boostrap.js" onload="alert('b')"></script> 加上async表示浏览器异步下载该脚本,下载完成后立即执行,在下载的过程中,浏览器同步向下解析
* `<a media="handheld" href="#">手持</a> <a media="tv" href="#"> 电视</a> <a href="http://www.baidu.com" hreflang="zh" ref="external">imooc</a>` media属性配置为handheld时,表示当网页所在设备为手持设备时,超链接对其优化,为tv时表示为电视进行优化;  hreflang 表示页面使用语言, external表示为外部链接
* `<ol start="50" reversed> <li>coffe</li><li>tea</li><li>milk</li></ol>` start 表示起始值,reversed表示倒序 显示内容时 50 milk,49 tea,48 coffe
* `<menu type="toolbar" label="menu"><li><input type="checkbox">Red</li><li><input type="textbox">Blue</li></menu>` type可分为toolbar(工具条),list(列表),contextmenu(右键上下文菜单) 
* `<div><style type="text/css" scroped> h1{xxx} h2{...}</style> <h1>xxx</h1><h2>www</h2></div>` scroped表示该样式只在当前区块(div)中起作用
* `<iframe seamless srcdoc="<h1>Hellp</h1>" sandbox src="http://www.baidu.com"></iframe> `sameless 表示框架没有边框和边距 srcdoc当配置该属性,就只显示该属性值,src属性忽略. sandbox 规定iframe内嵌框架的安全级别(严格级别:禁止提交表单,禁止运行js,表示内嵌页面和该页面内的其他元素不是相同的源,异源),可赋值,默认异源.allow-forms允许提交表单,allow-same-orign 允许同源,allow-scripts允许执行脚本,allow-top-navigation:允许脚本跳转.

---
##全局属性
*	`<form data-type="comment"><label hidden>ce</lable><textarea tabindex="1" spellcheck="true"></textarea></form><table contenteditable="true"><tr><td>1</td><td>1</td></tr></table>` 
*	data-*自定义属性;
*	hidden 表示元素隐藏; 
*	spellcheck 语法检查;
*	tabindex 按tab键时顺序; 
*	contenteditable表示内容可编辑
*	`<script type="text/javascript>window.document.designMode="on";</script>"` document.designMode="on" 表示页面内的所有元素都可以进行编辑 

---
##语义化标签
*kkk 


