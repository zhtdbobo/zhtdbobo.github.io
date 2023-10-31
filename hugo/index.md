# hugo


[TOC]
# hugo
### 安装
1. win+r输入：powershell，使用 
	```shell
	scoop install hugo-extended
	hugo version //查看是否安装成功
	```
2. 进入自己的文件夹，创建自己的网站
	```shell
	hugo new site jaridli
	cd jaridli
	```
3. 初始化当前目录中的空 Git 存储库。
	```shell
	git init
	```
4. 安装LoveIt主题
	- [下载压缩包](https://github.com/dillonzq/LoveIt/releases)
	- 解压，名字为LoveIt，放在博客根目录的themes中
	- 将exampleSite中的config.toml文件复制到博客根目录
5. 启动服务，访问本地路径http://localhost:1313/
	```
	hugo server
	```
### 文章测试
1. 创建第一篇文章
	- 创建md文件
		```
		hugo new posts/test.md
		```
	- Hugo 不会在您构建网站时发布草稿内容。即
		```
		---
		title: "My First Post"
		date: 2022-11-20T09:03:20-08:00
		draft: true		//变为false即不使用草稿模式
		---
		```
	- 将draft变为false，再次启动
	```
		hugo server
	```
### 部署在github
1. git建立仓库
	形式：账户名.github.io
	如 zhtdbobo.github.io
2. 静态网站
	- 打开本地博客根目录
	- 输入hugo命令
	- public文件夹中生成静态网站
3. 将自己本地的文件夹推送到github
	- 进入public文件夹
	- 
5. 访问地址https://zhtdbobo.github.io


### 配置自定义域名
# 文章
### post头
1. 分类
	categories: ['软考']
