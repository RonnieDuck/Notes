###**Recommend**
- [Node](http://wiki.jikexueyuan.com/project/nodejs/)
- [NPM](http://www.runoob.com/nodejs/nodejs-npm.html)
- [node搭建服务器]( http://www.nodebeginner.org/index-zh-cn.html#javascript-and-nodejs)

#Module
- node.js通过实现CommonJS的Modules/1.0标准引入了模块(module)概念,模块是Node.js的基本组成部分.一个node.js文件就是一个模块,也就是说文件和模块是一一对应的关系.这个文件可以是JavaScript代码,JSON或者编译过的C/C++扩展.
- Node.js的模块分为两类，一类为原生（核心）模块，一类为文件模块。  
在文件模块中，又分为3类模块。这三类文件模块以后缀来区分，Node.js会根据后缀名来决定加载方法。
- .js。通过fs模块同步读取js文件并编译执行。  
.node。通过C/C++进行编写的Addon。通过dlopen方法进行加载。  
.json。读取文件，调用JSON.parse解析加载。
- Node.提供了exports和require两个对象,其中exports是模块公开的接口,require用于从外部获取一个模块接口,即所获取模块的exports对象.
- 原生模块在Node.js源代码编译的时候编译进了二进制执行文件，加载的速度最快。另一类文件模块是动态加载的，加载速度比原生模块慢。  
但是Node.js对原生模块和文件模块都进行了缓存，于是在第二次require时，是不会有重复开销的。尽管require方法极其简单，但是内部的加载却是十分复杂的，其加载优先级也各自不同。
- require方法接受以下几种参数的传递：
http、fs、path等，原生模块。
- ./mod或../mod，相对路径的文件模块。
/pathtomodule/mod，绝对路径的文件模块。
mod，非原生模块的文件模块。
- 当require一个文件模块时,从当前文件目录开始查找node_modules目录；然后依次进入父目录，查找父目录下的node_modules目录；依次迭代，直到根目录下的node_modules目录。
- *Reference:*
>- [Node.js模块 require和 exports](https://liuzhichao.com/p/1669.html)

#node.js的基本机制
- node.js是异步单线程非阻塞机制
- 同步和异步关注的是消息通信机制 (synchronous communication/ asynchronous communication)
所谓同步，就是在发出一个*调用*时，在没有得到结果之前，该*调用*就不返回。但是一旦调用返回，就得到返回值了。
换句话说，就是由*调用者*主动等待这个*调用*的结果。
而异步则是相反，*调用*在发出之后，这个调用就直接返回了，所以没有返回结果。换句话说，当一个异步过程调用发出后，调用者不会立刻得到结果。而是在*调用*发出后，*被调用者*通过状态、通知来通知调用者，或通过回调函数处理这个调用。
######阻塞与非阻塞
阻塞和非阻塞关注的是程序在等待调用结果（消息，返回值）时的状态
阻塞调用是指调用结果返回之前，当前线程会被挂起。调用线程只有在得到结果之后才会返回。
非阻塞调用指在不能立刻得到结果之前，该调用不会阻塞当前线程。

#npm images
- 安装
```bash
$ npm install images
```
- API

		import images from 'images';
		//从指定文件加载并解码图像
		images(file)
		//创建一个指定宽高的透明图像
		images(width, height)
		//从Buffer数据中解码图像
		images(buffer[, start[, end]])
		//从另一个图像中复制区域来创建图像
		images(image[, x, y, width, height])
		//在当前图像( x , y )上绘制 image 图像
		.draw(image, x, y)
		//编码并保存当前图像到 file ,如果type未指定,则根据 file 自动判断文件类型，config为图片设置，目前支持设置JPG图像质量
		eg:images("input.png").save("output.jpg", {operation:50})
		.save(file[, type[, config]])


#npm qr-image
- 安装

		$ npm install qr-images

- API

		import qrImage from 'qr-image';
		qrImage.image(url/string, { type: 'png' }) //将目标转二维码
		  .pipe(fs.createWriteStream(path + filename)) //保存到本地
		  .on('close', function(err) { //异步stream写入完成事件后
		    //some codes
		  });


#Node-MySQL
- 安装依赖 `$ npm install mysql`

- 如何使用

		var mysql = require('mysql');
		var connection = mysql.createConnection({
		  host: 'localhost',
		  port: 'port', //默认:3306
		  user: 'me',
		  password: 'secret',
		  database: 'my_db'
		});
		//开启配置好的连接
		connection.connect();
		//执行sql语句操作
		connection.query('SELECT 1 + 1 AS solution', function(err, rows, fields) {
		  if (err) throw err;
		  console.log('The solution is: ', rows[0].solution);
		});
		//关闭连接
		connection.end();

- sql语句防注入方式

		//先定义sql语句，将参数传入query方法中
		var sql = 'UPDATE users SET foo = ?, bar = ?, baz = ? WHERE id = ?';
		let param = ['a', 'b', 'c', userId];
		connection.query(sql, param, function(err, rows, fields) {});

- 使用连接池

		var pool  = mysql.createPool({
		  connectionLimit : 10,
		  host            : 'example.org',
		  user            : 'bob',
		  password        : 'secret',
		  database        : 'my_db'
		});
		pool.getConnection(function(err, connection) {
		  // 使用连接
		  connection.query( 'SELECT something FROM sometable', function(err, rows) {
		    // 使用连接执行查询
		    connection.release();
		    //连接不再使用，返回到连接池
		  });
		});

- *Attention*
>- `connection.end()`方法会确保在数据库执行quit前完整执行query，而且包含回调函数；`connection.destroy()`则不包含回调函数

- *Reference*
>- [Node-MySQL 官方文档](http://www.oschina.net/translate/node-mysql-tutorial?utm_source=tuicool&utm_medium=referral)

#Express配合Socket.io
- 不能连接socket的原因：使用express脚手架创建的项目含有启动文件/bin/www；需要改造使得在createServer后才监听socket
- 在express脚手架创建项目的前提下，修改/bin/www文件；`var http = require('http');`后面加入`var socketio = require('../route/socket.js');`，`var server = http.createServer(app);`后面加入`socketio.bindSocket(server);`

- 自定义socket.js文件的内容以及路径，主要是export出一个方法在www中接收`http.createServer(app)`这个参数

- *Reference*
>- [express4.X框架中使用socket.io](http://blog.csdn.net/zzwwjjdj1/article/details/52149438)