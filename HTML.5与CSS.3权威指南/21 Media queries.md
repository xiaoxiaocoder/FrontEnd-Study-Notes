##21.1 根据浏览器的窗口大小来选择使用不同的样式
	在CSS中,与媒体相关的定义从CSS2.1开始, 媒体类型:显示器,便携设备,电视机等;
Firefox,safari,chrome,oprea


##21.2 在iPhone中显示
在 iphone 3GS和iPod touch中使用的safari浏览器对CSS 3媒体查询表达式提供支持
iphone 320px*480px
利用meta标签在页面中指定safari刘浏览器在处理本页面时按照多个像素的窗口来进行.
<meta name="vierport" content="width=600px"/>



##21.3 Media Queryires
@media 设备类型 and (设备特性) {样式代码}

Media Queries

可以指定的值					设备类型
all							所有设备
screen						电脑显示器
print						打印用纸或打印预览设备
handheld					便携设备
tv							电视机类型的设备
speech						语音和音频合成器
braille						盲人用点触觉回馈设备
embossed					盲文打印机
projection					各种投影设备
tty							使用固定密度字母栅格的媒介,比如电传打字机和终端

13设备特性
特性				可指定值						是否允许使用min/max前缀			特性说明
width			带单位的长度数值(400px)		允许								浏览器窗口的宽度
height			带单位的长度数值(200px)		允许								浏览器窗口的高度
device-width		带单位的长度数值(400px)		允许								设备屏幕分辨率的宽度值
device-height	带单位的长度数值(200px)		允许								设备屏幕分辨率的高度值
orientation		两个值(portrait/landscape)	不允许							浏览器窗口的方向是纵向还是横向.当窗口的高度值大于等																			于宽度值,该特性值为portrait,否则为landscape
aspect-ratio		比例值(16/9)					允许								浏览器窗口纵横比,比例值(浏览器窗口的宽度/高度值)	
device-aspection-ratio 比例值(16/9)			允许			屏幕分辨率纵横比,比例值为设备屏幕分辨率的宽度值/高度值
color			整数值						允许			设备使用多少位的颜色值,如果不是彩色设备,该为0
color-index		整数值						允许			彩色表中的色彩数
monochrome		整数值						允许			单色帧缓冲器每像素的字节数
resolution		分辨率(300dpi)				允许			设备的分辨率
scan			progressive,interlace		不允许		电视设备类型设备的扫描设备(progressive,逐行扫描  interlace,隔行扫描)
grid			0/1							不允许		设备时基于栅格还是基于位图.栅格为1,否则为0		

@media screen and (max-width:639px)	{}
@media handheld and (min-width:360px),screen and (min-width:480px){}
可以在表达式中加上not关键字或only关键字.not关键字表示对后面的表但是执行取反操作
/*
	对not后面语句执行取反操作
样式代码将被使用在除便携设备之外的其他设备或非彩色便携设备
*/
@media not handeld and (color){}

//样式代码将被使用在所有非彩色设备中
@media all and (not color)

only关键字的作用是,让那些不支持Media Queries但是能够读取Media Type的设备的浏览器将表达式中的样式隐藏起来
@media only screen and (color) {}

对于支持Media Queries的设备来说,将能够正确应用样式,就像only不存在一样.
对于不支持Media Queries但是能够读取Media Type的设备(譬如IE8 只支持 "@media screen"),由于限度去到时only而不是screen,将忽略这个样式.

对于不支持Media Queries的浏览器(譬如IE8 之前的浏览器)来说,无论是否有only,将忽略这个样式
CSS 3中Media Queries模块中也支持对外部样式表的引用,使用方法:
@import url(color.css) screen and (min-width:1000px);
<link rel="stylesheet" type="text/css" media="screen and (min-width:1000px)" href="style.css"/>