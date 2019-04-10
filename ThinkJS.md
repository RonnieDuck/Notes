#WebSocket
###*开启与配置*
- WebSocket 功能默认是关闭的，项目如果需要开启，可以修改配置文件 src/common/config/websocket.js：
```js
export default {
  on: false, //是否开启 WebSocket
  type: "socket.io", //使用的 WebSocket 库类型，默认为 socket.io
  allow_origin: "", //允许的 origin
  adp: undefined, // socket 存储的 adapter，socket.io 下使用
  path: "", //url path for websocket
  messages: {
    // open: "home/websocket/open",
  }
};
```
- 需要将配置 on 的值修改为 true，并重启 Node.js 服务。
- ThinkJS 里对 WebSocket 的包装遵循了 socket.io 的机制，服务端和客户端之间通过事件来交互，这样服务端需要将事件名映射到对应的 Action，才能响应具体的事件。配置在 messages 字段，具体如下：
```js
export default {
  messages: {
    open: "home/socketio/open", // WebSocket 建立连接时处理的 Action
    close: "home/socketio/close", // WebSocket 关闭时处理的 Action
    adduser: "home/socketio/adduser", //adduser 事件处理的 Action
  }
};
```
- 其中 open 和 close 事件名固定，表示建立连接和断开连接的事件，其他事件均为自定义，项目里可以根据需要添加。

###*后台Action 处理*
- 通过上面配置事件到 Action 的映射后，就可以在对应的 Action 作相应的处理。如：
```js
export default class extends think.controller.base {
  /**
   * WebSocket 建立连接时处理
   * @param  {} self []
   * @return {}      []
   */
  openAction(self){
    var socket = self.http.socket;
    //this.broadcast对当前socket以外所有socket广播事件
    this.broadcast("new message", {
      content: 'this is broadcast'
    });
    //this.emit对当前socket广播事件
    this.emit("new message", {
      content: 'this is emit'
    })
  }
}
```

###*后台Action 处理*
- 安装依赖
```bash
$ npm install socket.io
```
- 前端引入和js
```html
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io('http://localhost');
  socket.on('news', function (data) { //监听服务器事件
    console.log(data);
    socket.emit('my other event', { my: 'data' }); //向服务器发送事件
  });
</script>
```

- *Reference*
>- [WebSocket 是什么原理？为什么可以实现持久连接？](https://www.zhihu.com/question/20215561)

#ThinkJs事务处理
###*startTrans()*
- 使用this.model('table').startTrans()开始事务
- 然后this.model('table').commit()提交
```js
export default class extends think.controller.base {
  async indexAction(){
    let model = this.model("user");
    try{
      await model.startTrans();
      let userId = await model.add({name: "xxx"});
      let insertId = await this.model("user_group").add({user_id: userId, group_id: 1000});
      await model.commit();
    }catch(e){
      await model.rollback();
    }
  }
}
```
- *Attention:*
>- 这种方法建议将代码放在trycatch代码块内，然后在catch代码块内使用this.model('table').rollback()

###*transaction({promise})*
- 使用this.model('table').transaction()方法
- 方法内接收一个promise，将数据库处理操作方法放在这里面
```js
export default class extends think.controller.base {
  async indexAction(self){
    let model = this.model("user");
    let insertId = await model.transaction( async() => {
      let userId = await model.add({name: "xxx"});
      return await self.model("user_group").add({user_id: userId, group_id: 1000});
    })
  }
}
```

###*多个模型操作事务*
- 先定义一个模型，开启事务
- 然后将该模型的数据库连接传递给需要进行数据库操作的另外的模型
```js
let model1 = this.model('table1');
let model2 = this.model('table2').db(table1.db());
```js
- *Attetion:*
>- 一个模型操作一个表，操作前需要传递数据库连接

- *Reference:*
>- [ThinkJS事务](https://thinkjs.org/zh-cn/doc/2.2/model_transaction.html#)