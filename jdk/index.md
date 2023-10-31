# jdk


[toc]
# win10
1. 添加JAVA_HOME环境变量
	```
	E:\java\jdk
	```
2. 添加CLASSPATH环境变量
	```
	%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
	```
3. Path添加两项          %JAVA_HOME%\bin    %JAVA_HOME%\jre\bin;
	```
	%JAVA_HOME%\bin    
	%JAVA_HOME%\jre\bin;
	```
# win11
1. 添加JAVA_HOME环境变量
	```
	E:\java\jdk
	```
2. Path添加一项          
	```
	%JAVA_HOME%\bin
	```
# 临时切换jdk版本
1. 打开cmd
2. 输出以下代码，并验证
	```
	set path=E:\java\jdk11\bin;%path%
	java -version
	```
