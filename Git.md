#配置服务器自动化更新
- 新建一个目录作为要部署代码的根目录 
```bash
$ mkdir /root/repository
```
- 进入项目根目录，初始化为git仓库，让仓库接受代码提交
```bash
$ cd /root/repository
$ git init
$ git config receive.denyCurrentBranch ignore
```
- 在本地将服务器添加到远程仓库列表，使用名字来区分不同的服务器
```bash
$ git remote add repository ssh://root@IP/root/repository/.git
```
- 将本地代码提交到测试服务器上面
```bash
$ git push repository master
```
- 更新服务端 git 仓库状态并检出文件
```bash
$ git update-server-info
$ git checkout -f
```
- 设置服务器端更新钩子，新建 post-receive文件，并添加bash
```bash
$ cd .git/hooks
$ vim post-receive
//下面是添加的内容
#!/bin/sh
unset GIT_DIR
cd /root/repository
git add . -A && git stash
git pull origin master
git update-server-info
git checkout -f
```
- 为 post-receive 添加可执行权限
```bash
$ chmod +x post-receive
```
- *Attention:*
>- post-receive文件中的文本颜色为蓝白色才生效（未清楚原因）
- *Reference:* 
>- [使用git做服务器端代码的部署](https://www.douban.com/note/407034249/), [使用Git Hook实现网站的自动部署](http://www.tuicool.com/articles/3QRB7jU)

#创建与合并分支
###*关于分支的基本命令*
- 创建分支
```bash
$ git checkout -b dev
//相当于 $ git branch dev && $git checkout dev
```
- 查看分支
```bash
$ git branch
```
- 删除分支
```bash
$ git branch -d dev
```

###*合并其他分支到当前分支*
- 提交dev分支的修改
```bash
$ git add .
$ git commit -m 'msg'
```
- 切换回到master分支
```bash
$ git checkout master
```
- 合并到当前分支
```bash
$ git merge dev
$ git push //最后提交合并
```
- *Reference:* 
>- [创建与合并分支](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000)
