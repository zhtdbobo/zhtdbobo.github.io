# nginx


[toc]
# 部署
### 下载
- 使用wget[下载](http://nginx.org/en/download.html)
- 下载到本地，上传到linux，解压缩
	```shell
	cd /home/用户
	tar -zxvf nginx-1.24.0.tar.gz
	```

- 安装依赖
	```
	yum install -y gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
	```

### 安装

- 进入nginx目录
	```shell
	cd nginx-1.24.0
	```
- 配置，编译，安装
	```shell
	./configure
	make
	make install
	```
### 运行
- 进入sbin目录
	```shell
	cd /usr/local/nginx/sbin
	./nginx
	```
- 验证
	```shell
	curl http://localhost:80
	```
- 其他
	```shell
	./nginx -s stop 关闭
	./nginx -s reload 重启
	```
# 注册服务
1. 编辑文件
	```shell
	vim /usr/lib/systemd/system/nginx.service
	```
2. 写入
	- 内容
		```vim
		[Unit]
		Description=nginx
		After=network.target
		
		[Service]
		Type=forking
		ExecStart=/usr/local/nginx/sbin/nginx
		ExecReload=/bin/kill -s HUP $MAINPID
		ExecStop=/bin/kill -s QUIT $MAINPID
		PrivateTmp=true
		
		[Install]
		WantedBy=multi-user.target
		```
	- 说明
		```
		[Unit]:服务的说明
		Description:描述服务
		After:描述服务类别
		
		[Service] 服务运行参数的设置
		Type=forking 后台运行的形式
		ExecStart 服务的具体运行命令
		ExecReload 重启命令
		ExecStop 停止命令
		PrivateTmp=True 给服务分配独立的临时空间
		注意：启动、重启、停止命令全部要求使用绝对路径
		
		[Install] 服务安装的相关设置，可设置为多用户
		```
3. 刷新服务
	```shell
	systemctl daemon-reload
	```
# 配置代理
1. 修改nginx.conf
- http
	```
	location / {
		root   html;
		index  index.html index.htm;
		proxy_pass  http://127.0.0.1:端口;
	}
	
	```
- websocket
	> proxy_http_version 必须使用http 1.1
	> proxy_set_header、proxy_set_header必填固定
	> proxy_pass 协议写http，ip和port写ws访问的实际地址
	```
	location /接口名 {
		proxy_pass  http://127.0.0.1:端口/接口名;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection "upgrade";
	}
	```
2. 重启
	```
	cd /usr/local/nginx/sbin
	./nginx -s reload
	```

# Q&A
### 安装后出现403
1. 打开nginx.conf
2. 将首行的user  nobody 改为  当前登录用户
