#7 本地存储

## 7.1 Web Storage

### 7.1.1 Web Storage (客户端本地存储数据)

 	HTML 4 中用cookies存储永久数据存在以下问题:
	大小:被限制在4KB
	带宽:cookies是随HTTP事务一起被发送的,因此会浪费一部分发送cookies时使用的带宽
	复杂性:要正确操作cookies是很困难的.

Web Stroge分为:
1.	sessionStorage:[临时保存]保存在session对象中.(session对象可以从进入网站到浏览器所经过的事件内所要求保存的任何数据)
	保存数据:sessionStroage.setItem(key,value);
	读取数据: arg=sessionStroage.getItem(key);
2.	localStorage:[永久保存]保存在客户端本地硬件设备(硬盘,或其他硬件设备),当浏览器关闭了,数据仍然存在,下次访问浏览器时仍可使用.
	保存数据:localStroage.setItem(key,value);
	读取数据:arg=localStroage.getItem(key);

### 7.1.2 简易留言板
localStorage.length 所有保存在localStorage中的数据条数
localStorage.key(index) 将想要得到的数据的索引作为index参数传入,可以得到localStorage中与这个索引对应的数据.
localStorage.clear()  将localStorage中保存的数据全部清楚.

### 简易数据库
var str=JSON.stringify(data);//data 表示要转换成JSON格式的文本数据对象,
将对象转换为JSON格式的文本,并返回

var data=JSON.parse(str);//str json格式的字符串
将JSON格式的字符串转换为对象,并返回

使用时注意浏览器兼容性


##7.2 本地数据库

###7.2.1 本地数据库的基本概念
HTML 5中内置了一个可以通过sql语言来访问的数据库,在HTML 4中,数据库只能放在服务器
像这种不需要在存储在服务器上的,被成为"SQLLite"的文件型SQL数据库已经得到广泛应用,所有HTML 5中也采用了这种数据库来作为本地数据库

要使用SQLLite数据库,两个步骤:
1.	创建方位数据库对象
	使用openDataBase方法来创建一个访问数据库对象
	var db=openDatabase('mydb','1.0','Test DB'M2*1024*1024)// 创建并返回,如果数据不存在,则创建
	第一个参数:数据库名
	第二个参数:版本号
	第三个参数:数据库描述
	第四个参数:数据库大小

	
2.	使用事务
	实际访问数据库的时候,还需要调用transaction方法来执行事务(使用事务用来达到在操作完之前,阻止别的用户访问数据库)
	db.transaction(function(tx){
		tx.executeSql('CREATE TABLE IF NOT EXISTS LOGS (id unique,Log)');
	});

### 7.2.2 用executeSql来执行查询
例:使用作为参数传递给回调函数的transaction对象的executeSql方法
transaction.executeSql(sqlquery,[],dataHandler,errorHandler)
	第一个参数:需要执行的sql语句
	第二个参数:为sql语句中所有使用到的参数的数据,在executeSql中,将sql语句中所要用到的参数先用? 代替,然后依次将参数组成数据放在第二份参数中.如:transaction.executeSql("update peple set age=? where name=?",[age,name])
	第三个参数:执行成功时调用的回调函数  function dataHandler(transaction,results){}//transaction对象,返回的查询结果集对象
	第四个参数:执行失败时调用的回调函数	 function errorHandler(transaction,errmsg){}//transaction对象,执行错误的信息文字	

### 7.2.3 使用数据库实现web留言板
1  	创建数据表
	tx.executeSql('create table if not exists MsgData(name text,message text,time integer)',[])//create table
	tx.executeSql('select * from MsgData',[],<function(tx,result){}>,<function(tx,error){}>);
1	追加数据
	addData函数中的transaction方法中:
	`tx.executeSql('insert into msgdata values(?,?,?)',[name,msg,time],function(tx,result){
		//after execute success
	},function(tx,error){
		//after execute error
		})