# javafx


[TOC]
# JavaFXSceneBuilder
### 使用
1. 下载

2. 打包
	- 在maven中引入依赖
	 ```xml
	 <dependencies>
        <dependency>
            <groupId>org.openjfx</groupId>
            <artifactId>javafx-controls</artifactId>
            <version>15.0.1</version>
        </dependency>
        <dependency>
            <groupId>org.openjfx</groupId>
            <artifactId>javafx-fxml</artifactId>
            <version>15.0.1</version>
        </dependency>
    </dependencies>
	 ```
	- 打包成exe，
		- 创建一个类文件来调用xxApplication
		- mainClass中指定这个类的名称
	- 配置插件
	```xml
	 <build>
        <plugins>
            <plugin>
                <groupId>io.github.fvarrui</groupId>
                <artifactId>javapackager</artifactId>
                <version>1.6.6</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>package</goal>
                        </goals>
                        <configuration>
                            <!-- 启动类 -->
                            <mainClass>sample.TimeGuardian</mainClass>
                            <!-- 绑定自定义JRE路径-->
                            <bundleJre>true</bundleJre>
                            <jrePath>E:\java\jre</jrePath>
                            <generateInstaller>true</generateInstaller>
                            <administratorRequired>false</administratorRequired>
                            <!-- 操作系统-->
                            <platform>windows</platform>
                            <copyDependencies>true</copyDependencies>
                            <!-- 名称与版本-->
                            <displayName>TimeGuardian</displayName>
                            <name>TimeGuardian</name>
                            <version>1.0</version>
                            <!-- 手动引入额外资源-->
                           	 <additionalResources>-->
                                <additionalResource>D:\Item\GD_AmtHardwareTest1.0\datas</additionalResource>
                                <additionalResource>D:\Item\GD_AmtHardwareTest1.0\lib</additionalResource>
                            </additionalResources>-->
                            <!--详细参数配置-->
                            <winConfig>
                                <icoFile>E:\Scipt\ahk\In-Time-Zone-Guardian\TimeGuardian.ico</icoFile>
                                <generateSetup>false</generateSetup>
                                <generateMsi>false</generateMsi>
                                <generateMsm>false</generateMsm>
                            </winConfig>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
	```
	- 右侧maven工具进行clean、package
	![](https://joplin-1-1304734442.cos.ap-nanjing.myqcloud.com/maven%E5%8F%B3%E4%BE%A7%E8%8F%9C%E5%8D%95.png)
# fxml
### TextField
1. 文本域值改变监听事件
```java
textField.textProperty().addListener(new ChangeListener(){
		@Override
		public void changed(ObservableValue observable, Object oldValue, Object newValue) {
			StartController.workTime = Integer.parseInt(workTextField.getText()) * 1;
		}
	});

```
# controller
### 线程
1. ui线程
	- 处理界面的变化
2. 自定义线程
	- 需要更新界面时，需要使用代码
		```java
		Platform.runLater(() -> {
			// 更新ui的代码
		});
		```
### 元素
1. stage
	- 设置全屏
		```java
		workStage.setFullScreen(true);
		workStage.setFullScreenExitHint("");//去除全屏的提示文字
		```
	- 透明、没有装饰
		```java
		workStage.initStyle(StageStyle.TRANSPARENT);
		```
	- 置顶
		```java
		workStage.setAlwaysOnTop(true);
		```
2. scene
	- 透明
		```java
		workScene.setFill(null);
		```
	- 半透明，使用rgba设置背景颜色
	```fxml
		style = "-fx-background-color: rgba(255,255,255,0.5);"
	```
3. 背景颜色
	```java
	workPane.setBackground(new Background(new BackgroundFill(new Color(0.65,0.23,0,0.86), null, null)));
	```
### 快捷键
1. 重置时间
	```java
		KeyCombination altb=new KeyCodeCombination(KeyCode.B,KeyCombination.ALT_DOWN);
		workScene.getAccelerators().put(altb,new Runnable(){
			public void run(){
				if(Main.status == 1){
					time = workTime;
				}
			}
		});
	```
# 注意
### 内存泄漏
1. 及时移除监听器
