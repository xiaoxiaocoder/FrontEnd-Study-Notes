##第5章 绘制图形
学习内容:
>学习掌握canvas元素的基本概念,学会如何在页面上放置一个canvas元素,如何使用canvas元素绘制出一个简单矩形
>掌握使用路径的方法,能够利用路径绘制出圆形与多边形
>掌握简便图形的绘制方法,学会图形变形,图形缩放,图形组合,以及给图形绘制阴影的方法.
>掌握在Canvas画布中使用画像的方法,能够在画布中绘制图像,复制图像,平铺图像,裁剪图像,以及使用图像中的像素来进行图像处理的方法.
>掌握如何在画布中绘制文字,给文字加上边框的方法
>掌握如何保存及恢复绘图状态,如何保存绘制出来的图形,图像,掌握在画布中制作简单动画的方法.

###5.1 canvas元素的基础知识

####5.1.1 在页面中放置canvas元素
canvas元素时HTML 5中新增的一个重要元素,应该怎样来防止一个canvas元素.
知识点:
首先,应该要指定的是ID,width,height三个属性. 虽然canvas元素时HTML 5中新增元素,但它在页面防止的时候跟其他元素没有太大区别.
在脚本导入之处,使用了<script type="text/javascript" src="srcipt.js" charset="gb2312"></script>语句.通过该语句,在本页面上导入了script.js这个脚本文件.然后使用该脚本文件,进行图形绘制.
在body属性中,使用了onload=draw('canvas');语句.使用脚本文件中的draw函数进行图形描画.

####5.1.2 绘制矩形
用canvas元素绘制图形时,需要经过几道步骤:


- 取得canvas元素
  首先,用document.getElementById()等方法取得canvas对象.因为需要调用这个对象提供的方法来进行图形绘制.
- 取得上下文(context)
  进行图形绘制时,需要用来图形上下文(graphics context),图形上下文中是一个封装了很多绘制功能的对象.需要使用canvas对象的getContext方法来获得图形上下文.在draw函数中,将参数设为"2d".(不能设置为3d,4d)
- 填充与绘制边框
  用canvas元素绘制图形的时候,有两种方式-----填充(fill)与绘制边框(stroke).填充(填满图形内部),绘制边框(只绘制图形的外框)
- 设定绘制样式(style)
  在进行图形绘制的时候,首先要设定好绘图的样式(style),然后调用有关的方法进行图形的绘制.所谓绘图的样式,主要是针对图形的颜色而言.但并限于图形的颜色.
  设定填充图形的样式
	fillStyle属性-----填充的样式,在该属性中填入填充的颜色值.
  设定图形边框的样式
	strokeStyle-------图形边框的样式.在该属性中填入边框的颜色值.
- 指定线宽
  使用图形上下文对象的lineWidth属性设置图形边框的宽度.在绘制图形的时候,任何直线都可以通过lineWidth来指定直线的宽度.
- 指定颜色值
  绘图时填充的颜色或边框的颜色分别通过fillStyle属性与storkeStyle属性来指定.颜色值使用的是普通样式表中使用的颜色值.比如:red,blue或"#FF0000"
  另外,也可以通过rgb(red,green,blue)或rgba(red,green,blue,透明度)来指定颜色
- 绘制矩形
  分别使用fillRect方法与strokeRect方法来填充矩形和沪指矩形边框.
 context.fillRect(x,y,width,height);	
 context.strokeRect(x,y,width,height);
 context指的是图形上下文对象.x值矩形起点横坐标,y矩形起点的纵坐标.坐标原点为canvas画布的最左上角,width指矩形长度,height指矩形的高度----通过这四个参数,矩形的大小同时被决定了.
- 通过上述步骤:通过getContext来取得图形上下文呢,通过fillStyle属性与strokeStyle属性来指定颜色,通过fillRect方法与strokeRect方法来绘制图形,就可以绘制出简单的图形了.
context.clearRect(x,y,width,height)擦出指定区域内的图形,使得该区域编程透明.

###5.2使用路径
5.2.1 绘制圆形
 开始创建路径
 创建图形的路径
 路径创建完成后,关闭路径
 设定绘制样式,调用绘制方法,绘制路径(使用路径勾勒图形轮廓,然后设置颜色,进行绘制.)

绘制圆形步骤:
- 开始创建路径
  	context.beginPath(); //每次创建时都要调用beginPath()函数
- 创建圆形路径
	创建圆形路径时,需要使用图形上下文对象的arc方法.
	context.arc(x,y,radius,startAngle,endAngle,anticlockwise);(x为绘制圆形的起点横坐标,y为绘制圆形的起点纵坐标,radius为圆形半径,startAngle为开始角度,endAngle为结束角度.anticlockwise为是否按顺时针方向进行绘制)
	arc方法不仅可以用来绘制圆形,也可以用来绘制圆弧(必须指定开始角度与结束角度).
- 关闭路径
  路径创建完成后,使用图形上下文对象的closePath()方法关闭
  context.closePath();//将路径关闭后,路径的创建工作就完成了,但只是路径创建完毕而已,还没有真正绘制任何图形.
- 设定绘制样式,进行图形绘制. 这部分代码如下:
  context.fillStyle='rgba(255,0,255,0.25)';
  context.fill();
  使用创建好的路径绘制图形,在指定绘制样式时,与绘制矩形的方法一样,使用fillStyle and strokeStyle方法.
  绘制图形的时候,使用了fill方法(stroke). 这两个方法的功能分别为"填充图形"与"绘制图形边框".因为路径已经决定了图形的大小,所以就不需要在该方法中使用参数来指定图形的大小
###5.2.2 如果没有关闭路径会怎样
  在画布中先是绘制一个深红色的半径最小的圆,然后每次半径变大的同时,圆的颜色方法也在逐渐变淡.
  原因:如果不关闭路径,已经创建的路径会永远保留着,就没用fill方法与stroke方法在页面上将图形已经绘制完毕,路径都都不会消失.因此,如果把"使用路径进行绘制"这个方法进行循环,创建的图形会一次又一次的进行重叠.

###5.2.3 moveTo与lineTo
   绘制直线时,一般会用到moveTo与lineTo两种方法.
- moveTo(x,y)
  moveTo方法的作用是将光标移动到指定坐标点,绘制直线的时候以这个左边点为起点.x表示指定坐标点的横坐标,y表示指定坐标点的纵坐标
- lineTo(x,y)
  lineTo方法在moveTo方法中指定的直线起点与参数中指定的直线重点之间绘制一条直线.x表示直线重点的横坐标,y表示直线重点的纵坐标绘制完成后,光标自动移动到lineTo犯法参数所指定的直线终点
  因此,创建路径时,使用moveTo将光标移动到指定的直线起点,然后使用lineTo方法在直线起点与直线终点之间创建路径,然后将光标移动到直线终点.如此往复

###5.2.4 使用bezierCurveTo绘制贝济埃曲线
 绘制贝济埃曲线,需要使用bezierCurveTo方法该方法可以说是lineTo的曲线版,将从当前坐标点到指定坐标点中间的贝济埃曲线追加到路径中.
context.bezierCurveTo(cp1x,cp1y,cp2x,cp2y,x,y)cp1x 第一个控制地啊的横坐标,cp1y为一个第一个控制点的纵坐标,cp2x为第二个控制点横坐标,cp2y为第二个控制点的纵坐标;x为贝济埃曲线的终点横坐标,y为贝济埃曲线的终点纵坐标.
- 二次样条曲线
  context.quadraticCurveTo(in float cpx,in float cpy,in float x,in float y);//控制点的横坐标,控制点的纵坐标,二次样条曲线终点的横坐标,二次样条曲线终点的横坐标

###5.3 绘制渐变图形

####5.3.1 绘制线性渐变
fillStyle方法在填充时指定填充的颜色,还可以用来指定填充的对象.
绘制线性渐变时,需要使用用到LinearGradient对象.使用图形上下文对象的createLinearGradient方法创建对象.
context.createLinearGradient(xStart,yStart,xEnd,yEnd) //xStart为渐变起始点的横坐标,yStart为渐变起始点的纵坐标,xEnd为渐变结束地点的横坐标,yEnd为渐变结束地点的纵坐标.
通过使用该方法,创建了一个使用两个坐标点的LinearGradient对象.使用addColorStop方法进行设定颜色.
context.addColorStop(offset,color); //offset为所设定的颜色离开渐变起始点的偏移量,0~1之间的浮点值,渐变起始点的偏移量为0,渐变结束点的偏移量为1.

####5.3.2 绘制径向渐变
使用Canvas API,除了可以沪指线性渐变之外,还可以绘制径向渐变.径向渐变时指沿着圆形的半径方向向外进行苦熬三的渐变方式. 譬如在沪指太阳时,沿着太阳的半径方向向外扩散出去的光晕,就是一种径向渐变.
使用图形上下文对象的createRadialGradient方法绘制径向渐变.
context.createRadialFraddient(xStart,yStart,radiusStart,xEnd,yEnd,radiusEnd);//xStart为渐变为开始圆的圆心横坐标,yStart为渐变开始圆的圆心纵坐标,radiusStart为开始圆的半径,xEnd为渐变结束圆的圆心横坐标,yEnd为渐变结束圆的圆心纵坐标,radiusEnd为结束圆的半径.
在这个方法中,分别指定了两个圆的大小与位置.从第一个圆的圆心处向外进行扩散渐变,一直扩散到第二个圆的外轮廓处.
在设定颜色时,与线性渐变相同,使用addColorStop方法进行设定即可.同样也需要设定0到1之间的浮点数来作为渐变转折点的偏移量.

###5.4 绘制变形图形

#### 5.4.1 坐标变化
- 平移  使用图形上下文对象的translate方法移动坐标轴原点.
  context.translate(x,y);//x表示将坐标轴原点向左移动多个单位,默认情况下为像素;y表示将坐标轴原点向下移动多个单位.
- 扩大  使用图形上下文对象的scale方法将图形放大.
context.scale(x,y); //x是水平方向的放大倍数,y是垂直方向的放大倍数.将图形缩小的时候,将这两个参数设定为0到1之间的小数就可以了,比如0.5指将图形缩小一般.
- 旋转 使用图形上下文对象的rotate方法将图形进行旋转.
  context.rotate(angle); //angle是指旋转的角度,旋转的中心点是坐标轴的原点.旋转是以顺时针方向进行的,(负数,逆时针旋转)

####5.4.2 坐标变换与路径的结合使用

####5.4.3 矩阵变换
利用Canvas API中利用坐标变换实现的图形技术.当利用坐标变换不能满足需要时,可以利用矩阵变化的技术.
context.transform(m11,m12,m21,m22,dx,dy);
该方法使用一个新的变换矩阵与当前变换矩阵进行乘法运算,

-------------------------------------------------
m11                m21                    dx
m12                m22                    dy
0                  0                      1


dx表示将坐标原点在x轴上向右移动x个单位,默认像素.dy表示将坐标原点在y轴上上向下移动单位. m11,m12,m21.m22通过矩阵乘法来变换.
上一节使用坐标变换进行图形变形的技术中所提到的三个方法,实际上都是隐式地修改了变换矩阵,都可以使用transform方法来代替.
- translate(x,y)
  可以使用context.transform(1,0,0,1,x,y)或context.transform(0,1,1,0,x,y)方法进行代替,前面的四个参数1,0,0,1或0,1,1,0表示不对图形进行缩放操作,变形操作,当dx设为x表示将坐标原点向右移动x个单位,dy设为y表示将坐标原点向下y个单位.
- scale(x,y)
可以使用context.transform(x,0,0,y,0,0)或context.transform(0,y,x,0,0,0)方法代替,前面四个参数x,0,0,y或0,y,x,0表示将图形横向扩大x倍,纵向扩大y倍.dx,dy为0表示不移动坐标原点.
- rotate(angle)
 替换方法
context.transform(Math.cos(angle*Math.PI/180)),Math.sin(angle*Math.PI/180),-Math.sin(angle.Math.PI/180),Math.cos(angle*Math.PI/180),0,0),
或
context.transform(-Math.sin(angle*Math.PI/180),Math.cos(angle*Math.PI/180),Math.cos(angle*Math.PI/180),Math.sin(angle*Math.PI/180),0,0)

使用了transform方法后,接下来要绘制的图形都会按照移动后的坐标原点与新的变换矩阵相结合的方法进行绘制,必须时可以使用setTransform方法将变换矩阵进行重置,setTransform方法进行定义:
context.setTransform(m11,m12.m21,m22,dx,dy);
setTransform方法的参数及参数的用法与transform相同,该方法的作用为将画布上的最左上角重置为坐标原点,当图形上下文创建完毕时将所创建的初始变换矩阵设置为当前变换矩阵,然后transform方法.

###5.5 图形组合
canvas API可以将一个图形重叠绘制在里一个图形上(图形中能够看到的部分完全取决去图形的绘制顺序),如果想要将两个图形进行组合,并且自己决定以那种方式进行组合,这时需要使用Canvas API的图形这技术.
在HTML 5中,只要用图形上下文对象的globaCompositeOperation属性就能决定图形组合方式.
context.globalCompositeOperation=type;
type:
- source-over(default)
  source-over为globalCompositeOperation属性的默认值,表示新的图形覆盖在原有图形之上.

- destination-over
  表示在原有图形之下绘制新图形

- source-in
  新图形与原有图形作in运算,只显示新图形中与原有图形相重叠的部分,新图形与原有图形的其他部分均变为透明

-destination-in
原有图形与新图形作in运算,只显示原有图形中与新图形重叠的部分,新图形与原有图形的其他部分均变为透明

-source-out
新的图形与原有图形作out运算,只显示新图形中与原有图形不重叠的部分,新图形与原有图形的其他部分均变为透明

-destination-out
原有图形与新图形作out运算,只显示原有图形中与新图形不重叠的部分,新图形与原有吐习惯的其他部分均变为透明

-source-atop
只绘制新图形中与原有图形重叠的部分与未被重叠的原有图形,新图形的其他部分变为透明

-destination-atop
只绘制原有图形中被新图形重叠覆盖的部分与新图形的其他部分,原有图形中的其他部分变为透明,不绘制新图形中与原有图形相重叠的部分.

-lighter
原有图形与新图形均绘制,重叠部分做加色处理.

-xor
只绘制新图形中与原有图形不重叠的部分,重叠部分变为透明

-copy
只绘制新图形,原有图形中未与新图形重叠的部分变为透明

###5.6 给图形绘制阴影
在HTML 5中,使用Canvas元素可以给图形添加阴影效果.添加阴影效果时,只需要利用图形上下文对象的几个关于阴影绘制的属性就可以了.
shadowOffsetX  阴影的横向位移量  默认0
shadowOffsetY  阴影的纵向位移量  默认0
shadowColor    阴影的颜色   
shadowBlur     阴影的模糊范围   optional  0~10

###5.7 使用图像

####5.7.1 绘制图像
在HTML 5中,不仅可以使用Canvas API来绘制图形,还可以读取磁盘或网络中的图像文件,然后使用Canvas API将该图像绘制在画布中.
绘制图像,drawImage方法
//image是一个Image对象,用该对象来装载图像文件,x,y为绘制时该图像在画布中的起始坐标

context.drawImage(image,x,y);  //图形大小等于原图大小
context.drawImage(image,x,y,w,h); //w,h指图像大小(缩放图像)
context.drawImage(image,sx,sy,sw,sh,dx,dy,dw,dhx);//用来将画布中已绘制好的图像的全部或局部区域复制到画布中的另一个位置上.
	image:被复制图像文件
	sx,sy分别表示源图像的被复制区域在画布中的起始作横坐标与起始纵坐标
	sw,sh表示被复制区域的宽度与高度
	dx,dy表示复制后的目标图像在画布中起始横坐标与起始纵坐标
	dw,dy表示复制后的目标图像的宽度与高度
绘制图像时首先使用不带参数的new方法创建Image对象,然后设定该Image对象的src属性为需要绘制的图像文件的路径
	image=new Image();
	image.src="1.jpg"; //当src指向网络图片时,当图像全部装载完毕后才能看到该图像,这种情况下,使用image.onload=function(){}


####5.7.2 图像平铺
` 使用canvas width,height计算,然后使用drawImage结合坐标计算实现
- 使用图形上下文对象的createPattern方法
- context.createPattern(image,type);
  type:
	no-repeat:不平铺
	repeat-x:横向平铺
	repeat-y:纵向平铺
	repeat:全方向平铺

####5.7.3 图像裁剪
canvas API的图像裁剪功能是指,在画布内使用路径,只回执该路径所包括的区域内的图像,不绘制路径外的图像.
- 使用图形上下文对象的不带参数的clip方法来实现canvas元素的图像裁剪功能.该方法使用路径来对Canvas画布设置一个裁剪区域,因此,必须先创建好路径.[裁剪区域一旦设置好之后,后面绘制的所有图形就都可以使用这个裁剪区域,如果要取消这个已经设置好的裁剪区域,需要使用绘制状态的保存与恢复功能,这两个功能保存与回复图形上下文的临时状态.]在设置图像裁剪区域时,首先调用save方法保存图形上下文的当前状态,在绘制完经过裁剪的图像后,再调用restore恢复之前保存的图形上下文状态.


####5.7.4 像素处理
使用Canvas API能够获取图像中的每一个像素,然后得到该像素颜色的rgb值或rgba值.使用图像上下文对象的getImageData方法来获取图像中的像素
var imagedata=context.getImageData(sx,sy,sw,sh);//sx,sy分别表示所获取区域起点横坐标,起点纵坐标,sw,sh分别表示所获取区域的宽度和高度.
imagedata变量是一个CanvasPiexlArray对象,具有height,width,data等属性. data属性是一个保存像素数据的数组.内容[r1,g1,b1,a1,r2,g2,b2,a2....]
r1,g1,b1,a1为第一个像素的红色值,绿色值,蓝色值,透明度值.data.length为所取得像素的数量
使用图形上下文对象的putImageData方法将反显操作后的图像重新绘制在画布上.[部分浏览器支持]
context.putImageData(imagedata,dx,dy[,dirtyX,dirtyY,dirtyWidth,dirtyHeight]);
imagedata为前面所述的像素数组,dx,dy分别表示重回图像的起点横坐标,纵坐标. dirtyX,dirtyY,dirtyWidth,dirtyHeight为可选参数,给出一个巨型的起点横坐标,纵坐标,宽度,高度

####5.8 绘制文字
绘制文字可选用fillText方法或strokeText方法.
void fillText(text,x,y,[maxWidth]); //text 要绘制的文字,x要绘制文字的起点横坐标,y表示要绘制文字的起点纵坐标,maxWidth可选参数,表示显示文字时做大宽度,可防止文字溢出
void strokeText(text,x,y,[maxWdith])  以轮廓方式绘制字符串

绘制文字的有关属性:
font  设置文字字体
textAlign:  设置文字水平对其方式,属性可以wiestart,end,left,right,center.默认start
textBaseline属性: 设置文字垂直对齐方式.属性值可以为top,hanging,middle,alphabetic,ideographic,bottom.默认alphabetic

可以使用图形上下文对象measureText方法来得到文字的宽度
metrics=context.measureText(text) //text要绘制的文字,该方法翻译一个TextMertrics对象,TextMetrics对象的width属性表示使用当前指定的字体后text参数中指定文字的总文字宽度


###5.9 补充知识

####5.9.1 保存和状态恢复
save, restore

图形上下文对象的当前状态的保存与状态恢复时是一个独立的话题,与Canvas API中其他的技术都不相关.可以在一下场合使用
- 图像或图形的变形
- 图像裁剪
- 改变图形上下文的以下属性:fillStyle,font,globalAlpha,globalCompositeOperation,lineCap,lineJoin,lineWidth,miterLimit,shadowBlur,shadowColor,shadowOffsetX,shadowOffsetY,strokeStyle,textAlign,textBaseline

####5.9.2 保存文件
在画布中绘制完一副图形或图像后,很多时候需要将该图形或图像保存到文件中,使用Canas API可以完成
Canvas API保存文件的原理实际上时把当前绘制的绘画状态输出到一个data URL地址所指向的数据中的过程.所谓data URL,是指大多数浏览器能够识别的一种base64位编码的URL,主要用于小型的,可以在网页中直接嵌入,而不需要外部文件嵌入的数据.譬如img元素中的图像文件等.data URL的格式类似"data:image/png;base4=64,iVbor
.....etc"
Canvas API使用toDataURL 方法把绘画输出到一个data URL中,然后重新装载,客户可以直接把装载后的文件进行保存.
canvas.toDataURL(type)  type表示输出数据的MIME类型


####5.9.3 简单动画的制作
制作动画实际上就是不断擦除,重绘,擦除,重绘的过程
1 预先编写好用来绘图的函数,在该函数中先用clearRect方法将画布整体或局部擦除
2 使用setInterval方法设置动画的间隔时间 (两个参数,第一方法,第二个时间间隔,毫秒)