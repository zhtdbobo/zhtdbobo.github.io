# tomcat


[toc]
# 基础安装
1. 下载tomcat。官网
2. 检查linux是否安装tomcat
	```shell
	rpm -qa | grep tomcat
	```
3. 将下载好的tomcat上传到服务器，解压缩并移动
	```shell
	tar -zxvf apache-tomcat-9.0.62.tar.gz 
	mv apache-tomcat-9.0.62 tomcat9
	mv tomcat9/ /usr/local/
	```
4. 启动/关闭tomcat，在bin目录下执行
	```shell
	cd bin
	./startup.sh
	./shutdown.sh
	```
# 扩展配置
## 同时运行多个tomcat
- 修改端口号不一样即可（shutdown， http ，https）
	```
		shutdown:8005
		http:8080
		https:8443
	```
## 设置开机自启
1. 切换目录
	```
	cd /etc/init.d
	vim tomcat9
	```
2. 复制以下代码：保存退出
```#!/bin/bash
	# description: Tomcat Start Stop Restart
	# processname: tomcat
	# chkconfig: 2345 20 80
	#idea - tomcat config start 
	#!/bin/bash
	# description: Tomcat Start Stop Restart
	# processname: tomcat
	# chkconfig: 2345 20 80
	JAVA_HOME=/usr/local/java/jdk
	export JAVA_HOME
	PATH=$JAVA_HOME/bin:$PATH
	export PATH
	CATALINA_HOME=/usr/local/tomcat9
	case $1 in
	start)
	sh $CATALINA_HOME/bin/startup.sh
	;;
	stop)
	sh $CATALINA_HOME/bin/shutdown.sh
	;;
	restart)
	sh $CATALINA_HOME/bin/shutdown.sh
	sh $CATALINA_HOME/bin/startup.sh
	;;
	esac
	exit 0
```
3. 为 tomcat 分配可执行权限：chmod +x tomcat9
4. 添加 tomcat 为系统服务：chkconfig --add tomcat9
5. 输入命令查看是否添加成功： chkconfig --list
6. 启动 tomcat命令：service tomcat9 start
7. 使用浏览器访问服务器的8080端口是否有tomcat主页出现
## 配置支持https
1. 上传ssl的ks证书到服务器，如jaridli.top.jks
2. 修改conf/server.xml，配置http连接和https连接
   redirectPort是让一个端口直接重定向到另外一个
   <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
   <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true" 
               maxThreads="150" scheme="https" secure="true" 
               keystoreFile="*******/jaridli.top.jks" 
               keystorePass="****" clientAuth="false"/>
3. 修改conf/web.xml，使HTTP 自动跳转 HTTPS，在结束标签</welcome-file-list> 后面换行，并添加以下内容，重启tomcat
    <login-config>
        <!-- Authorization setting for SSL -->
        <auth-method>CLIENT-CERT</auth-method>
        <realm-name>Client Cert Users-only Area</realm-name>
    </login-config>
    <security-constraint>
        <!-- Authorization setting for SSL -->
       <web-resource-collection>
           <web-resource-name>SSL</web-resource-name>
           <url-pattern>/*</url-pattern>
       </web-resource-collection>
       <user-data-constraint>
           <transport-guarantee>CONFIDENTIAL</transport-guarantee>
       </user-data-constraint>
    </security-constraint>


## 使用tomcat作为上传文件服务器
1. tomcat服务器默认是不可写操作，只允许读，所以在conf/web.xml文件中的servlet标签内加入readonly：false,重启tomcat
<init-param>
    <param-name>readonly</param-name>
    <param-value>false</param-value>
</init-param>
2. 创建自己需要上传到的目的文件夹，并上传一张图片用于测试
3. 修改conf/server.xml，配置目的文件夹和外网访问路径的映射，在末尾host标签内添加如下配置，并重启服务器
<Context docBase ="/home/img/logo/" path ="/logo" debug ="0" reloadable ="true"/>
参数说明：
docBase ="图片的真实放置的路径文件夹"
path="外网访问的路径"
4. 在浏览器访问服务器ip:8080/外网访问路径/文件名称
