# JAVA 环境变量配置

```
CLASSPATH
.;%java_home%\lib\dt.jar;%java_home%\lib\tools.jar

JAVA_HOME
D:\Developers\jdk-11

Path
%java_home%\bin;%java_home%\jre\bin;
```



# ADB 环境变量配置

```jade
Path
%ANDROID_HOME%\tools;%ANDROID_HOME%\platform-tools;

ANDROID_HOME
D:\Developers\Android\Sdk
```



# Cmder 环境变量配置

```
CMDER_HOME
D:\Developers\Cmder

Path
%CMDER_HOME%
```

***添加 cmder 到右键菜单***

配置环境变量后，在**管理员权限**的终端输入以下语句

```
Cmder.exe /REGISTER ALL
```



# Apache Maven 环境变量配置

```
MAVEN_HOME
D:\Developers\Apache Maven\apache-maven-3.9.3

Path
%MAVEN_HOME%\bin
```



# Apache JMeter 环境变量配置

```
JMETER_HOME
D:\Developers\Apache JMeter\apache-jmeter-5.6.2

CLASSPATH
%JMETER_HOME%\lib\ext\ApacheJMeter_core.jar;%JMETER_HOME%\lib\jorphan.jar;

Path
%JMETER_HOME%\bin
```



# Apache Ant 环境变量配置

```
ANT_HOME
D:\Developers\Apache Ant\apache-ant-1.10.13

Path
%ANT_HOME%\bin;%ANT_HOME%\lib;
```

ant -version可查看是否配置成功。



# Apache Tomcat 环境变量配置

```
CATALINA_HOME
D:\Developers\Apache Tomcat\apache-tomcat-8.5.91

CATALINA_BASE
D:\Developers\Apache Tomcat\apache-tomcat-8.5.91

Path
%CATALINA_HOME%\lib;%CATALINA_HOME%\bin;
```

***Tomcat启动***

运行startup.bat文件。

***Tomcat关闭***

运行shutdown.bat文件或者直接关闭掉启动窗口。

***访问Tomcat***

访问Tomcat的URL格式：[http://ip](https://link.zhihu.com/?target=http%3A//ip):port

访问本机Tomcat的URL格式：[http://localhost:8080](https://link.zhihu.com/?target=http%3A//localhost%3A8080/)

***解决控制台乱码***

控制台产生乱码的原因是在Tomcat在输出日志中使用的是UTF-8编码，而我们中文的Windows操作系统使用的是GBK编码。由于编码格式不统一，所以出现了乱码。

解决方式：修改conf目录中的logging.properties文件重新指定的编码方式。如果还是不行,那么就删除该行即可

```
java.util.logging.ConsoleHandler.encoding = GBK
```

***修改Tomcat监听端口***

Tomcat默认监听端口为8080。可通过修改server.xml文件来改变Tomcat监听端口。

```
<Connector port="8080" protocol="HTTP/1.1"
connectionTimeout="20000" redirectPort="8443" />
```

***配置Tomcat并发数***

Tomcat的最大并发数是可以配置的，实际运用中，最大并发数与硬件性能和CPU数量都有很大关系的。更好的硬件，更多的处理器都会使Tomcat支持更多的并发。
这个并发能力还与应用的逻辑密切相关，如果逻辑很复杂需要大量的计算，那并发能力势必会下降。如果每个请求都含有很多的数据库操作，那么对于数据库的性能也是非常高的。
对于单台数据库服务器来说，允许客户端的连接数量是有限制的。并发能力问题涉及整个系统架构和业务逻辑、系统环境不同、Tomcat版本不同、JDK版本不同、以及修改的设定参数不同。并发量的差异还是满大的。并发数设置参数有如下几个

maxThreads="1000"

最大并发数

minSpareThreads="100"

初始化时创建的线程数

maxSpareThreads="500"

一旦创建的线程超过这个值，Tomcat就会关闭不再需要的socket线程。

acceptCount="700"

指定当所有可以使用的处理请求的线程数都被使用时，可以放到处理队列中的请求数，超过这个数的请求将不予处理

配置实例：

```
<Connector port="8080"
protocol="HTTP/1.1" minSpareThreads="100" maxSpareThreads="500" maxThreads="1000"
acceptCount="700" connectionTimeout="20000" redirectPort="8443" />
```



# allure 环境变量配置

```
Path
D:\Developers\allure\allure-2.23.0\bin;
```

配置完成后，可以通过下面命令进行检查

```
where allure
```

**allure 官网**

[**http://allure.qatools.ru/**](http://allure.qatools.ru/)