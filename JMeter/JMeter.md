# **Jmeter优缺点**

优点：

1. 开源、免费 2. 跨平台
2. 支持多协议 4. 小巧
3. 功能强大

缺点：

1. 不支持IP欺骗
2. 使用JMeter无法验证JS程序，也无法验证页面UI，所以要和Selenium配合来完成Web2.0应用的测试

# **安装和启动**

JMeter修改编码集

中文乱码问题，请先修改编码集。bin目录下jmeter.properties

```java
修改前
# The encoding to be used if none is provided (default ISO-8859-1)
#sampleresult.default.encoding=ISO-8859-1

修改后
# The encoding to be used if none is provided (default ISO-8859-1)
sampleresult.default.encoding=UTF-8
```

**启动并运行JMeter**

- 图形化启动

bin目录下，ApacheJMeter.jar

- windows下启动

bin目录下，jmeter.bat

- Linux下启动

bin目录下，jmeter.sh

# **JMeter基本使用**

设置接口三要素查询所有学院信息：

1、测试计划--右键--线程--添加线程组

2、线程组--右键--取样器--http请求

3、测试计划--右键--添加监听器--查看结果树

4、点击运行，查看结果

![image-20230813153852040](./JMeter.assets/image-20230813153852040-1695890702780-4.png)

![image-20230813153859147](./JMeter.assets/image-20230813153859147-1695890730069-6.png)

![image-20230813153905994](./JMeter.assets/image-20230813153905994-1695890754482-8.png)

# **JMeter线程组相关**

进 程: 正在运行的程序

线 程: 是进程中的执行线索

线程组: 进程中有许多线程，为了方便管理，可以对线程按照性质分组，分组的结果就是线程组

PS: 三者关系，一个进程可以包含多个线程组，一个线程组可以包含多个线程

# **JMeter并发执行和顺序执行**

并发执行: 多个线程同时执行

顺序执行: 多个线程顺序执行

勾选测试计划中的独立运行每个线程组，并发改为顺序执行

![image-20230813153931918](./JMeter.assets/image-20230813153931918-1695890784207-10.png)

# **JMeter两个特殊线程组**

setUp线程组：最优先执行的线程组

tearDown线程组：最后执行的线程组

# **JMeter线程组常用属性**

![image-20230813154009403](./JMeter.assets/image-20230813154009403-1695890811127-12.png)

# HTTP请求默认值

http请求默认值：被复用的内容的封装

![image-20230813154031272](./JMeter.assets/image-20230813154031272-1695890845519-14.png)

# **信息头管理器**

新增修改实现时提交的数据是 JSON 格式的，需声明提交的数据的内容类型：

![image-20230813154044512](./JMeter.assets/image-20230813154044512-1695890868890-16.png)

![image-20230813154051501](./JMeter.assets/image-20230813154051501-1695890883498-18.png)

# **参数化**

当提交的数据量较大，怎么提交?每测试一次就修改一次吗?

定义：动态的获取、设置或生成数据，是一种由程序驱动代替人工驱动的数据设计方案，提高脚本的编写效率以及编写质量

以下四种方式实现参数化：

1. **用户定义的变量**

   ![image-20230813154124014](./JMeter.assets/image-20230813154124014-1695890910003-20.png)

   ![image-20230813154133539](./JMeter.assets/image-20230813154133539-1695891020547-22.png)

   ![image-20230813154141659](./JMeter.assets/image-20230813154141659-1695891051996-24.png)

2. **CSV 数据文件设置**

   CSV ：逗号分隔值，是一种简洁且常见的数据存储格式，存储语法如下图所示

   ![image-20230813154215841](./JMeter.assets/image-20230813154215841-1695891084129-26.png)

   实现步骤:

   1、使用 CSV 文件存储测试数据

   2、编写脚本模板

   注意: 编码集使用 UTF-8 无 BOM 格式

   ![image-20230813154243364](./JMeter.assets/image-20230813154243364-1695891122175-28.png)

   3、关联脚本与数据(将文件数据导入脚本)

   ![image-20230813154309741](./JMeter.assets/image-20230813154309741-1695891163515-30.png)

   不确定有几条数据，勾选永远；确定几条数据，可以填写具体次数

   ![image-20230813154326987](./JMeter.assets/image-20230813154326987-1695891192074-32.png)

   CSV数据文件设置属性

   ![image-20230813154347449](./JMeter.assets/image-20230813154347449-1695891220924-34.png)

3. **用户参数**

   ![image-20230813154404783](./JMeter.assets/image-20230813154404783-1695891270163-36.png)

   实现步骤:

   1、编写脚本模板

   2、使用用户参数存储测试数据

   ![image-20230813154417077](./JMeter.assets/image-20230813154417077-1695891298465-38.png)

   3、将数据导入脚本模板

   4、设置执行次数

   线程组中线程数设置为2，通过线程数而非循环次数控制脚本执行次数

   ![image-20230813154432564](./JMeter.assets/image-20230813154432564-1695891486623-40.png)

4. **函数**

   常见函数：

   __counter 计数器函数 TRUE(每个用户都有自己的计数器) FALSE(所有用户共用一个计数器)

   __Random 随机数函数 参数1：取值范围最小值(包含) 参数2：取值范围最大值(包含)

   __time 获取当前时间的函数 无参: 获取的是距离 1970/01/01 00:00:00 的毫秒值

   参数1: yyyyMM_dd HH:mm:ss 格式化成 年\月_日 时:分:秒 格式

   示例1：当设置2个用户，每个用户执行3次。在结果里想查看每个用户执行的具体次数，也就是每个人有独立的计数器

   ![image-20230813154503118](./JMeter.assets/image-20230813154503118-1695891579888-42.png)

   打开函数助手->选择函数->为函数传参->生成并复制调用函数格式->函数运行结果预览

   ![image-20230813154516431](./JMeter.assets/image-20230813154516431-1695891608353-44.png)

   请求处粘贴调用函数格式

   ![image-20230813154527847](./JMeter.assets/image-20230813154527847-1695891638429-46.png)

   运行

   ![image-20230813154540005](./JMeter.assets/image-20230813154540005-1695891663893-48.png)

   示例二：看总共执行多少次，也就是所有人共用一个计数器

   ![image-20230813154552293](./JMeter.assets/image-20230813154552293-1695891691018-50.png)

   示例三：添加时间

   ![image-20230813154605297](./JMeter.assets/image-20230813154605297-1695891742434-52.png)

   

# **直连数据库**

通过直连数据库让程序代替接口访问数据库，如果二者预期结果不一致，就找到了程序缺陷。

1、Jmeter 不具备直连数据库功能，必须整合第三方( jar包)实现，配置数据库的连接

- jmeter要连接mysql数据库，首先得下载mysql jdbc驱动包，尽量保证其版本和你的数据库版本一致，至少不低于数据库版本，否则可能有问题。官网下载地址为：https://dev.mysql.com/downloads/connector/j/

  ![image-20230813154723892](./JMeter.assets/image-20230813154723892-1695891787965-54.png)

- 解压后把jdbc驱动jar包引入测试计划，点击测试计划-->点击浏览-->选中mysql驱动jar包-->打开，如下图所示：

  ![image-20230813154750331](./JMeter.assets/image-20230813154750331-1695891813732-56.png)

- 配置JDBC Connection Configuration

  在线程组下新建一个JDBC Connection Configuration配置元件，需要用到这个元件来完成数据库的连接，详细配置如下图所示：

  ![image-20230813154818552](./JMeter.assets/image-20230813154818552-1695891858161-58.png)

  2、通过JDBC Request请求向数据库发送 SQL语句并接收提取响应结果

  ![image-20230813154845535](./JMeter.assets/image-20230813154845535-1695892457523-60.png)

![image-20230813154855648](./JMeter.assets/image-20230813154855648-1695892472583-62.png)

Query： 填写要被执行的SQL语句

Parameter values：参数值

Parameter types：参数类型

Variable names：保存sql语句返回结果的变量名

Result variable name：创建一个对象变量，保存所有返回的结果

Query timeout：查询超时时间

Handle result set：定义如何处理由callable statements语句返回的结果

![image-20230813154910872](./JMeter.assets/image-20230813154910872-1695892493734-64.png)

3、结果获取规则可以通过 Debug Sampler 组件查看

![image-20230813154929775](./JMeter.assets/image-20230813154929775-1695892524664-66.png)

通过JDBC Request获取到的结果以val为前缀进行数据的保存

![image-20230813154943218](./JMeter.assets/image-20230813154943218-1695892543427-68.png)

![image-20230813154952732](./JMeter.assets/image-20230813154952732-1695892556580-70.png)

4、将提取到的响应结果，在其他请求中使用

![image-20230813155007289](./JMeter.assets/image-20230813155007289-1695892581400-72.png)

# **断言**

断言：让程序代替人工判断响应结果是否符合预期

步骤：

1、按照之前的实现编写测试脚本

2、为被判断的取样器添加断言组件

3、直接运行查看结果。断言通过: 无提示，断言失败: 给出错误

# **逻辑控制器**

通过参数化可以实现单个接口的功能测试，而接口测试过程中，除了单个接口的功能测试之外，还会测试接口业务实现，所谓业务，就是一套完整的业务逻辑或流程，这就必须要使用到逻辑控制和关联。

需求1:测试计划中定义一个 http 请求，但是该请求不是无条件执行的，声明一个用户定义的变量，如果变量是 itcast 才执行，否则就不执行

1. 搭框架，测试计划，线程组，结果树，声明一个用户定义的变量

   ![image-20230813155039971](./JMeter.assets/image-20230813155039971-1695892610320-74.png)

2. 核心:添加 if 控制器，子级添加取样器 (和之前实现不同，控制器和取样器存在父子级关系)

   ![image-20230813155104135](./JMeter.assets/image-20230813155104135-1695892639821-76.png)

需求2:有一组关键字 [广州分公司,上海分公司,成都分公司] (使用用户定义的变量存储)要依次取出，并在系统搜索

1. 搭框架，测试计划，线程组，结果树，声明一个用户定义的变量,存储一组数据

   ![image-20230813155133480](./JMeter.assets/image-20230813155133480-1695892711594-78.png)

2. 添加 forEach 控制器，子级添加取样器 (和之前实现不同，控制器和取样器存在父子级关系)

   ![image-20230813155147738](./JMeter.assets/image-20230813155147738-1695892732889-80.png)

3. 系统搜索关键字

   ![image-20230813155200214](./JMeter.assets/image-20230813155200214-1695892754870-82.png)

需求3:循环访问系统10次

实现:

1. 搭框架，测试计划，线程组，结果树

2. 添加循环控制器，子级添加取样器 (和之前实现不同，控制器和取样器存在父子级关系)

   HTTP请求1执行3次，HTTP请求2执行5次

![image-20230813155236992](./JMeter.assets/image-20230813155236992-1695892796216-84.png)

# **关联**

关联: 上一个请求的响应结果和下一个请求的数据有关系

需求:两个http请求，请求A访问传智播客官网，请求B访问百度 ，请求A将传智播客官网源码中的 title 标签的值取出，传递给请求B，在请求B中作为关键字搜索这个 title 值

步骤：

1. 搭框架,编写两个请求，传智播客 + 百度搜索

2. 核心: 取出传智播客页面源码的 title 值

   ![image-20230813155308687](./JMeter.assets/image-20230813155308687-1695892823660-86.png)

   ![image-20230813155317626](./JMeter.assets/image-20230813155317626-1695892840187-88.png)

3. 传递给百度：${变量名} 的方式传值

   ![image-20230813155337362](./JMeter.assets/image-20230813155337362-1695892863786-90.png)

   运行结果

   ![image-20230813155347736](./JMeter.assets/image-20230813155347736-1695892899111-92.png)

![image-20230813155355637](./JMeter.assets/image-20230813155355637-1695892934031-94.png)

# **关联-正则表达式提取器**

需求:两个http请求，请求A查询一级分类id，请求B根据一级分类查询详细信息 

步骤：

1. 搭框架,编写两个请求

2. 核心: 取出查询一级分类id值

   ![image-20230813155425289](./JMeter.assets/image-20230813155425289-1695892960399-96.png)

   ![image-20230813155431753](./JMeter.assets/image-20230813155431753-1695893131431-98.png)

3. 传递给请求B：${变量名} 的方式传值

   ![image-20230813155446033](./JMeter.assets/image-20230813155446033-1695893153351-100.png)

# **跨线程组关联**

变量作用域局限于当前线程组，其他线程组不可以直接调用。可以将请求A中提取的结果导出到公共空间(可以被不同线程组共享),请求B再从公开空间调用该变量，相当于全局变量。

步骤：

注意：测试计划中先勾选独立运行每个线程组

![image-20230813155502471](./JMeter.assets/image-20230813155502471-1695893173130-102.png)

1. 将请求A的数据导出到公共空间( __setProperty)

   函数助手，选择setProperty，设置值后，点击生成

   ![image-20230813155529375](./JMeter.assets/image-20230813155529375-1695893219890-104.png)

2. 把代码放在beanshell取样器中

   ![image-20230813155544206](./JMeter.assets/image-20230813155544206-1695893258620-106.png)

   ![image-20230813155550055](./JMeter.assets/image-20230813155550055-1695893276604-108.png)

3. 请求B从公共空间调用数据 (__property)

   ![image-20230813155603071](./JMeter.assets/image-20230813155603071-1695893301388-110.png)

   ![image-20230813155608088](./JMeter.assets/image-20230813155608088-1695893317730-112.png)

# **性能测试**

模拟各种正常的、峰值的测试环境，检测程序的各项性能指标是否能够达标

# **高并发**

JMeter 中内置了 定时器，可以实现时间模式相关的性能测试

需求1:同一时刻 100 个用户查询一级分类功能,统计高并发情况下平均响应时间以及错误率(高并发)

1. 搭框架,测试计划，线程组，取样器，结果树(局限性),指定线程组的线程数属性值为 100

   ![image-20230813155632488](./JMeter.assets/image-20230813155632488-1695893350349-114.png)

   ![image-20230813155645422](./JMeter.assets/image-20230813155645422-1695893365570-116.png)

2. 添加定时器 synchronizing timer(集合点组件)

   ![image-20230813155658696](./JMeter.assets/image-20230813155658696-1695893389435-118.png)

   同步定时器

   ![image-20230813155717949](./JMeter.assets/image-20230813155717949-1695893413726-120.png)

3. 运行并查看结果查看：聚合报告组件,可以对结果汇总分析

   ![image-20230813155731870](./JMeter.assets/image-20230813155731870-1695893433909-122.png)

# **高频率**

需求2:一个用户以 20QPS ( == 20 次/s) 的频率访问XXX系统服务器，持续15秒，统计服务器的平均响应时间

QPS: Query per Seconds 每秒查询数(查询率),每秒访问多少次服务器

1. 搭框架，测试计划，线程组，取样器，聚合报告,根据题干计算数据:

   循环次数 = 访问频率 * 持续时间

   ![image-20230813155801238](./JMeter.assets/image-20230813155801238-1695893462977-124.png)

   填写循环次数

   ![image-20230813155811088](./JMeter.assets/image-20230813155811088-1695893490087-126.png)

2. 添加QPS访问频率控制的相关组件：

   每分钟访问次数 = 访问频率 * 60

   ![image-20230813155833596](./JMeter.assets/image-20230813155833596-1695893511780-128.png)

# **生成图形化测试报告**

在 JMeter 中可以以图形化（饼状图、柱状图...）的方式显示脚本运行结果，较之于聚合报告或查看结果

树组件实

现更直观，用户体验更友好

生成图形化测试报告

命令: jmeter -n -t 脚本文件 -l 日志文件 -e -o 目录

-n 无图形化运行

-t 被运行的脚本

-l 将运行信息写入日志文件，生成.jtl格式的测试报告

-e 生成测试报告

-o 指定报告输出目录

![image-20230813155852054](./JMeter.assets/image-20230813155852054-1695893529904-130.png)

# **分布式**

多台机协作，以集群的方式完成测试任务，可以提高测试效率。

![image-20230813155907859](./JMeter.assets/image-20230813155907859-1695893548069-132.png)

环境搭建：

1、不同的测试机上安装 Jmeter

2、配置基础环境(统一操作系统、JDK、Jmeter .... )

3、核心: 控制机如何与执行机通信? 关键点：端口号

4、控制机中设置执行机的 IP

%JMETER_HOME%/bin/jmeter.properties ----> remote_hosts=执行机A的IP:端口号, 执行机B的IP:端口号, .....

打开JMETER安装目录，\bin\jmeter.properties修改配置文件

![image-20230813155922686](./JMeter.assets/image-20230813155922686-1695893567362-134.png)

5、控制机和执行机都得设置远程访问相关属性：

server.rmi.ssl.disable=true

![image-20230813155935179](./JMeter.assets/image-20230813155935179-1695893586543-136.png)

6、执行机Jmeter保持启动状态下，控制机远程启动所有

![image-20230813155945800](./JMeter.assets/image-20230813155945800-1695893604291-138.png)

# **Ant与jmeter集成**

1. Ant环境变量配置

2. jmeter目录下的扩展文件夹\extras\ant-jmeter-1.1.1.jar复制到\ant\lib目录下

3. 全局配置

   - Jmeter配置文件

     .jtl格式基于xml文件格式，所以需要修改bin目录下jmeter.properties

     ```java
     修改前
     #jmeter.save.saveservice.output_format=csv
     
     修改后
     jmeter.save.saveservice.output_format=xml
     ```

   - jmeter是否保存响应数据，不是必须

     ```java
     修改前
     #jmeter.save.saveservice.response_data=false
     
     修改后
     jmeter.save.saveservice.response_data=true
     ```

   - jmeter是否保存采样器数据，不是必须

     ```java
     修改前
     #jmeter.save.saveservice.samplerData=false
     
     修改后
     jmeter.save.saveservice.samplerData=true
     ```

   - 配置build.xml文件（指导ant命令如何去执行jmx测试用例以及如何生成html报告）

   文件拷贝到脚本目录，执行ant即可

   ```html
   <?xml version="1.0" encoding="UTF-8"?>
    
   <project name="ant-jmeter-test" default="run" basedir=".">
       <tstamp>
           <format property="time" pattern="yyyyMMddhhmm" />
       </tstamp>
     
       <target name="run">
              <antcall target="clear1" />
       <antcall target="test1" />
               <antcall target="report1" />
       <antcall target="sendmail1" />
       </target>
       
       <target name="test1">
             <echo message="开始执行jmeter脚本..."></echo>
           <taskdef name="jmeter" classname="org.programmerplanet.ant.taskdefs.jmeter.JMeterTask" />
           <jmeter jmeterhome="D:\DeveloperConfig\apache-jmeter-5.5" resultlog="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\copy\result\ant_jmeter_test_report.jtl">
                <!-- 声明要运行的脚本。"*.jmx"指包含此目录下的所有jmeter脚本-->
               <testplans dir="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\script" includes="*.jmx" />
           <property name="jmeter.save.saveservice.output_format" value="xml"/>
           </jmeter>
       </target>
           
            <path id="xslt.classpath">
           <fileset dir="D:\DeveloperConfig\apache-jmeter-5.5\lib" includes="xalan*.jar"/>
           <fileset dir="D:\DeveloperConfig\apache-jmeter-5.5\lib" includes="serializer*.jar"/>
       </path>
       
       <target name="report1">
       <echo message="开始转换执行结果..."></echo>
       <tstamp> <format property="time" pattern="yyyy/MM/dd HH:mm" /></tstamp>
           <xslt classpathref="xslt.classpath"
                 force="true"
        in="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\copy\result\ant_jmeter_test_report.jtl"
                 out="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\copy\html\ant_jmeter_test_report.html"
                 style="D:\DeveloperConfig\apache-jmeter-5.5\extras\jmeter-results-detail-report_21.xsl">
                   <param name="titleReport" expression="测试报告"/> 
                   <param name="dateReport" expression="${time}"/>  
          </xslt>  
                       <!-- jmeter-results-detail-report_21  因为上面生成报告的时候，不会将相关的图片也一起拷贝至目标目录，所以，需要手动拷贝--> 
           <copy todir="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\copy\html">
               <fileset dir="D:\DeveloperConfig\apache-jmeter-5.5\extras">
                   <include name="collapse.png" />
                   <include name="expand.png" />
               </fileset>
           </copy>            
       </target>
    
           <target name="clear1">
       <!-- 每次执行前先删除清空jtl和html文件夹，释放空间 -->
           <echo message="释放空间,清空jtl和html文件夹中 ..."></echo>
           <delete dir="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\copy\result" />
           <delete dir="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\copy\html" />
           <!-- 删除单个文件-->
           <delete file="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\copy\ant_jmeter_test_report.rar" />
           <sleep seconds="2"/>  
           <echo message="释放完成 ..."></echo>
       </target>
    
    
       <target name="sendmail1">
       <!--把报告进行压缩打包-->
       <jar jarfile="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\copy\ant_jmeter_test_report.rar" basedir="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\copy\html"/>
       <!--把文件贴到邮件正文-->
       <loadfile property="html" srcFile="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\copy\html\ant_jmeter_test_report.html" encoding="UTF-8">
       <filterchain>
               <expandproperties />
           </filterchain>
       </loadfile>
       
       <mail mailhost="smtp.qq.com" 
       mailport="25" subject="邮件标题" 
       messagemimetype="text/html" tolist="收件邮箱1,收件邮箱2,收件邮箱3"
       user="发件邮箱" password="授权码(可在邮箱设置中找到)">
       <from address="发件邮箱"/>
       <!--发送附件-->
       <fileset dir="D:\DeveloperConfig\apache-jmeter-5.5\TestSuite\copy">
               <include name="ant_jmeter_test_report.rar"/>
          </fileset>
       <message><![CDATA[ 
       <p>项目组收：
               这是正文最前面(可任意填写邮件正文)
       </p>
       <pre> ${html} </pre>
       <p>"这是正文最后面(可任意填写邮件正文)"</p>
       ]]></message>
       </mail>
       </target>
   </project>
   ```

   build.xml可进一步优化项如下：

   优化一：报告名称、压缩包名称变更

   ant_jmeter_test_report(尾缀有.jtl,.html,.rar)：报告名称、压缩包名称。(存在于第18行、第35行、第36行、第56行、第64行、第66行、第79行)

   PS:如果需要更改ant_jmeter_test_report名称时，尽量全部都同步更新，使用替换操作即可方便更换。

   优化二：编写邮件标题

   第73行：subject="邮件标题"

   优化三：添加多个收件邮箱

   第75行：tolist="设置多个收件邮箱，使用英文逗号隔开，注意只能在最外层有双引号"

   例如：tolist="123@qq.com,456@qq.com,789@qq.com"

   优化四：编写邮件正文内容

   第82-86行：可以任意编写邮件正文内容。