# mysql


[toc]
# 安装mysql
1. 将下载好的mysql上传到服务器
- 将mysql的tar.gz文件上传到服务器/home/software文件夹下
	tar -zxvf mysql-5.7.40-el7-x86_64.tar.gz
	解压	
- mv mysql-5.7.40-el7-x86_64 mysql
	重命名	
2. 检查是否安装了mysql和mariadb
```
	rpm -qa | grep mysql
	rpm -qa | grep mariadb
```
如果存在，卸载mariadb
```
	rpm -e --nodeps mariadb-libs-5.5.68-1.el7.x86_64
```

3. 安装需要的依赖包
```
	yum -y install libaio
```
4. 创建MySQL安装目录和数据存放目录，并授权
```
	mkdir /usr/local/mysql
	mv mysql /usr/local/mysql
	mkdir /usr/local/mysql/mysqldb
	chmod -R 777 /usr/local/mysql
	chmod -R 777 /usr/local/mysql/mysqldb/
```
5. 创建MySQL组：创建MySQL用户，并设置密码。
```
	groupadd mysql
	useradd mysql -g mysql
	passwd mysql
	密码
```
6. 将mysql目录的权限授给mysql用户和mysql组
```
	chown -R mysql:mysql /usr/local/mysql
```
7. 创建MySQL的安装初始化配置文件
- vim /etc/my.cnf
	配置文件
-  填入以下内容	
	```
	[mysqld]
	# 设置3307端口
	port=3307
	# 设置mysql的安装目录
	basedir=/usr/local/mysql/mysql
	# 设置mysql数据库的数据的存放目录
	datadir=/usr/local/mysql/mysqldb
	# 设置group_concat最大数值
	group_concat_max_len = 2000000
	# 允许最大连接数
	max_connections=10000
	# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
	max_connect_errors=10
	# 服务端使用的字符集默认为UTF8
	character-set-server=utf8
	# 创建新表时将使用的默认存储引擎
	default-storage-engine=INNODB
	# 默认使用“mysql_native_password”插件认证
	default_authentication_plugin=mysql_native_password
	[mysql]
	# 设置mysql客户端默认字符集
	default-character-set=utf8
	[client]
	# 设置mysql客户端连接服务端时默认使用的端口
	port=3307
	default-character-set=utf8
	```
8. 安装mysql
- cd /usr/local/mysql/mysql/bin/
- ./mysqld --initialize --console
	- 记录默认生成的mysql密码 	
	
	  > 例如：NFhGPyp2EW%f
	- ==如果 error while loading shared libraries: libnuma.so.1:==
	  - yum remove libnuma.so.1
	  	卸载32位的
	  - yum -y install numactl.x86_64
	  	安装64位的
9. 启动mysql
```
	cd ../support-files/
```

==修改mysql.server中conf位置为自己的my.cnf存在的位置==
```
	yum -y install vim
	vim mysql.server
```
启动
```
	./mysql.server start
```
==出错的话在执行一次授权==
```
	chmod -R 777 /usr/local/mysql
	chmod -R 777 /usr/local/mysql/mysqldb/
```
10. 将MySQL加入系统进程中
- cp mysql.server /etc/init.d/mysqld
- service mysqld restart
	- 重启mysql服务
	- 如果没有service命令，执行以下命令列出initscripts，并安装
		- yum list | grep initscripts
		- yum -y install initscripts
- systemctl enable mysqld	
	开机自启
11. 修改mysql密码
```
	cd ../bin/
	./mysql -u root -p	输入默认生成的mysql密码
	ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '密码';
```
12. 设置远程登录
```
	use mysql;
	update user set host='%' where user='root';
	flush privileges;
```
13. 退出MySQL	并重启服务
```
	exit
	service mysqld restart
```
14. 删除已经安装的yum包
```
	yum -y remove vim numactl.x86_64
```
---
---
# mysql的http插件扩展
1. 将mysql-udf-http的tar.gz文件上传到服务器/home/software文件夹下，并进入文件夹
```
	tar -zxvf mysql-udf-http-1.0.tar.gz
	cd mysql-udf-http-1.0
```
2. 配置
```
	./configure --prefix=/usr/local/mysql/mysql --with-mysql=/usr/local/mysql/mysql/bin/mysql_config
```

- 参数
	- prefix=“mysql安装路径”
	- with-mysql=“mysql_config所在路径”
- 提示no acceptable C compiler
	- yum -y install gcc-c++
- 提示No package 'libcurl'
	- yum -y install libcurl-devel.x86_64 
- 安装依赖包
	- yum -y install automake autoconf libtool make
3. 编译并安装
```
	make & make install
```
4. 安装好的插件在这里，需要进行移动到plugin目录下，否则加载不到无法创建函数
```
	cd /usr/local/mysql/mysql/lib
	mv mysql-udf-http.* plugin/
```
5. 在navicat中远程连接数据库，新建查询执行下面命令添加函数
```sql
	create function http_get returns string soname 'mysql-udf-http.so';
	create function http_post returns string soname 'mysql-udf-http.so';
	create function http_put returns string soname 'mysql-udf-http.so';
	create function http_delete returns string soname 'mysql-udf-http.so';
```
6. 在navicat检查是否安装成功，
```
	SELECT http_get('http://m.baidu.com/s?word=xoyo&pn=0');  
```
- 如果有返回值则成功
- ![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/mysql%E5%AE%89%E8%A3%85http%E6%88%90%E5%8A%9F.png)
- 具体用法见[这里](http://zyan.cc/mysql-udf-http/)
7. 删除已经安装的yum包
```
	yum -y remove gcc-c++ libcurl-devel.x86_64 automake autoconf libtool make

```
---
---
# 定时任务
1. 查看状态
	```
		show variables like 'event_scheduler';
	```
	- 如果显示为off，执行如下
	```
		set global event_scheduler = on;
	```
	- 注意：服务器重启或者mysql重启会失效，建议修改配置文件加上下面的配置
	```
		event_scheduler=ON
	```

# Q&A
1. max_allowed_packet太小
- 进入mysql控制台
	```sql
	set global max_allowed_packet = 1024*1024*16;
	```
