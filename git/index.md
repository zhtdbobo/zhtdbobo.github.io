# git


[toc]
# 基本使用
### 创建本地的git用户信息
1. 项目级别/仓库级别
```
	git config user.name lyb_pro
	git config user.email jaridli_pro@163.com
```
2. 操作系统用户级别
```
	git config --global user.name lyb
	git config --global user.email jaridli@163.com
```
### 生成ssh密钥配置到git
1. 判断本机是否存在ssh密钥
```
	cd ~/.ssh
	ls
```
2. 如果没有 id_rsa.pub 或 id_dsa.pub 文件，生成密钥
```
	ssh-keygen -t rsa -C "your_email@example.com"
```
3. 复制.pub中的内容到github的ssh，在本地git bash进行测试
```
	ssh -T -p 443 git@github.com
```
### 本地仓库状态
1. 添加到暂存区
```
	git add .
```
2. 提交分支
```
	git commit -m "提交信息"
```
### 分支
1. 创建分支
```
	git branch 分支名
```
2. 切换分支
```
	git checkout 分支名
```
3. 创建并切换分支
```
	git checkout -b 分支名
```
### 推送
1. 连接远程仓库
	- 添加远程别名
	```
		git remote add 别名 远程地址
	```
	- 删除远程别名
	```
		git remote remove 别名
	```
	- 查看remote信息
	```
		git remote -v
	```
2. 推送到远程仓库
	- 带分支名，会在远程==自动创建一个同名的分支==
	```
		git push 别名 分支名
	```
	- 本地有，远程没有，推送的同时在远程创建
	```
		git push 别名 本地存在的分支名:即将创建的远程分支名
	```
	- 不带分支名，先通过upstream和远程分支建立关联，再推送
	```
		git push --set-upstream 别名 远程分支名
		git push
	```
3. 强制推送
	```
		git push -f origin master
	```
4. 远程仓库添加多个url同时push，pull
	- 找到.git下面的config文件
	- 添加url
# Q&A
### Connection refused
> ssh: connect to host github.com Port : 22 Connection refused
1. 添加配置文件，并编辑
	```shell
	vim config
	```
2. 添加如下内容
	```
	Host github.com
	User 820096913@qq.com
	Hostname ssh.github.com
	PreferredAuthentications publickey
	IdentityFile ~/.ssh/id_rsa
	Port 443
	```
