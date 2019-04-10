**Recommend**
- [linux命令大全](http://wenku.baidu.com/link?url=cCTALd5odx8Z_gx4lqwxAFo8eVNhJ0V8x_JL-72bO4D9FmlqR0L_t4TziITnwuAZQByHuKDNXt-gBc5vV6f__u1sfyrBwVT5WyYyh2kl7di)

#Ubuntu安装git
- 在ubuntu中输入git指令，就可以检验出该系统是否已安装git
- 安装git的指令

		$ apt-get install git

#CentOS安装git
- yum安装  

		yum install git


#Ubuntu操作mysql
- 链接远端mysql  

		mysql -h主机名 -u用户名 -p密码

- mysql对于命令行的操作  

		?         (\?) Synonym for `help'.
		clear     (\c) Clear the current input statement.
		connect   (\r) Reconnect to the server. Optional arguments are db and host.
		delimiter (\d) Set statement delimiter.
		edit      (\e) Edit command with $EDITOR.
		ego       (\G) Send command to mysql server, display result vertically.
		exit      (\q) Exit mysql. Same as quit.
		go        (\g) Send command to mysql server.
		help      (\h) Display this help.
		nopager   (\n) Disable pager, print to stdout.
		notee     (\t) Don't write into outfile.
		pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
		print     (\p) Print current command.
		prompt    (\R) Change your mysql prompt.
		quit      (\q) Quit mysql.
		rehash    (\#) Rebuild completion hash.
		source    (\.) Execute an SQL script file. Takes a file name as an argument.
		status    (\s) Get status information from the server.
		system    (\!) Execute a system shell command.
		tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
		use       (\u) Use another database. Takes database name as argument.
		charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
		warnings  (\W) Show warnings after every statement.
		nowarning (\w) Don't show warnings after every statement.
		resetconnection(\x) Clean session context.

- 操作数据库与sql语句相同
- 修改mysql root用户的密码

		# 查看数据库用户密码
		$ vim etc/mysql/debian.cnf
		// 用该用户名密码登陆数据库，use mysql
		$ update user set authentication_string = Pasword('pwd') where User = 'root';
		# 使用root用户登录

- 启动/停止/重启mysql服务

		$ sudo mysql start
		$ sudo mysql stop
		$ sudo mysql restart

###*Attention:*
>- 在使用mysql命令行时都以分号';'结尾  
###*Reference:*
>- [ubuntu下mysql的常用命令](http://blog.knowsky.com/186050.htm)

#CentOS安装node-canvas
- 安装依赖：[Installation Amazon Linux AMI (EC2)](https://github.com/Automattic/node-canvas/wiki/Installation---Amazon-Linux-AMI-%28EC2%29)
- 依赖的libjpeg版本过低解决方法：[Wrong JPEG library version: library is 62, caller expects 80](https://www.webhostingneeds.com/Wrong_JPEG_library_version:_library_is_62,_caller_expects_80)
- 然后npm管理包安装`npm install canvas`

###*Attention:*
>- 安装cairo时，千万不要相信网上任何帖子！！！只看官方文档

#CentOS安装与配置Reids
###*安装*
- 下载并解压缩、安装

		$ wget http://download.redis.io/redis-stable.tar.gz
		$ tar -zxvf xxx.tar.gz
		$ make && make install

###*配置*
- 分别创建配置用文件夹

		$ cd /etc/ && mkdir redis
		$ cd /var/ && mkdir redis
		$ cd redis/ && mkdir data log run

- 将解压目录下的redis.conf文件复制到/etc/redis/中

		$ cp redis.conf /etc/redis/

- 修改redis.conf

		$ vim /etc/redis/redis.conf
		# 修改下面的配置选项
		port 6379
		pid /var/redis/run/redis.pid
		dir /var/redis/data
		logfile /var/redis/log/redis.log
		daemonize yes  //后台运行

- 持久化配置项

		appendonly yes  # 先设置yes开启持久化再选择持久化类型
		appendfsync always  # 每次收到写命令就立即强制写入磁盘，最慢的，但是保证完全的持久化，不推荐使用  
		appendfsync everysec  # 每秒钟强制写入磁盘一次，在性能和持久化方面做了很好的折中，推荐  
		appendfsync no  # 完全依赖os，性能最好,持久化没保证  


###*服务*
- 拷贝解压包下utils下redis启动脚本至/etc/init.d/，修改脚本名称(也可不修改)为redis

		$ cp redis_init_script /etc/init.d/
		$ mv redis_init_script redis

- 修改脚本pid及conf路径为实际路径

		REDISPORT=6379
		EXEC=/usr/local/bin/redis-server
		CLIEXEC=/usr/local/bin/redis-cli
		PIDFILE=/var/redis/run/redis.pid
		CONF="/etc/redis/redis.conf"
		# 生产环境下，配置时，配置文件、pid等最好加上端口标识，以便区分，如
		PIDFILE=/var/redis/run/redis_{REDISPORT}.pid
		CONF="/etc/redis/redis_{REDISPORT}.conf"

- 然后可以通过service redis start/stop 命令启动和关闭redis，如果其他目录不能执行，则给/etc/init.d/redis文件加上执行权限

		chmod +x /etc/init.d/redis


###*配置开机自动启动*
- 检查脚本中的redis启动优先级信息

		$ cd /etc/init.d/ && vim redis

- 优先级信息如下，没有则手动加入

		# chkconfig: 2345 90 10
		# description: Redis is a Persistent key-value database

- 开启自动启动

		$ chkconfig redis on


###*配置外网可访问*
- 修改/tec/redis/redis.conf配置文件，将所有bind信息全部屏蔽以及设置protected-mode为no。

		$ vim /etc/redis/redis.conf
		# bind 127.0.0.1
		protected-mode no

- 修改 Linux 的防火墙(iptables)，开启你的redis服务端口，默认是6379。

		$ /sbin/iptables -I INPUT -p tcp --dport 6379 -j ACCEPT
		$ /etc/rc.d/init.d/iptables save


###*Reference:*
>- [CentOS6.5下Redis安装与配置](http://blog.csdn.net/ludonqin/article/details/47211109)
>- [redis启用持久化](http://huangyunbin.iteye.com/blog/1894583)
>- [配置redis外网可访问](http://blog.csdn.net/qq_14926159/article/details/51379298)

#CentOS安装nodejs
- 安装并更新gcc依赖到4.8.4以上

		# yum安装
		$ yum -y install gcc gcc-c++ glibc-static libstdc++-static zip

- 升级gcc(该步骤安装4.8.2版本，解决g++版本过低问题)

		# 获取安装包并解压
		$ wget http://mirrors.ustc.edu.cn/gnu/gcc/gcc-5.1.0/gcc-5.1.0.tar.bz2
		$ tar jxf gcc-5.1.0.tar.bz2 -C /usr/local/src
		# 下载供编译需求的依赖项
		$ cd /usr/local/src/gcc-5.1.0
		$ ./contrib/download_prerequisites
		# 生成Makefile文件
		$ ./configure --prefix=/usr/local/gcc-5.1.0 --disable-multilib --enable-languages=c,c++,java
		# 编译（注意：此步骤非常耗时）
		$ make -j4
		//安装
		$ make && make install

- 安装nodejs

		# 获取下载安装包并解压
		$ wget https://nodejs.org/download/release/v6.9.5/node-v6.9.5.tar.gz
		$ tar -zxvf node-v6.9.5.tar.gz
		# 配置、安装
		$ cd node-v6.9.5
		$ ./configure
		$ make && make install

###*Reference*
>- [CentOS 6.* 64位系统升级gcc4.4.7升级gcc5.1详解](http://wangying.sinaapp.com/archives/2386)

#CentOS用nvm安装nodejs（更加建议的方法）
- 下载并安装NVM脚本

		$ curl https://raw.githubusercontent.com/creationix/nvm/v0.31.7/install.sh | bash
		$ source ~/.bash_profile

- 列出所需要的版本

		$ nvm list-remote

- 安装相应的版本

		$ nvm install v0.10.30

- 查看已安装的版本

		$ nvm list

- 切换版本

		$ nvm use v0.10.30

- 设置默认版本

		$ nvm alias default v0.10.30


###*Reference*
>- [在CentOS6.5上安装Node.js](http://wangying.sinaapp.com/archives/2456)

#安装与配置CentOS 6.8
###*虚拟机安装CentOS*
- 普通安装步骤
###*Attention:*
>- 如果iso镜像为minimal，则先安装一个空的虚拟机。在配置机器硬件时再选择本地的iso镜像

###*安装一系列依赖*
- vim

		$ yum -y install vim-enhanced
		# 如果使用crontab出错：
		[root@176177 ~]$ crontab -e
		no crontab for root - using an empty one
		/bin/sh: /bin/vi: No such file or directory
		crontab: "/bin/vi" exited with status 127
		# 则运行：
		$ yum -y install vim*

- wget

		$ yum -y install wget


###*Reference:*
>- [CentOS 6.8 服务器系统安装配置图解教程](http://www.jb51.net/os/RedHat/480406_2.html)
>- [Centos安装vim](http://www.cnblogs.com/jenry/archive/2013/06/13/3134215.html)
>- [centos安装wget](http://www.cnblogs.com/chusiping/archive/2011/11/10/2243805.html)
>- [CentOS 6.8 服务器系统安装配置图解教程](http://www.jb51.net/os/RedHat/480406_2.html)

#配置80端口指向其他端口（没有root权限使用80端口的情况下）
- 添加规则

		# 将80指向8080
		$ sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080

- 查看现有规则

		$ iptables -t nat -L -n --line-numbers

- 删除规则

		# 删除第一条规则
		$ iptables -t nat -D PREROUTING 1


###*Reference:*
>- [配置iptables,把80端口转到8080的简单方法](http://www.jb51.net/article/98995.htm)
>- [删除指定的iptables规则](http://www.jb51.net/os/RedHat/480406_2.html)

#CentOS安装字体
- 将要的字体复制到/usr/share/fonts/chinese/TrueType目录下，没有该目录的话，则手动创建
- 修改字体权限，使root以外的用户可以使用这些字体（给666权限）

		$ chmod -R 666 msyhbd.ttf

- 建立字体缓存

		$ cd /usr/share/fonts/chinses/TrueType
		$ mkfontscale 
		$ mkfontdir 
		$ fc-cache  -fv 

- 查看安装的字体

		$ fc-list :lang=zh


###*Reference:*
>- [CentOS安装微软雅黑字体](http://blog.csdn.net/brad_chen/article/details/50110189)

#Azure捕获虚拟机镜像
- SSH连接到虚拟机，执行命令进行虚拟机一般化

		# +user参数决定是否删除/home/azureuser/目录下的磁盘内容
		$ sudo waagent -deprovision
		$ sudo waagent -deprovision+user

- 在Azure CLI当中捕获该镜像

###*Reference:*
>- [如何捕获经典 Linux 虚拟机以用作映像](https://www.azure.cn/documentation/articles/virtual-machines-linux-classic-capture-image)

#CentOS安装及配置MySQL5.6
- 需要检测系统是否自带安装mysql

		$ yum list installed | grep mysql
		# 如果有自带则删除
		$ yum -y remove mysql-libs.x86_64

- 下载并安装（选择5.6版本原因是国外服务器网速慢，5.6文件大小比5.7小很多）

		$ wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
		# 这个rpm还不是mysql的安装文件，只是两个yum源文件，执行后，在/etc/yum.repos.d/ 这个目录下多出mysql-community-source.repo和mysql-community.repo
		$ rpm -ivh mysql-community-release-el6-5.noarch.rpm
		# 这个时候，可以用yum repolist mysql这个命令查看一下是否已经有mysql可安装文件
		$ yum repolist all | grep mysql
		# 用yum安装
		$ yum install mysql-community-server

- 启动mysql

		$ service mysqld start

- 配置

		# 由于mysql刚刚安装完的时候，mysql的root用户的密码默认是空的，所以我们需要及时用mysql的root用户登录（第一次回车键，不用输入密码），并修改密码
		$ mysql -u root
		$ use mysql;
		$ update user set password=PASSWORD("这里输入root用户密码") where User='root';
		$ flush privileges;
		# 查看mysql是否自启动,并且设置开启自启动命令
		$ chkconfig --list | grep mysqld
		$ chkconfig mysqld on
		# mysql安全设置(系统会一路问你几个问题，看不懂复制之后翻译，基本上一路yes)：
		$ mysql_secure_installation
		# 允许mysql远程登录
		$ GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY "123456"; 
		$ flush privileges;


###*Attention:*
>- 不要直接使用yum安装，yum的mysql资源版本最高只有5.1。目前常用版本为5.6

###*Reference:*
>- [linux CentOS6.5 yum安装mysql 5.6](https://segmentfault.com/a/1190000007667534)

#关于守护进程的理解与操作
###*nodejs的进程*
- 使用pm2启动

		# 需要启动的node
		$ node app.js
		# 用pm2启动的方式
		$ pm2 start app.js

- pm2相关命令

		# 查看运行中的node进程的日志
		$ pm2 logs
		# 查看运行中的node进程概览
		$ pm2 status
		# 暂停对应id或全部正在运行中的node进程
		$ pm2 stop {ID}/all
		# 重启对应id或全部正在运行中的node进程
		$ pm2 restart {ID}/all
		# 终止对应id或全部正在运行中的node进程
		$ pm2 delete {ID}/all


###*linux命令启动的进程*
- 使用nohup {command} &

		# 启动一个应用程序
		$ ./app
		# 后台运行
		$ nohup ./app &

- 关闭后台运行的进程

		# 查出全部进程/配合grep查出指定进程的pid
		$ ps -ef
		$ ps -ef | grep {command}
		# 终止进程
		$ kill {pid}
		# 强行终止进程
		$ kill -9 pid

- 关闭后台无用进程（回收内存，增加性能）

		$ kill -9 XXX


###*Attention:*
>- 通过xshell等远端连接时，所有在远端启动的进程都为ssh进程的子进程。所以无论加上什么命令，当远端这个父进程退出后，它下面的所有子进程都会被关闭。故建议远端时使用screen

###*screen命令*
- 如果系统中没有screen命令，则安装

		$ yum install screen –y

- 什么是screen  
Screen将创建一个执行shell的全屏窗口。你可以执行任意shell程序，就像在ssh窗口中那样。在该窗口中键入exit退出该窗口，如果这是该screen会话的唯一窗口，该screen会话退出，否则screen自动切换到前一个窗口。  
它不是当前远端的子进程，而是自己一个独立的进程。所以在screen当中运行的进程在断开远端时并不会随之终止
- 启动和相关命令

		# screen命令后跟你要执行的程序
		$ screen ./app
		# 查看以前在screen中执行的会话id
		$ screen -ls
		# 恢复会话
		$ screen -r {id}
		# 想要关闭进程，直接根据id恢复后ctrl+C


#Docker

###*安装和启动*
- CentOS下安装

		# centos 6.*
		$ yum install http://mirrors.yun-idc.com/epel/6/i386/epel-release-6-8.noarch.rpm
		$ yum install docker-io
		# centos 7.*
		$ yum install docker

- Ubuntu下安装

		$ apt-get update
		$ apt-get install -y docker.io
		$ ln -sf /usr/bin/docker.io /usr/local/bin/docker
		$ sed -i '$acomplete -F _docker docker' /etc/bash_completion.d/docker.io

- 启动

		service docker start


###*docker的bash命令*
- 镜像的操作

		# 获取镜像
		$ docker pull {docker_url}
		# 查看本地镜像
		$ docker images
		# 修改已有镜像
		$ 
		# 移除容器
		$ docker rm {docker}
		# 移除镜像
		$ docker rmi {docker}
		# 利用 Dockerfile 来创建镜像(-t是tag信息，“.” 是 Dockerfile 所在的路径)
		$ docker build -t="tag" .


###*dockerfile命令语法*
- FROM

		# FROM命令可能是最重要的Dockerfile命令。改命令定义了使用哪个基础镜像启动构建流程。基础镜像可以为任意镜 像。如果基础镜像没有被发现，Docker将试图从Docker image index来查找该镜像。FROM命令必须是Dockerfile的首个命令。
		$ FROM ubuntu

- CMD

		# 和RUN命令相似，CMD可以用于执行特定的命令。和RUN不同的是，这些命令不是在镜像构建的过程中执行的，而是在用镜像构建容器后被调用。
		$ CMD ["node","app.js"]

- ENV

		# ENV命令用于设置环境变量。这些变量以”key=value”的形式存在，并可以在容器内被脚本或者程序调用。这个机制给在容器中运行应用带来了极大的便利。
		$ ENV SERVER_WORKS 4

- EXPOSE

		# EXPOSE用来指定端口，使容器内的应用可以通过端口和外界交互。
		$ EXPOSE 8080

- COPY

		# 复制本地主机的 {src}（Dockerfile 所在目录的相对路径）到容器中的 {path}。
		$ COPY app.js /usr/local/

- RUN

		# 每条 RUN 指令将在当前镜像基础上执行指定命令，并提交为新的镜像。当命令较长时可以使用 \ 来换行
		$ RUN {command}
		$ RUN ["executable", "param1", "param2"]
		# 前者将在 shell 终端中运行命令，即 /bin/sh -c；后者则使用 exec 执行。指定使用其它终端可以通过第二种方式实现，例如 RUN ["/bin/bash", "-c", "echo hello"]


###*Reference:*
>- [文档](http://wiki.jikexueyuan.com/project/docker-technology-and-combat/)

#Linux安装nginx
###*Azure的Ubuntu下安装*
- 安装依赖

		$ sudo apt install gcc
		$ sudo apt-get install libpcre3 libpcre3-dev
		$ sudo apt-get install openssl libssl-dev
		$ sudo apt-get install make

- 下载并解压

		$ wget http://nginx.org/download/nginx-1.10.3.tar.gz
		$ tar zxvf nginx-1.10.3.tar.gz

- 可能遇到的问题

		# checking for C compiler ... found but is not working
		# ./configure error : C compiler gcc is not found
		# configure首先会编译一个小测试程序，通过测试其运行结果来判断编译器是否能正常工作，由于交叉编译器所编译出的程序是无法在编译主机上运行的，故而产生此错误。
		$ vim auto/cc/name
		# 将21行的“exit 1”注释掉

		# autotest:Syntax error: Unterminated quoted string bytes 
		# ./configure : error:can not detect int size
		# cat : /home/ubuntu/packets/nginx-1.6.2/build/autotest.c: No such file or directory
		$ vim auto/types/sizeof
		# ngx_test="$CC $CC_TEST_FLAGS $CC_AUX_FLAGS => ngx_test="gcc $CC_TEST_FLAGS $CC_AUX_FLAGS 

- 配置安装

		$ ./configure --with-http_stub_status_module --with-http_ssl_module --with-http_realip_module --with-http_gzip_static_module
		$ sudo make && sudo make install

- 启动

		# 先配置好nginx.conf文件
		$ sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf


###*CentOS下安装*
- 安装依赖

		$ yum install gcc-c++  
		$ yum install pcre pcre-devel  
		$ yum install zlib zlib-devel  
		$ yum install openssl openssl--devel  

- 下载解压安装

		$ wget http://nginx.org/download/nginx-1.7.4.tar.gz
		$ tar -zxvf nginx-1.7.4.tar.gz
		$ cd nginx-1.7.4
		$ ./configure   
		$ make  
		$ make install 

- 启动

		# 先配置好nginx.conf文件
		$ sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf


###*Reference:*
>- [Ubuntu 16.04 LTS 下Nginx的编译安装与启动](https://my.oschina.net/u/923772/blog/704637)
>- [nginx 交叉编译 ( checking for C compiler found but is not working )](http://blog.csdn.net/fish43237/article/details/40515897)
>- [CentOS7.0安装Nginx 1.7.4](http://www.centoscn.com/image-text/install/2014/0812/3480.html)

#CentOS gcc升级4.4.7到6.1.0
- 先用yum安装好旧版4.4.7的gcc，为了更新时的编译

		$ yum -y install gcc gcc-c++ glibc-static libstdc++-static zip

- 官网下载压缩包，并解压；[release list](http://ftp.gnu.org/gnu/gcc)

		$ wget http://ftp.gnu.org/gnu/gcc/gcc-6.1.0/gcc-6.1.0.tar.bz2
		$ tar -jxvf gcc-6.1.0.tar.bz2

- 执行脚本安装依赖，解压后的文件夹中已有脚本

		$ cd gcc-6.1.0
		$ ./contrib/download_prerequisites

-  建立一个目录供编译出的文件存放(该步骤非常重要，不然更新后依然为4.4.7)

		$ mkdir gcc-build-6.1.0
		$ cd gcc-build-6.1.0

-  配置编译安装

		$ ../configure -enable-checking=release -enable-languages=c,c++ -disable-multilib
		$ make
		$ make install


###*Reference:*
>-  [CentOS 6.6 升级GCC G++ (当前最新版本为v6.1.0) (完整)](http://www.cnblogs.com/lzpong/p/5755678.html)

#CentOS开机自启动脚本
- 添加脚本文件到指定目录，添加shell命令

		$ cd /etc/rc.d/init.d/
		$ touch run
		$ echo 'command' > run

- 添加执行权限

		$ chmod +x run

- 把脚本文件加入到chkconfig

		$ chkconfig --add run
		$ chkconfig run on


###*Reference:*
>- [Linux下chkconfig命令详解](http://www.cnblogs.com/panjun-Donet/archive/2010/08/10/1796873.html)

#OS X安装nvm
- 下载安装包(完成后自动安装)

		$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
		# or
		$ wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash

- 添加环境变量

		$ export NVM_DIR="$HOME/.nvm"
		$ [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm


###*Attention:*
- 当~目录下缺少.bash_profile时，每次启动终端都需要重新赋值环境变量
- 解决方法，手动添加.bash_profile文件，加入下面代码：

		source ~/.bashrc
		export NVM_DIR="$HOME/.nvm"
		[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
		[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

#OS X安装nginx
- 安装homebrew(如已安装则忽略)，自行到官网查看安装命令
- 使用homebrew安装nginx

		$ brew install nginx

- 安装好的程序和配置文件路径分别为/usr/local/Cellar/nginx/, /usr/local/etc/nginx/
- nginx启动，关闭及重启

		# 启动
		$ nginx
		# 重启
		$ nginx -s reload
		# 关闭(查找nginx的master pid，然后杀死进程)
		$ ps -ef | grep nginx
		$ kill -QIUT {pid}

###*Reference*
>- [在Mac上安装nginx](http://www.jianshu.com/p/46b083bfd5e0)

#nginx配置https
- 在任意一个目录创建存放证书的文件夹
- 通过Terminal进入该文件夹
- 创建服务器私钥，需要输入一个口令

		$ openssl genrsa -des3 -out server.key 1024

- 创建签名请求的证书(CSR)

		$ openssl req -new -key server.key -out server.csr
		# 然后需要需要相关信息

- 在加载SSL支持的Nginx并使用上述私钥时除去必须的口令

		$ cp server.key server.key.org
		$ openssl rsa -in server.key.org -out server.key

- 最后标记证书使用上述私钥和CSR

		$ openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

###* Reference*
>- [nginx配置HTTPS](http://blog.csdn.net/weixin_35884835/article/details/52588157)

#iTerm2配置免密连接remote linux
- 编写好eexpect脚本

		#!/usr/bin/expect  
		set timeout 30  
		spawn ssh [lindex $argv 0]@[lindex $argv 1]  
		expect {  
		        "(yes/no)?"  
		        {send "yes\n";exp_continue}  
		        "password:"  
		        {send "[lindex $argv 2]\n"}  
		}  
		interact

- chmod添加执行权限
- 将脚本文件添加到/usr/local/bin下
- profiles中的设置选择"Login shell"
- 在"Send text at start"中输入

		shell.exp {username} {host} {password}