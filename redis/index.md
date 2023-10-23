# redis


[toc]
# 安装
1. 上传文件到/home/software
解压	 tar -zxvf redis-6.2.1.tar.gz
2. 检查gcc版本	gcc --version
3. 进入目录	cd redis-6.2.1
执行	make
跳过	make test
执行	make install
4. 安装成功在 /usr/local/bin
# 配置
1. 备份配置文件	
mkdir	/myredis
cp  /home/software/redis-6.2.1/redis.conf  /myredis
2. 修改配置文件redis.conf
设置后台启动	daemonize no 改成 yes
修改默认端口	port 6379改为6380
允许其他主机访问	找到	bind 127.0.0.1 -::1	加 # 注释，
protected-mode no 改成 yes
3. 指定端口访问
	- 启动redis服务	redis-server /myredis/redis.conf
	- 进入实例	redis-cli -p 6380
	- 测试	ping	返回pong代表成功
4. 其他
exit	退出命令行
shutdown	关闭实例	或者在命令行外使用redis-cli -p 6380 shutdown
5. 密码访问
	- 设置密码	requirepass 
	- 登录时使用auth进行验证
