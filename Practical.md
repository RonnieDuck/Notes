#VM12安装OSX，虚拟OSX安装VM tools
- 下载好vmunlocker206工具、mac镜像
- 开启window服务，关闭所有VM的服务
- 找到unlocker文件夹里的win-install.cmd，右键以管理员身份运行，等待doc窗口关闭
- 打开VM，正常安装步骤，选好配置
- 将安装好的文件夹里面的OS X.vmx用笔记本打开，在 smc.present = "TRUE" 后 加上 smc.version = 0
- 开启mac
- 进入到mac的安装磁盘界面后，鼠标移到上方菜单栏“实用工具”中的“磁盘工具”，选择VMware的一个抹掉
- 再选择刚刚抹掉的磁盘安装
- *Reference:* 
>- [VM12安装OS X](http://jingyan.baidu.com/article/bea41d4388a8c4b4c51be6ab.html), [VMware虚拟安装Mac OS X后怎么安装VMware Tools?](http://www.beihaiting.com/a/RJC/GJRJ/20141119/5704.html)

#word从第n页设置页码(n>1)
- 打开你要设置的word文档，把鼠标光标移动到第n-1页的结尾，然后点击页面布局=>分隔符
- 在分隔符的下拉菜单里，我们选择分节符的第一个，下一页（插入分节符并在下一页上开始新节）
- 插入之后，你会发现，分节符（下一页）
- 如果你看不到分节符，你要开启编辑标记的显示，点亮开启，就可以看到了，一般情况下，我们都会开启这个显示，这样方便word的编辑与排版，看得到哪些地方一些什么标记
- 插入分节符后，我们把鼠标双击第三页的页脚部分，打开页眉和页脚的编辑模式，把“链接到前一条页眉”的标示取消
- 取消后，这个按钮就不亮了，如图，这样就前后分为独立的两节了
- 接下来就要设置页码了，把鼠标光标放在第三页的页脚处，点击设计——》页码——页面底端——》选择你需要的页码格式
- 当我们把页码设置好后，发现这个页码还要3，不要急，接下来，选择这个页码，选择设计——》页码——》设置页码格式
- 我们看到页码格式中的页码编号分为续前节和起始页码自定义，如图，那么我们就选择起始页码，然后，设置从1开始
- 同时，我们再回到上面我看第二页和第一页，下方要没有页码的，同时你可以根据你的需要在这里设置其它或者空白都没有关系，不会影响到后面了
- *Reference:*
>- [word如何从第三页设置页码](http://jingyan.baidu.com/article/a948d6515f1b870a2dcd2ec1.html) 

#ORM
- 什么是ORM?  
ORM，即Object-Relational Mapping（对象关系映射），它的作用是在关系型数据库和业务实体对象之间作一个映射，这样，我们在具体的操作业务对象的时候，就不需要再去和复杂的SQL语句打交道，只需简单的操作对象的属性和方法。
- 优点：隐藏了数据访问细节，“封闭”的通用数据库交互，ORM的核心。他使得我们的通用数据库交互变得简单易行，并且完全不用考虑该死的SQL语句。快速开发，由此而来。ORM使我们构造固化数据结构变得简单易行。在ORM年表的史前时代，我们需要将我们的对象模型转化为一条一条的SQL语句，通过直连或是DB helper在关系数据库构造我们的数据库体系。而现在，基本上所有的ORM框架都提供了通过对象模型构造关系数据库结构的功能。这，相当不错。
- 缺点：  1. 无可避免的，自动化意味着映射和关联管理，代价是牺牲性能（早期，这是所有不喜欢ORM的人的共同点）。选择的各种ORM框架都在尝试使用各种方法来减轻这块(LazyLoad, Cache)，效果还是很显著的。2. 面向对象的查询语言(X-QL)作为一种数据库与对象之间的过度，虽然隐藏了数据层面的业务抽象，但并不能完全的屏蔽掉数据层的设计，并且无疑将增加学习成本。3. 对于复杂查询，ORM仍然力不从心。虽然可以实现，但是不值得。视图可以解决大部分calculated column, case, group, having, order by, exists, 但是查询条件(a and b and not c and (d or d))...

#代理服务器
- *Reference:*
>- [百度百科-代理服务器](http://baike.baidu.com/link?url=UVbTfqlJg5XTUFhB4aLmNblh0t9uXyJkNamAegy6tj8jGpowoAJFPPXlaMeh8OEica7vxbWF69oK75amDve4YAy93GpVwC79r4T54P3CgLyX9JMFoZqtW0fhplVIA5nJXT96BScShNk2-nlvulxPP_)
>- [代理服务器与反向代理服务器的区别](http://www.tuicool.com/articles/eiYRbu)

#Fiddler抓包更改数据
- Fiddler命令行输入`bpu $url`设置断点
- 浏览器中访问该url
- Fiddler中断点抓取后，Inspectors=>WebForms更改请求发送参数，点击Break on Response
- 设置http状态码，TextView可以更改JSON数据，点击Run to Completion完成
- *Reference:*
>- [抓包工具：Fiddler 修改请求表单和响应数据](http://blog.csdn.net/liuquan0071/article/details/51917893)

#nodemailer发送邮件出错，错误代码：535
- 测试中出现过的错误：535、553、550（5开头的错误）
- 原因：smtp服务器验证不通过，因为网易的邮箱有个授权限制（这个就是坑所在，鄙视一万年）
解决方案：不适用SSL的话，用25端口，密码要用授权码当密码（登录网易邮箱绑定手机就可以获取得到）。  
PS：那个网易邮箱（A）的授权码需要绑定手机（AA）才可以获取，然而我发现了bug，获取授权码后用别的邮箱（B）绑定这个手机（AA），邮箱（A）就可以解绑刚才那个手机（AA），但是邮箱（A）授权码依然是有效的
- *Reference:*
>- [nodemailer发送邮件出错](https://cnodejs.org/topic/55629a8a8f294e213d10b8e7)

#Windows Server搭建微擎
- 安装phpstudy（傻瓜式安装）
- 开启openssl：  
“其他选项菜单” => “打开配置文件” => “php-ini”  
找到;extension=php_openssl.dll
去掉前面的分号";"，保存
- 配置本地访问程序  
“其他选项菜单” => “phpstudy设置” => “端口常规设置”  
- 配置域名访问程序（需要已有可用域名绑定到当前ip）：  
“其他选项菜单” => “站点域名管理”  
- 配置好域名后添加端口新规则（windows server端口访问默认只允许IIS）  
“控制面板” => “高级防火墙” => right-click“入站规则” => “添加新规则”
- *Reference:*
>- [phpstudy如何绑定域名](https://zhidao.baidu.com/question/521337996945865125.html)
>- [安装了phpstudy后，本机可以访问，内网或外网都不可以访问web的解决方案](http://blog.sina.com.cn/s/blog_15a1ea49b0102x9be.html)