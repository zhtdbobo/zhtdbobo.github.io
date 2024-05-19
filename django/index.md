# django


[toc]
# 创建项目
### 支持websocket
> 版本
> python：3.7
> django：3.2.22
> channels: 3.0.0
1. 安装依赖
	```
	pip install channels == 3.2.22
	pip install django == 3.0.0
	```
2. 创建app
	> 添加成功会显示一个新的app的文件夹
	- 命令行
		```
		python manage.py startapp app名字
		```
	- 添加consumers.py
		```python
		from channels.generic.websocket import WebsocketConsumer
		from channels.exceptions import StopConsumer


		class ChatConsumer(WebsocketConsumer):
			def websocket_connect(self, message):
				# 有客户端来向后端发送websocket连接的请求时，自动触发。
				# 服务端允许和客户端创建连接。
				self.accept()
	
			def websocket_receive(self, message):
				# 浏览器基于websocket向后端发送数据，自动触发接收消息。
				print(message)
				self.send("小天：\n\t" + message['text'])
				# self.close()
	
			def websocket_disconnect(self, message):
				# 客户端与服务端断开连接时，自动触发。
				print("断开连接")
				raise StopConsumer()
		```
3. 配置项目的routing.py
	- 修改setting.py
		> 修改INSTALLED_APPS加上自己新建的app
		> 指定websocket使用asgi网关
		```
		INSTALLED_APPS = [
			'django.contrib.admin',
			'django.contrib.auth',
			'django.contrib.contenttypes',
			'django.contrib.sessions',
			'django.contrib.messages',
			'django.contrib.staticfiles',
			'channels',
			'app',
		]
		ASGI_APPLICATION = "ws.asgi.application"
		```
		> 修改允许其他电脑访问
		```
		ALLOWED_HOSTS = ["*"]
		```
	- 新建routing.py
		> 和项目的setting.py同级
		```python
		from django.urls import re_path
		
		from app import consumers
		
		websocket_urlpatterns = [
			re_path("^test", consumers.ChatConsumer.as_asgi()),
		]
		```
	- 修改asgi.py
		> asgi网关（异步服务）配置访问访问http和websocket
		```python
		import os
		from . import routing
		from channels.routing import ProtocolTypeRouter, URLRouter
		from django.core.asgi import get_asgi_application
		
		os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'ws.settings')
		
		application = ProtocolTypeRouter({
			"http": get_asgi_application(),
			"websocket": URLRouter(routing.websocket_urlpatterns),
		})
		```
# 执行项目
### 运行
- 迁移
	```
	python manage.py migrate
	```
- 执行
	- 本地访问
		```
		python manage.py runserver 端口号
		```
	- 其他电脑访问
		```
		python manage.py runserver 0.0.0.0:端口号
		```
- 截图
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/django%E7%9A%84websocket.png)
### 测试
- 截图
![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/%E6%B5%8B%E8%AF%95django%E7%9A%84websocket.png)
