```java
import com.jd.forcebot.toolkit.parameterized.latest.AsciiFileAccessArbitrarily;
import com.jd.forcebot.toolkit.parameterized.latest.AsciiFileAccessCircularly;
import com.jd.forcebot.toolkit.parameterized.latest.AsciiFileAccessCommonArbitrarily
import com.jd.forcebot.engine.groovy.Lifecycle;
import com.jd.forcebot.engine.groovy.RatePolicy;
import com.jd.forcebot.engine.groovy.TestCase;
import com.jd.forcebot.engine.groovy.TestSuite;
import java.util.concurrent.TimeUnit;
import java.net.URLEncoder;
import com.jd.forcebot.engine.TestUtils;
import com.jd.forcebot.engine.groovy.*;
import com.jd.forcebot.toolkit.*;
import okhttp3.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;


/**
 * @author jiangyu9
 *
 * @TestSuite - 类注解
 *   value - 事物父名称》String
 *   Lifecycle - 生命周期》Lifecycle.PROCESS, Lifecycle.THREAD, Lifecycle.SUITE, Lifecycle.CASE
 *   ratePolicy - 比例执行策略》RatePolicy.STANDARD, RatePolicy.PROBABILITY
 *
 * @TestCase - 方法注解
 *   value - 事物详细名称》String
 *   rate - 方法运行比例权重》int
 *   timeout - 方法运行超时时间》long default 60000L
 *   record - 是否统计该方法》boolean
 *
 * 自定义事务：
 * TestUtils.transactionBegin("xxx")
 * TestUtils.transactionSuccess("xxx")
 * TestUtils.transactionFailure("xxx")
 */
@TestSuite(value = "forcebot")
class Test {

  /**
  * 实例化顺序读取文本文件，构造方法参数：
  * filepath:文件存储路径，可以指定classpath相对路径，也可以指定文件所在文件系统的绝对路径
  * separator:文件分隔符，用于对行数据进行分列读取使用，默认值为NULL，不对行数据进行分列读取
  * enCycle:启用循环读取文件模式，默认为true，当为true时，文件读取到末行数据时，下次调用读取方法时会将文件指针指向文件起始位置，当为false时，调用读取方法至文件末时，会返回NULL
  */
  // static AsciiFileAccessCircularly accessCircularly = new AsciiFileAccessCircularly("ip.txt",",",true);

  /**
  * 实例化随机读取文本文件，构造方法参数：
  * filepath:文件存储路径，可以指定classpath相对路径，也可以指定文件所在文件系统的绝对路径
  * separator:文件分隔符，用于对行数据进行分列读取使用，默认值为NULL，不对行数据进行分列读取
  */
  //static AsciiFileAccessArbitrarily arbitrarily = new AsciiFileAccessArbitrarily("ip.txt");

  public final Logger logger = TestUtils.LOGGER;
  public final OkHttpClient client;
  public final Request.Builder builder;


  Test() {
    client = new OkHttpClient().newBuilder()
          .connectTimeout(6, TimeUnit.SECONDS)
          .readTimeout(6, TimeUnit.SECONDS)
          .writeTimeout(6, TimeUnit.SECONDS)
          .followRedirects(false)
          .build();
    builder = new Request.Builder();
    logger.info("TestRunner init...");
  }

    //record默认为true，表示该函数会作为一个事务记录, 时间单位为毫秒
  @TestCase(record = false, timeout = 0L)
  void test() {
    // String oneline = accessCircularly.readLine(); //顺序读取一行数据，并将文件指针移向下一行行首
    // String[] onelineSplit = accessCircularly.readToArray(); //顺序读取文件中某一行数据，并根据指定的分隔符进行拆分到数组中
    // String oneline = arbitrarily.readLine(); //随机读取一行数据
    // String[] onelineSplit = arbitrarily.readToArray(); //随机读取文件中某一行数据，并根据指定的分隔符进行拆分到数组中
    //set request body
    //MediaType mediaType = MediaType.parse("application/xml");
    
    
    
    int barType=(int) (Math.random() * (5 - 1 + 1)) + 1;
    logger.info("当前barType类型:{}",barType);
    long barCodeTemp = System.currentTimeMillis() * 10000;
    long boxIdTemp = System.currentTimeMillis() / 1000;
    long outSideNoTemp = System.currentTimeMillis() / 100;
    //long barCode;
    String barCode = "jyTest";
    long boxId;
    long outSideNo;
    if(barType == 1){
        barCode = barCode + barCodeTemp - 1000;
        boxId = boxIdTemp + 1000;
        outSideNo = outSideNoTemp -1000;
    }else if (barType == 2){
        barCode = barCode + barCodeTemp - 2000;
        boxId = boxIdTemp + 2000;
        outSideNo = outSideNoTemp -2000;
    }else if (barType == 3){
        barCode = barCode + barCodeTemp - 3000;
        boxId = boxIdTemp + 3000;
        outSideNo = outSideNoTemp -3000;
    }else if (barType == 4){
        barCode = barCode + barCodeTemp - 4000;
        boxId = boxIdTemp + 4000;
        outSideNo = outSideNoTemp -4000;
    }else if (barType == 5){
        barCode = barCode + barCodeTemp - 5000;
        boxId = boxIdTemp + 5000;
        outSideNo = outSideNoTemp -5000;
    }
    
    
    
    
    //long barCode = System.currentTimeMillis() * 10000;
    logger.info("当前箱报文barCode值: {}", barCode);
    //long boxId = System.currentTimeMillis() / 1000;
    logger.info("当前箱报文boxId值: {}", boxId);
    //String outsideno = new StringBuilder("VW").append(String.valueOf(System.currentTimeMillis() / 100)).toString();
    //long outSideNo = System.currentTimeMillis() / 100;
    logger.info("当前箱报文outSideNo值: {}", "VW"+outSideNo);
    
    //to对应wms_distribution,toSid对应mini_warehouse_no,from对应wms_org_no,startSid对应wms_warehouse_no
    //保温中替换成存在的sku，替换Wid
    //ib_main表
    
    String requestmsg="<Envelope xmlns=\"http://schemas.xmlsoap.org/soap/envelope/\">\n" +
                "    <Body>\n" +
                "        <queryWs xmlns=\"http://wms3.360buy.com\">\n" +
                "            <arg0 xmlns=\"\">{\"bizType\":\"receivedIbMain\",\"callCode\":\"JD.MINIWMS.WS.CALLCODE\",\"uuid\":\"1\"}</arg0>\n" +
                "            <arg1 xmlns=\"\"><![CDATA[<?xml version=\"1.0\" encoding=\"utf-16\"?>\n" +
                "<BoxDetail xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns:xsd=\"http://www.w3.org/2001/XMLSchema\">\n" +
                "  <taskId>a311b473-af3a-4778-919e-a49a6b30438e</taskId>\n" +
                "  <from>6</from>\n" +
                "  <to>364</to>\n" +
                "  <operatorName>IPC</operatorName>\n" +
                "  <barcode>"+barCode+"</barcode>\n" +
                "  <boxid>"+boxId+"</boxid>\n" +
                "  <storeProperty>3</storeProperty>\n" +
                "  <startSid>9500</startSid>\n" +
                "  <toSid>2020253</toSid>\n" +
                "  <outsideno>"+"VW"+outSideNo+"</outsideno>\n" +
                "  <ywtype>G4</ywtype>\n" +
                "  <subType>4</subType>\n" +
                "  <transportType>BB</transportType>\n" +
                "  <qingdanlist>\n" +
                "    <Boxware>\n" +
                "      <id>1264388188</id>\n" +
                "      <type>1</type>\n" +
                "      <bilv>1</bilv>\n" +
                "      <Wid>100000149746</Wid>\n" +
                "      <WName>正大（CP）微辣鸭板肠200g  卤味鸭肉 休闲零食 化冻即食</WName>\n" +
                "      <Wnum>2</Wnum>\n" +
                "      <boxid>"+boxId+"</boxid>\n" +
                "      <OperatorID>IPC</OperatorID>\n" +
                "      <TimeState>2022-04-21 07:04:43.0 UTC</TimeState>\n" +
                "      <State>2</State>\n" +
                "      <rfid>0</rfid>\n" +
                "      <OldState>0</OldState>\n" +
                "      <NewState>0</NewState>\n" +
                "      <barcode>"+barCode+"</barcode>\n" +
                "    </Boxware>\n" +
                "    <Boxware>\n" +
                "      <id>1264388181</id>\n" +
                "      <type>1</type>\n" +
                "      <bilv>1</bilv>\n" +
                "      <Wid>100000304093</Wid>\n" +
                "      <WName>上鲜 白羽鸡 鸡小胸 1.5kg/袋 冷冻 圈养 出口日本级 健身鸡胸肉鸡里脊鸡肉 健身餐轻食代餐鸡胸肉 清真食品</WName>\n" +
                "      <Wnum>3</Wnum>\n" +
                "      <boxid>"+boxId+"</boxid>\n" +
                "      <OperatorID>IPC</OperatorID>\n" +
                "      <TimeState>2022-04-21 07:04:43.0 UTC</TimeState>\n" +
                "      <State>2</State>\n" +
                "      <rfid>0</rfid>\n" +
                "      <OldState>0</OldState>\n" +
                "      <NewState>0</NewState>\n" +
                "      <barcode>"+barCode+"</barcode>\n" +
                "    </Boxware>\n" +
                "  </qingdanlist>\n" +
                "  <RequestContext>\n" +
                "    <bu>301</bu>\n" +
                "    <language>zh</language>\n" +
                "    <timeZone>GMT+8</timeZone>\n" +
                "  </RequestContext>\n" +
                "</BoxDetail>\n" +
                "]]>\n" +
                "</arg1>\n" +
                "</queryWs>\n" +
                "</Body>\n" +
                "</Envelope>";
    
    logger.info("当前参数化初始化报文IbMainInit：{}",requestmsg);
    
    RequestBody body = RequestBody.create(MediaType.parse("application/xml"), requestmsg);
    Response response = null;
    //Request request = builder.url("http://11.158.11.164/services/simpleWebServiceSoap")
    Request request = builder.url("http://11.147.33.81/services/simpleWebServiceSoap")
          .header("pfinder", "EAAAAAAAAAAAAAAAAAABMQ==")
        .header("Content-Type", "application/xml")
        .header("Connection", "keep-alive")
          .method("POST", body)
          .build();
    TestUtils.transactionBegin("test_tran");
    try {
        response = client.newCall(request).execute();
        String responseBodyStr = response.body().string();
        if(response.code() == 200){ //TODO 请根据业务需求添加判断！(如：responseBodyStr.contains("success"))
            TestUtils.transactionSuccess("test_tran");
            logger.info("---------- Success! response body: {}", responseBodyStr);
            logger.info("接收内配入库信息成功: {}", responseBodyStr);
        }else{
            TestUtils.transactionFailure("test_tran");
            logger.error("---------- response code: {}, response body: {}", response.code(), responseBodyStr);
        }
    } catch (Exception e) {
        TestUtils.transactionFailure("test_tran");
        logger.error("error info:", e);
    } finally {
        if (response != null) response.close();
    }
    }
}
```

