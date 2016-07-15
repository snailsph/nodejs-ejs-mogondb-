# nodejs-ejs-mogondb-
nodej+ejs模板，通过mogondb数据查询数据实现简单的登陆验证。

搭建mogondb：

一、首先安装mongodb

1.下载地址：http://www.mongodb.org/downloads

2.解压缩到自己想要安装的目录，比如d:\mongodb

3.创建文件夹d:\mongodb\data\db、d:\mongodb\data\log，分别用来安装db和日志文件，在log文件夹下创建一个日志文件MongoDB.log，即d:\mongodb\data\log\MongoDB.log

4.运行cmd.exe进入dos命令界面，执行下列命令
　　> cd d:\mongodb\bin 
　　> d:\mongodb\bin>mongod -dbpath "d:\mongodb\data\db"
　看到类似的信息，则说明启动成功，默认MongoDB监听的端口是27017，mysql的是3306

5.测试连接
　新开一个cmd窗口，进入mongodb的bin目录，输入mongo或者mongo.exe，出现如下信息说明测试通过，此时我们已经进入了test这个数据库，如何进入其他数据库下面会说。
　输入exit或者ctrl+C可退出。

 6.当mongod.exe被关闭时，mongo.exe 就无法连接到数据库了，因此每次想使用mongodb数据库都要开启mongod.exe程序，所以比较麻烦，此时我们可以将MongoDB安装为windows服务
　还是运行cmd，进入bin文件夹，执行下列命令
　> d:\mongodb\bin>mongod --dbpath "d:\mongodb\data\db" --logpath "d:\mongodb\data\log\MongoDB.log" --install --serviceName "MongoDB"
　这里MongoDB.log就是开始建立的日志文件，--serviceName "MongoDB" 服务名为MongoDB
　接着启动mongodb服务
　> d:\mongodb\bin>NET START MongoDB
　打开任务管理器，可以看到进程已经启动

7.关闭服务和删除进程
　> d:\mongodb\bin>NET stop MongoDB   (关闭服务)
　> d:\mongodb\bin>mongod --dbpath "d:\mongodb\data\db" --logpath "d:\mongodb\data\log\MongoDB.log" --remove --serviceName "MongoDB"      (删除，注意不是--install了）

二、创建hello-world数据库
  1、mongo.exe启动
  2、创建数据库 use hello-world
  3、插入数据（登陆时的账号密码）db.users.insert({"userid" : "admin", "password" :"123456"}) 
  
  这样子就ok了；
  
  
三：安装node依赖包

  npm install
  
四：启动node项目
  node app.js
  显示 端口：3000启动成功


附件：使用mongodb基础知识

1.常用的命令

show dbs    显示数据库列表
use dbname    进入dbname数据库，大小写敏感，没有这个数据库也不要紧
show collections    显示数据库中的集合，相当于表格

2.创建&新增

db.users.save({"name":"lecaf"})    创建了名为users的集合，并新增了一条{"name":"lecaf"}的数据
db.users.insert({"name":"ghost", "age":10})    在users集合中插入一条新数据，，如果没有users这个集合，mongodb会自动创建
save()和insert()也存在着些许区别：若新增的数据主键已经存在，insert()会不做操作并提示错误，而save() 则更改原来的内容为新内容。
存在数据：{ _id : 1, " name " : " n1 "} ，_id是主键
insert({ _id : 1, " name " : " n2 " })    会提示错误
save({ _id : 1, " name " : " n2 " })     会把 n1 改为  n2 ，有update的作用。

3.删除

db.users.remove()    删除users集合下所有数据
db.users.remove({"name": "lecaf"})    删除users集合下name=lecaf的数据
db.users.drop()或db.runCommand({"drop","users"})    删除集合users
db.runCommand({"dropDatabase": 1})    删除当前数据库

4.查找

db.users.find()    查找users集合中所有数据
db.users.findOne()    查找users集合中的第一条数据

5.修改

db.users.update({"name":"lecaf"}, {"age":10})    修改name=lecaf的数据为age=10，第一个参数是查找条件，第二个参数是修改内容，除了主键，其他内容会被第二个参数的内容替换，主键不能修改
