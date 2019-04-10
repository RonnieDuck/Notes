#mysql 1055如何解决
- 错误信息：[Err] 1055 - Expression #1 of ORDER BY clause is not in GROUP BY clause and contains nonaggregated column 'information_schema.PROFILING.SEQ' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
- 进入mysql，查看是否有该配置
```sql
select @@sql_mode;
```
- 打开mysql配置文件
```bash
$ sudo vim /etc/mysql/my.cnf
```
- 添加行
```sql
sql_mode=STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
```
- *Attention:*
>- 如果配置文件内没有[mysqld]块区或改块区已被注释，则手动添加或反注释
- *Reference:*
>- [MySQL5.7 group by新特性，报错1055](http://blog.csdn.net/u283056051/article/details/52463948)

#mysql group_concat()函数
- 将多条数据的某一个字段整合在一起
```sql
select group_concat(`field`) from `table`;
```
- *Attention:*
>- 该函数的返回值有长度限制1024，结果数据量较大时不建议使用
- *Reference:*
>- [关于mysql函数GROUP_CONCAT](http://www.poluoluo.com/jzxy/200812/53698.html), [MySQL group_concat() 长度限制](http://blog.csdn.net/hyman_xie/article/details/7299414)

#navicat for mysql 2003-Can't connect to mysql server on 'localhost'(10038)#如何解决
###*ubuntu解决方法*
- 查看端口是否打开
```bash
$ netstat -an | grep 3306
```
- 打开mysql配置文件
```bash
$ vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
- 将bind-address = 127.0.0.1注释掉
- 重启mysql
```bash
$ sudo etc/init.d/mysql restart
```
- 进入数据库将root用户授权给所有链接
```bash
$ grant all privileges on *.* to 'root'@'%' identified by '数据库连接密码';
```

###*centos解决方法*
- 如果user表中的host没有%，则修改
```sql
update user set host = '%' where user = 'root';
flush privileges;
```
- 修改防火墙
```bash
# 编辑防火墙配置文件
$ vim /etc/sysconfig/iptables
# 增加下面一行（注意：增加的开放3306端口的语句一定要在icmp-host-prohibited之前）
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
# 重启防火墙
service iptables restart
```
- 若发生错误：`Can't get hostname for your addressConnection closed by foreign host`
```bash
# 修改数据库配置文件
$ vim /etc/my.cnf
# 在[mysqld]节点下新增或修改如下两行
$ skip-name-resolve    #忽略主机名的方式访问  
$ lower_case_table_names=1    #忽略数据库表名大小写 
```
- *Reference:*
>- [ubuntu 15.04开放mysql远程3306端口](http://www.linuxdiyf.com/linux/15206.html)
>- [mysql开启远程访问权限及防火墙开放3306端口](http://blog.csdn.net/benben0503/article/details/51671680)

#sql语句：ON DUPLICATE KEY
- 根据表中的唯一索引(unique key)避免重复插入数据，一般为insert改update
- 需要满足与唯一索引完全相等的数据在表中已经存在
- 例如，如果列a被定义为UNIQUE，并且包含值1，则以下两个语句具有相同的效果：
```sql
INSERT INTO `table` (`a`, `b`, `c`) VALUES (1, 2, 3) ON DUPLICATE KEY UPDATE `c`=`c`+1; 
UPDATE `table` SET `c`=`c`+1 WHERE `a`=1;
```

#NodeJS mysql解决sql注入问题
- 引用mysql依赖，使用
```js
import mysql from 'mysql';
```
- escape方法转义变量配合模板字符串
```js
let sql = 'select * from tb_user where name = ${mysql.escape(name)}';
```
- 替换参数query（详细参考Nodejs.md-Node--MySQL）
- *Attention:*
>- 该方法会将任何类型转换为字符串类型