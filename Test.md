**Recommend**
- [百度百科 单元测试](http://baike.baidu.com/link?url=QDszuE0yZMwByr6rA7XuDJtm3ufPKVnIghexe4zuELcR_XbgnnrqPYzW16oZaY_hQSh5hOxfe_IpzwuNUOQ2Yq)

#黑盒测试技巧
- 先从最基本最简单的行为开始测试，然后再组合各种行为
- 测试用例要细分
- console控制台记录报错代码
- 需要记录bug是否可重现

#测试用例
- 分类建立测试用例
- 根据不同的数据类型测试
- 根据数据长度测试边界值
- 也可以自行构想偏门的用例

#TDD
- TDD指的是Test Drive Development，很明显的意思是测试驱动开发，也就是说我们可以从测试的角度来检验整个项目。大概的流程是先针对每个功能点抽象出接口代码，然后编写单元测试代码，接下来实现接口，运行单元测试代码，循环此过程，直到整个单元测试都通过。
TDD的好处自然不用多说，它能让你减少程序逻辑方面的错误，尽可能的减少项目中的bug，开始接触编程的时候我们大都有过这样的体验，可能你觉得完成得很完美，自我感觉良好，但是实际测试或者应用的时候才发现里面可能存在一堆bug，或者存在设计问题，或者更严重的逻辑问题，而TDD正好可以帮助我们尽量减少类似事件的发生。而且现在大行其道的一些模式对TDD的支持都非常不错，比如MVC和MVP等。但是并不是所有的项目都适合TDD这种模式的，我觉得必须具备以下几个条件。  
首先，项目的需求必须足够清晰，而且程序员对整个需求有足够的了解，如果这个条件不满足，那么执行的过程中难免失控。当然，要达到这个目标也是需要做一定功课的，这要求我们前期的需求分析以及HLD和LLD都要做得足够的细致和完善。 其次，取决于项目的复杂度和依赖性，对于一个业务模型及其复杂、内部模块之间的相互依赖性非常强的项目，采用TDD反而会得不尝失，这会导致程序员在拆分接口和写测试代码的时候工作量非常大。另外，由于模块之间的依赖性太强，我们在写测试代码的时候可能不采取一些桥接模式来实现，这样势必加大了程序员的工作量。

#aauto自动化测试
- 创建项目：对应需要测试的webform/winform创建项目
- 获取元素节点的方法
```
wb.document.getElementById("id")
wb.document.getElementsByName("name")
wb.document.getElementsByTagName("tag")
```
- 提交表单，获取到元素节点后对该对象调用click方法模拟点击事件：ele.click()（推荐使用）  
表单内的元素调用form的submit方法：ele.form.submit()
- *Attention:*
>- aauto现已放弃更新，暂不支持较新的前端框架，导致某些标签无法取值或赋值

#QTP/UFT自动化测试
- 建议使用UFT而非QTP，QTP是UFT的前身，所以都为老版本
- 安装：傻瓜式安装
- 测试项目：新建可选GUI/API测试，自动化测试选择GUI；点击GUI测试的视图是一个流程图，里面包含规定的开始结束和自己设计的Action
- Action：Action内则是存放对象以及对象的行为的地方；  
“菜单>工具>对象侦测器>手形图标”可以选中后获取界面上的web和winForm元素，然后可以添加到对象存储库  
“资源>对象存储库>点击加号”也可以直接添加对象到此，同时可以查看元素分支和属性  
在界面下方的工具箱内点击刷新可以得到对象存储库，将添加的页面元素拖动到右侧文本编辑器上，即可生成元素行为语句
- 行为语句具体语法参考工具的使用指南
- 参数化（我们可以对要测的web中的某个功能测试进行迭代，每次变换不同的参数）：  
使用数据表，如"A"列中是需要测试的userName的用例数据，填充好数据后语句的Set方法变为Set DataTable("A",dtGlobalSheet/dtLocalSheet)；  
方法中第二个参数dtGlobalSheet为全局变量表，dtLocalSheet为当前Action表
- 迭代：迭代需要在“文件>设置>运行”中点击单选框设置“在所有行上运行”
- *Attention:*
>- 每一次迭代进行完之后需要回到原点，否则下次迭代会找不到对象后报错</br>目前测试可得只支持Internet Explorer

#单元测试覆盖率
- 语句覆盖：被测代码中每个可执行语句是否被执行到了
- 判断覆盖：程序中每一个判定的分支是否都被测试到了
- 条件覆盖：判定中的每个子表达式结果true和false是否被测试到了
- 路径覆盖：是否函数的每一个分支都被执行了
- 覆盖率数据只能代表你测试过哪些代码，不能代表你是否测试好这些代码
- 不要过于相信覆盖率数据
- *Reference:*
>- [单元测试代码覆盖率浅谈](http://blog.csdn.net/deyili/article/details/6688504/)

#mocha单元测试
- 安装mocha指令
```bash
$ npm install -g mocha
```
- `describe`归纳同类用例，`it`内测试用例
```js
describe('一类方法测试', function(){
  it('测试用例描述', function(done) {            
    console.log(routesUrl);
    //引用了supertest模块
    request.get(routesUrl[index])
    .send()            
    .end(function(err, res) {                
      should.not.exist(err);               
      res.body.data.length.should.equal(0);                
      done();            
    });       
  });
});
```
- *Attention:*
>- 异步测试在`it`内一定要显式`done()`方法
>- 遍历数组内的用例进行异步测试时，可以将`it`封装为Promise对象