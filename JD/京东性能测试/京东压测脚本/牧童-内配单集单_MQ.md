```java
import com.jd.forcebot.engine.TestUtils;
import com.jd.forcebot.engine.groovy.Lifecycle;
import com.jd.forcebot.engine.groovy.RatePolicy;
import com.jd.forcebot.engine.groovy.TestCase;
import com.jd.forcebot.engine.groovy.TestSuite;
import com.jd.forcebot.toolkit.parameterized.latest.AsciiFileAccessCircularly;
import com.jd.jmq.client.connection.ClusterTransportManager;
import com.jd.jmq.client.connection.TransportConfig;
import com.jd.jmq.client.connection.TransportManager;
import com.jd.jmq.client.producer.MessageProducer;
import com.jd.jmq.common.message.Message;
import com.jd.jmq.common.network.command.Acknowledge;
import com.jd.jmq.client.producer.ProducerConfig;
import org.junit.Assert;
import org.slf4j.Logger;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import com.jd.forcebot.toolkit.parameterized.latest.AsciiFileAccessArbitrarily;
import com.jd.forcebot.toolkit.parameterized.latest.AsciiFileAccessCommonArbitrarily;
import java.io.*;

@TestSuite(lifecycle = Lifecycle.THREAD, ratePolicy = RatePolicy.STANDARD)
class ProducterMessageTest {

    private static final Logger logger = TestUtils.LOGGER;
    private static TransportManager manager;
    private static MessageProducer producer;
    
    //测试环境
    //private static String topic = "jkd_inner_collect_success";
    //private static String app = "jkdSystem";
    //private static String address = "test-nameserver.jmq.jd.local:50088";
    
    //预发环境
    private static String topic = "jkd_inner_collect_success_uat";
    private static String app = "jkdSystem";
    private static String address = "nameserver.jmq.jd.local:80";
    
    
    private static String messagebody = "";
    static AsciiFileAccessCommonArbitrarily atomicAccess = new  AsciiFileAccessCommonArbitrarily("jkd_inner_collect_success_data.txt");
    //static AsciiFileAccessCommonArbitrarily atomicAccess = new  AsciiFileAccessCommonArbitrarily("test.txt");

    static {
        try {
            logger.info(messagebody);
            //连接配置
            TransportConfig config = new TransportConfig();
            
            //测试环境
            //config.setApp("jkdSystem");
            //设置broker地址
            //config.setAddress("test-nameserver.jmq.jd.local:50088");
            //设置用户名
            //config.setUser("jkdSystem");
            //设置密码
            //config.setPassword("469dbc8b4d7242c38199af758f9a453c");
            
            
            //预发环境
            config.setApp("jkdSystem");
            //设置broker地址
            config.setAddress("nameserver.jmq.jd.local:80");
            //设置用户名
            config.setUser("jkdSystem");
            //设置密码
            config.setPassword("6cb6d9c318a3425bbb82652679c4b0dd");
            
            
            
            //设置发送超时
            config.setSendTimeout(10000);
            //设置是否使用epoll模式，windows环境下设置为false，linux环境下设置为true
            config.setEpoll(false);
            //创建集群连接管理器
            manager = new ClusterTransportManager(config);
            manager.start();
            //创建发送者
            producer = new MessageProducer(manager);
            ProducerConfig conf = new ProducerConfig();
            conf.setAcknowledge(Acknowledge.ACK_FLUSH);
            producer.setConfig(conf);
            producer.start();
        } catch (Exception e) {
            logger.error("连接mq出现问题："e.getMessage())
            e.printStackTrace();
        }
    }


    @TestCase
    void test() {
        //String maveTime = System.currentTimeMillis()-1000000;
        //logger.info("波次时间获取:{}",maveTime);

        //File f = new File(getClass().getClassLoader().getResource("jkd_inner_collect_success_data.txt").getFile());
        String path=getClass().getClassLoader().getResource("jkd_inner_collect_success_data.txt").getFile();
        //String path = getClass().getClassLoader().getResource("test.txt").getFile();
        logger.info("当前文件路径:{}", path);

        File sourceFile = new File(path);
        // 获取文件的内容的总行数
        logger.info("获取文件的内容的总行数:" + getTotalLines(sourceFile));
        logger.info("当前文件路径:" + sourceFile);

        /////
        //N个内配单号为一个批次
        int groupBegin = 1;
        int groupEnd = 50;
        while (true) {
            //readSelectedLine(groupBegin, groupEnd);
            //用来存一行文件内容的对象
            String line = null;
            //用来存很多行文件内容的对象
            StringBuilder text = new StringBuilder();
            //用来计数
            int tmpCount = 0;
            //开始行
            int begin = groupBegin;
            //结束行
            int end = groupEnd;
            try (BufferedReader sb = new BufferedReader(new FileReader(sourceFile))) {
                //这里使用sb.readLine()将一行文件的内容赋予line，注意：在出现sb.readLine()的时候不管你是用来做判断还是赋值，这行内容都已经从流消失。
                while ((line = sb.readLine()) != null) {
                    //++=+1
                    tmpCount++;
                    logger.info("当前行号:" + tmpCount);
                    //如果行数大于等于开始行并且小于等于结束行
                    if (tmpCount >= begin && tmpCount <= end) {
                        //用词对象将这行的内容存起来，并加上换行符
                        //text.append(line).append("\n");
                        if (tmpCount == begin && tmpCount == getTotalLines(sourceFile)) {
                            logger.info("只有一行数据");
                            text = new StringBuilder("\"").append(line).append("\"");
                        } else if (tmpCount == getTotalLines(sourceFile)) {
                            text = text.append("\"").append(",").append("\"").append(line).append("\"");
                        } else if (tmpCount == begin) {
                            text = new StringBuilder("\"").append(line);
                        } else if (tmpCount < end) {
                            text = text.append("\"").append(",").append("\"").append(line);

                        } else if (tmpCount == end) {
                            logger.info("当前批次中最后一行数据的行数" + tmpCount);
                            text = text.append("\"").append(",").append("\"").append(line).append("\"");
                        }
                    }
                }
                //关闭流
                sb.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            logger.info("当前批次的内配单号innerNoList:{}", text.toString());
            //组装报文
            try {
                //5批次
                //messagebody = "{\"batchNo\":\"1-1\",\"collectCode\":\"JKD20230403727443370056\",\"innerNoList\":[" + text.toString() + "],\"waveTime\":1680534000000}";
                //100批次
                String collectCode = System.currentTimeMillis() * 1000000;
                logger.info("当前批次号:{}","JKDJY"+collectCode);
                messagebody = "{\"batchNo\":\"1-1\",\"collectCode\":\"JKDJY"+collectCode+"\",\"innerNoList\":[" + text.toString() + "],\"waveTime\":1680534000000}";
                logger.info("当前集单结果报文:{}", messagebody);
                Message message = new Message(topic, "" + 1, "businessId" + 1);
                message.setBody(messagebody.getBytes());
                TestUtils.transactionBegin("createBill");
                producer.send(message);
                TestUtils.transactionSuccess("createBill");
                logger.info("JMQ执行成功！报文如下\n" + messagebody);
            } catch (Exception e) {
                logger.error("exception e={}, id={}", e, id);
                Assert.assertTrue(false)
                TestUtils.transactionFailure("createBill");
                logger.error("JMQ执行异常！");
            }
            groupBegin += 50;
            groupEnd += 50;
            logger.info("新的一组开始行:" + groupBegin);
            logger.info("新的一组结束行:" + groupEnd);
            if (groupBegin > getTotalLines(sourceFile)) {
                return;
            }
        }
    }

    
    //获取文件内容的总行数
    public static int getTotalLines(File file) throws IOException {
        FileReader in = new FileReader(file);
        LineNumberReader reader = new LineNumberReader(in);
        String s = reader.readLine();
        int lines = 0;
        while (s != null) {
            lines++;
            s = reader.readLine();
        }
        reader.close();
        in.close();
        return lines;
    }
    
}
```

