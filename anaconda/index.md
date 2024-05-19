# anaconda


[TOC]
# conda安装
1. 配置环境变量
```
	E:\Anaconda
	E:\Anaconda\Scripts
	E:\Anaconda\Library\bin
```
2. 可以在c盘的.condarc添加环境默认的安装路径，修改目标路径的访问权限为完全控制
```yaml
	envs_dirs: 
	  - E://Anaconda//envs #新的环境保存位置
```
# conda换源
1. conda换源
	- 在cmd中输入
		```shell
		conda config --set show_channel_urls yes
		```
		在C盘用户目录下会生成.condarc文件。
	- 然后使用记事本打开，用下面的内容将其替换
		```
		show_channel_urls: true
		channels:
		  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
		  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
		  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
		  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
		  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
		  - defaults
		
		```
		
	- 最后保存，然后在CMD中输入
		```shell
		conda clean -i
		```
		此时Anaconda已经换源成功！
2. 查看conda信息
	```shell
		conda info
	```
3. python换源
	- 在CMD中输入
		```
		pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
		```
# conda常用命令
1. 创建环境
	- 新环境
		```
		conda create -n env-name python=version
		```
	- 从已有环境复制
		```
		conda create -n new_env_name --clone old_env_name
		```
2. 激活环境
    ```
    conda activate env-name
    ```
3. 并在新的环境中配置jupyter notebook
	```
	activate env-name
	pip install ipykernel
	python -m ipykernel install --user --name env-name --display-name env-name
	```
4. conda删除一个环境
	```
	conda remove -n env-name --all
	```
5. jupyter删除kernel环境
	```
	jupyter kernelspec remove env-name
	```
6. 查看环境
	```
	conda env list
	```
7. 在已有的环境中更换python版本，如从3.7变为3.8
	```
	conda install python==3.8
	```
# 添加conda右键菜单
1. 在空白处添加，修改代码中的icon路径和bat文件路径
	```
	REG ADD HKCR\Directory\Background\shell\Conda\ /ve /f /d "Conda Prompt Here"
	REG ADD HKCR\Directory\Background\shell\Conda\ /v Icon /f /t REG_EXPAND_SZ /d C:\miniconda3\Menu\Iconleak-Atrous-Console.ico
	REG ADD HKCR\Directory\Background\shell\Conda\command /f /ve /t REG_EXPAND_SZ /d "%windir%\System32\cmd.exe "/K" C:\miniconda3\Scripts\activate.bat
	```
2. 在目录上添加，修改代码中的icon路径和bat文件路径
	```
	REG ADD HKCR\Directory\shell\Conda\ /ve /f /d "Conda Prompt Here"
	REG ADD HKCR\Directory\shell\Conda\ /v Icon /f /t REG_EXPAND_SZ /d C:\miniconda3\Menu\Iconleak-Atrous-Console.ico
	REG ADD HKCR\Directory\shell\Conda\command /f /ve /t REG_EXPAND_SZ /d "%windir%\System32\cmd.exe "/K" C:\miniconda3\Scripts\activate.bat
	```
# jupyter notebook
1. 安装
	```
	pip install jupyter notebook 
	```
2. 修改默认工作目录
	- 生成配置文件
		```
		jupyter notebook --generate-config
		```
	- 搜索配置文件
		```
		C:\Users\JaridLi\.jupyter\jupyter_notebook_config.py
		```
	- 搜索notebook_dir关键词，修改值为自己的目录
		```
		c.NotebookApp.notebook_dir = 'E:\Jupyter'
		```
# 生成开始菜单
```
python .\Lib\_nsis.py mkmenus
```

# Q&A

1. pycharm终端无法激活conda环境
   - 原因：pycharm 默认的终端是 Windows PowerShell
   - 解决：新建一个cmd的终端
   ![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/pycharm%E4%B8%8D%E6%BF%80%E6%B4%BBanaconda.png)
