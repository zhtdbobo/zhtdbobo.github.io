# java


[toc]
# jdk安装 

1. 下载jdk传到服务器并解压 

   ```shell
   mkdir /usr/local/java 
   tar -zxvf jdk-8-linux-x64.tar.gz -C /usr/local/java
   ```

2. 修改配置文件 vim /etc/profile 添加如下：

   - 方案一

       ```shell
       JAVA_HOME=/usr/local/java/jdk1.8.0_301
       CLASSPATH=$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar 
       PATH=$JAVA_HOME/bin:$PATH 
       export PATH JAVA_HOME CLASSPATHTH
       ```
   - 方案二

       ```shell
       export JAVA_HOME=/usr/local/java/jdk8
       export JRE_HOME=/usr/local/java/jdk8/jre
       export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASS_PATH
       export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin:$JAVA_HOME
       ```

3. 加载配置文件 

   ```shell
   source /etc/profile 
   ```

4. 验证并看看自己的配置是否都正确

   ```shell
   java -version
   javac
   echo $JAVA_HOME  
   echo $CLASSPATH  
   echo $PATH
   ```

# 使springboot项目支持https安全协议 

1. 在yml文件中配置https的端口以及jks证书(.jks文件)以及密码

   ```yaml
   server:
   port: 8082
   ssl: # ssl相关配置
   enabled: true
   key-store: classpath:文件名
   key-store-password: 密码
   key-store-type: JKS
   ```

2. 若要配置http自动跳转https，添加HttpConfig配置类如下，并在yml文件中添加http的端口 

   ```yaml
   http-port: 80 # http重定向https配置
   ```

3. 修改配置类

   ```java
   package com.lyb.travel.config;
   
   import org.apache.catalina.Context;
   import org.apache.catalina.connector.Connector;
   import org.apache.tomcat.util.descriptor.web.SecurityCollection;
   import org.apache.tomcat.util.descriptor.web.SecurityConstraint;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   /**
    * @author lyb
    * @create 2022/4/10-18:18
    * @method
    */
   @Configuration
   public class HttpsConfiguration {
   
       @Value("${http-port}")
       private int port;
   
       @Value("${server.port}")
       private int sslPort;
   
       @Bean
       public TomcatServletWebServerFactory tomcatServletWebServerFactory() {
           TomcatServletWebServerFactory factory = new TomcatServletWebServerFactory() {
               @Override
               protected void postProcessContext(Context context) {
                   SecurityConstraint securityConstraint = new SecurityConstraint();
                   securityConstraint.setUserConstraint("CONFIDENTIAL");
                   SecurityCollection securityCollection = new SecurityCollection();
                   securityCollection.addPattern("/*");
                   securityConstraint.addCollection(securityCollection);
                   context.addConstraint(securityConstraint);
               }
           };
           factory.addAdditionalTomcatConnectors(redirectConnector());
           return factory;
       }
   
       private Connector redirectConnector() {
           Connector connector = new Connector(TomcatServletWebServerFactory.DEFAULT_PROTOCOL);
           connector.setScheme("http");
           connector.setPort(port);
           connector.setSecure(false);
           connector.setRedirectPort(sslPort);
           return connector;
       }
   }
   
   ```

# springboot部署 

1. 在pom文件中引入打包插件，并且在插件中添加repackage功能（点击package之后打包成的jar之 后再将这个jar重新打包成可运行的jar，将原先的jar包改成.original为后缀的文件） 

   ```java
   <build>
	   <finalName>${project.name}</finalName>
	   <plugins>
		   <plugin>
			   <groupId>org.springframework.boot</groupId>
			   <artifactId>spring-boot-maven-plugin</artifactId>
			   <version>2.6.4</version>
			   <executions>
				   <execution>
					   <goals>
					   <goal>repackage</goal>
					   </goals>
				   </execution>
			   </executions>
		   </plugin>
	   </plugins>
   </build>
   ```

2. 在maven中进行clean，package打包成jar包 
3. 传输到linux服务器 
4. 将项目启动并在后台运行 nohup java -jar ai.jar &
