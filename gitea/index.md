# gitea


[toc]
# 安装
### 安装mysql
### 安装git
- v1 
	> 使用yum安装，最高版本是1.8.3.1
	```shell
	yum install -y git
	```
- v2
	> 添加 End Point Package Repository，进行安装
	```shell
	yum remove -y git
	yum install -y https://packages.endpointdev.com/rhel/7/os/x86_64/endpoint-repo.x86_64.rpm
	yum install -y git
	```
### 安装gitea
- 1.16.8版本
1. [下载]( https://dl.gitea.io/gitea)
	```shell
	wget -O gitea https://dl.gitea.io/gitea/1.16.8/gitea-1.16.8-linux-amd64
	```
2. 创建用户和组
	```shell
	groupadd git
	useradd git -g git
	```
3. 下载文件、配置权限
	```shell
	mkdir /home/gitea /var/log/gitea
	cd /home/gitea
	wget https://dl.gitea.io/gitea/1.16.8/gitea-1.16.8-linux-amd64
	```
4. 使用git用户登录，创建文件
	```shell
	mkdir custom/conf
	vim app.ini
	```
5. 配置权限
	```shell
	chmod +x gitea
	chown -R git:git /home/gitea/gitea /var/log/gitea
	```
6. 运行gitea
	- 直接运行
		```shell
		./gitea web
		```
	- 创建服务
	1. 创建服务文件
		```
		[Unit]
		Description=Gitea
		After=syslog.target
		After=network.target
		
		[Service]
		RestartSec=2s
		Type=simple
		User=git
		Group=git
		ExecStart=/home/gitea/gitea web --config /home/gitea/custom/conf/app.ini
		Restart=always
		
		[Install]
		WantedBy=multi-user.target
		```
	2. 重新加载系统服务
		```shell
		systemctl daemon-reload
		```
	3. 使用service
		```shell
		service gitea stop
		service gitea start
		service gitea restart
		```
- [2.39.3版本](https://docs.gitea.com/zh-cn/installation/install-from-binary)
# 配置


# Q&A
