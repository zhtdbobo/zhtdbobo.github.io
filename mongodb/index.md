# mongodb


[toc]
# 安装
1. [官网](https://www.mongodb.com/try/download/community)下载
2. 上传到服务器并解压，移动到 /usr/local 目录下
	```shell
	tar -zxvf mongodb-linux-x86_64-rhel70-5.0.9.tgz
	mv mongodb-linux-x86_64-rhel70-5.0.9 mongodb
	mv mongodb-linux-x86_64-rhel70-5.0.9 /usr/local/mongodb
	```

3. 进入 mongodb 目录，并创建文件夹 data，在 data 文件夹下再创建 db 文件夹（用于存放数据库数据）和 log文件夹（存放 mongo 日志）。然后为其设置可读写权限。
	```shell
	cd /usr/local/mongodb/
	mkdir data data/db data/log
	chmod 666 data/db data/log/
	```
4. 在 mongodb 目录下新建配置文件 mongodb.conf，默认端口27017
	```shell
	vim mongodb.conf
	```

	```vim
	# 数据库数据存放目录
	dbpath=/usr/local/mongodb/data/db
	# 日志文件存放目录
	logpath=/usr/local/mongodb/data/log/mongodb.log
	# 日志追加方式
	logappend=true
	# 端口
	port=27017
	# 是否认证
	auth=true
	# 以守护进程方式在后台运行
	fork=true
	# 远程连接要指定ip，否则无法连接；0.0.0.0代表不限制ip访问
	bind_ip=0.0.0.0
	```
5. 配置环境变量
	```shell
	vim /etc/profile
	```
	末尾加入如下内容：
	```shell
	export MONGODB_HOME=/usr/local/mongodb
	export PATH=$PATH:$MONGODB_HOME/bin
	```
	重启系统配置
	```shell
	source /etc/profile
	```
6. 开机自启动
	```shell
	vim /lib/systemd/system/mongodb.service 命令新建开机启动配置文件
	```
	输入以下内容：
	```shell
	[Unit]
		Description=mongodb
		After=network.target remote-fs.target nss-lookup.target
	[Service]
		Type=forking
		ExecStart=/usr/local/mongodb/bin/mongod -f /usr/local/mongodb/mongodb.conf
		ExecReload=/bin/kill -s HUP $MAINPID
		ExecStop=/usr/local/mongodb/bin/mongod -f /usr/local/mongodb/mongodb.conf --shutdown
		PrivateTmp=true
	[Install]
		WantedBy=multi-user.target
	```
	然后依次执行以下4个命令，使之生效。
	```shell
	systemctl start mongodb.service
	systemctl status mongodb.service
	systemctl enable mongodb.service
	systemctl daemon-reload
	```
7. 输入mongo进入数据库
	创建管理员帐号
	
	```sql
	use admin
	db.createUser({user:"管理员",pwd:"密码",roles:[{role:"userAdminAnyDatabase",db:"admin"}]})
	```
	> 认证：如果不进行 db.auth("用户名","密码") 进行用户验证的话，是执行不了任务命令的，只有通过认证才可以。注意，每一个用户都需要在创建这个用户的认证库下进行认证。
	```
	db.auth("管理员","密码")
	```
	创建具体数据库的用户，赋予readWrite权限和dbAdmin权限
	```
	use test
	db.createUser({user:'用户名',pwd:'密码',roles:[{role:'readWrite',db:'数据库名'},{role:'dbAdmin',db:'数据库名'}]})
	```
	退出当前用户，使用新用户登录
	
	```
	exit
	db.auth("用户名","密码")
	use 数据库名
	```
	
	创建集合
	
	```
	db.createCollection("集合名")
	```
8. 使用navicat连接远程mongodb，默认端口27017，输入验证的数据库、用户名、密码
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/mongodb.png))
# docker安装mongodb
1. 拉取mongodb镜像，并查看
	```shell
		docker pull mongo:latest
		docker images
	```
2. 不带验证，启动mongodb容器,参数如下: 
	>  -p 端口映射（本机端口：容器端口） 
	>  -v 数据卷映射 
	>   --name 命名  
	>   -d 后台运行
	```
		docker run -p 27018:27017 -v /data/mongo:/data/db --name mongodb -d mongo
	```

3. 进入容器，进入mongodb控制台，最后创建各种用户，可以使用上面的步骤。
	```shell
	docker exec -it mongodb /bin/bash
	```
	输入mongo进入
4. 删除原有容器，用验证方式启动mongodb容器
	```shell
	docker rm -f mongodb
	docker run -p 27018:27017 -v /data/mongo:/data/db --name mongodb -d mongo --auth
	```
