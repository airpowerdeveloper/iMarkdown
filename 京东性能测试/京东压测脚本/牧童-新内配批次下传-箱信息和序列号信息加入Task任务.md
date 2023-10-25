```java
import com.jd.forcebot.toolkit.parameterized.latest.AsciiFileAccessArbitrarily;
import com.jd.forcebot.toolkit.parameterized.latest.AsciiFileAccessCircularly;
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
		
		int type = new java.util.Random().nextBoolean() ? 1 : 0;
		logger.info("当前类型为:{}",type);
        if (type == 1) {
            String remark = "单个箱信息,序列号信息下传";
            logger.info("当前模拟场景为:{}",remark);
        } else if (type == 0) {
            String remark = "多个箱信息,序列号信息下传";
            logger.info("当前模拟场景为:{}",remark);
        }
        
        
        int codeType=(int) (Math.random() * (5 - 1 + 1)) + 1;
        logger.info("当前codeType类型:{}",codeType);
        long sendNoTemp = System.currentTimeMillis() * 100000000;
        long boxCodeTemp = System.currentTimeMillis() * 111111111;
        long outSideNoTemp = System.currentTimeMillis();
        //long sendNo;
        String sendNo = "jyTest";
        long boxCode;
        long outSideNo;
        if(codeType == 1){
            sendNo = sendNo + sendNoTemp - 1000;
            boxCode = boxCodeTemp + 1000;
            outSideNo = outSideNoTemp -1000;
        }else if (codeType == 2){
            sendNo = sendNo + sendNoTemp - 2000;
            boxCode = boxCodeTemp + 2000;
            outSideNo = outSideNoTemp -2000;
        }else if (codeType == 3){
            sendNo = sendNo + sendNoTemp - 3000;
            boxCode = boxCodeTemp + 3000;
            outSideNo = outSideNoTemp -3000;
        }else if (codeType == 4){
            sendNo = sendNo + sendNoTemp - 4000;
            boxCode = boxCodeTemp + 4000;
            outSideNo = outSideNoTemp -4000;
        }else if (codeType == 5){
            sendNo = sendNo + sendNoTemp - 5000;
            boxCode = boxCodeTemp + 5000;
            outSideNo = outSideNoTemp -5000;
        }
    
    
        
        //long sendNo = System.currentTimeMillis() * 100000000;
        logger.info("当前批次号sendNo值: {}", sendNo);
        //long boxCode = System.currentTimeMillis() * 111111111;
        logger.info("当前boxCode值: {}", boxCode);
        //String outsideno = new StringBuilder("VW").append(String.valueOf(System.currentTimeMillis() / 100)).toString();
        //long outSideNo = System.currentTimeMillis();
        logger.info("当前箱报文outSideNo值: {}", outSideNo);
		
		//boxCode对应ib_main的barCode值,storehouseNumber对应mini_warehouse_no
		//可以在内配入库单明细表goods_batch_item查询
		
		String requestmsg="";
        if (type == 1) {
            requestmsg = "<Envelope xmlns=\"http://schemas.xmlsoap.org/soap/envelope/\">\n" +
                    "    <Body>\n" +
                    "        <queryWs xmlns=\"http://wms3.360buy.com\">\n" +
                    "            <arg0 xmlns=\"\">{\"bizType\":\"receivedIbBatchMainNew\",\"callCode\":\"JD.MINIWMS.WS.CALLCODE\",\"uuid\":\"1\"}</arg0>\n" +
                    "            <arg1 xmlns=\"\">\n" +"<![CDATA[\n" +"{\n" +"    \"miniSendBatches\":[\n" +
                    "        {\n" +
                    "  \"sendNo\" : \""+sendNo+"\",\n" +
                    //"            \"storehouseNumber\":\"1522908\",\n" +
                    "            \"storehouseNumber\":\"2020253\",\n" +
                    "            \"totalBoxNum\":1,\n" +
                    "            \"goodsItemList\":[\n" +
                    "                {\n" +
                    //"                    \"boxCode\" : \""+boxCode+"\",\n" +
                    "                    \"boxCode\" : \"jyTest16837262411470000\",\n" +
                    //"                    \"outSideNo\" : \""+outSideNo+"\",\n" +
                    "                    \"outSideNo\" : \""+outSideNo+"\",\n" +
                    "                    \"artNo\":\"4726769211\",\n" +
                    "                    \"qty\":10,\n" +
                    "                    \"ywType\":\"G4\",\n" +
                    "                    \"makeDate\":\"2022-05-06\",\n" +
                    "                    \"deadLine\":\"2023-05-06\",\n" +
                    "                    \"lotNo\":\"f739f98387251c113e22309cd5e40d23\",\n" +
                    "                    \"productLevel\":\"1\",\n" +
                    "                    \"storeProperty\":\"冷冻\"\n" +
                    "                }\n" +
                    "            ]\n" +
                    "        }\n" +
                    "    ]\n" +
                    "}\n" +
                    "]]>\n" +
                    "            </arg1>\n" +
                    "        </queryWs>\n" +
                    "    </Body>\n" +
                    "</Envelope>";
        } else if (type == 0) {
            requestmsg = "<Envelope xmlns=\"http://schemas.xmlsoap.org/soap/envelope/\">\n" +
                    "    <Body>\n" +
                    "        <queryWs xmlns=\"http://wms3.360buy.com\">\n" +
                    "            <arg0 xmlns=\"\">{\"bizType\":\"receivedIbBatchMainNew\",\"callCode\":\"JD.MINIWMS.WS.CALLCODE\",\"uuid\":\"1\"}</arg0>\n" +
                    "            <arg1 xmlns=\"\">\n" +"<![CDATA[\n" +"{\n" +"    \"miniSendBatches\":[\n" +
                    "        {\n" +
                    "  \"sendNo\" : \""+sendNo+"\",\n" +
                    //"            \"storehouseNumber\":\"1522908\",\n" +
                    "            \"storehouseNumber\":\"2020253\",\n" +
                    "            \"totalBoxNum\":2,\n" +
                    "            \"goodsItemList\":[\n" +
                    "                {\n" +
                    "                    \"boxCode\" : \"jyTest16837262411470000\",\n" +
                    //"                    \"boxCode\" : \""+boxCode+"\",\n" +
                    "                    \"outSideNo\" : \""+outSideNo+"\",\n" +
                    //"                    \"outSideNo\" : \""+outSideNo+"\",\n" +
                    "                    \"artNo\":\"4726769211\",\n" +
                    "                    \"qty\":10,\n" +
                    "                    \"ywType\":\"G4\",\n" +
                    "                    \"makeDate\":\"2022-05-06\",\n" +
                    "                    \"deadLine\":\"2023-05-06\",\n" +
                    "                    \"lotNo\":\"f739f98387251c113e22309cd5e40d23\",\n" +
                    "                    \"productLevel\":\"1\",\n" +
                    "                    \"storeProperty\":\"冷冻\"\n" +
                    "                },\n" +
                    "                {\n" +
                    "                    \"boxCode\" : \"jyTest16837262411470000\",\n" +
                    //"                    \"boxCode\" : \""+boxCode+"\",\n" +
                    "                    \"outSideNo\" : \""+outSideNo+"\",\n" +
                    //"                    \"outSideNo\" : \""+outSideNo+"\",\n" +
                    "                    \"artNo\":\"100002551513\",\n" +
                    "                    \"qty\":20,\n" +
                    "                    \"ywType\":\"G4\",\n" +
                    "                    \"makeDate\":\"2022-05-20\",\n" +
                    "                    \"deadLine\":\"2023-05-20\",\n" +
                    "                    \"lotNo\":\"f739f98387251c113e22309cd5e40d23\",\n" +
                    "                    \"productLevel\":\"1\",\n" +
                    "                    \"storeProperty\":\"冷冻\"\n" +
                    "                }\n" +
                    "            ]\n" +
                    "        }\n" +
                    "    ]\n" +
                    "}\n" +
                    "                ]]>\n" +
                    "            </arg1>\n" +
                    "        </queryWs>\n" +
                    "    </Body>\n" +
                    "</Envelope>";
        }
        logger.error("新内配批次下传(箱信息，序列号信息), 加入Task任务, 任务信息：DOWN-IBBNEW, parameter: {}",requestmsg);
		
		//MediaType mediaType = MediaType.parse("application/json");
		RequestBody body = RequestBody.create(MediaType.parse("application/xml"), requestmsg);
		Response response = null;
		//Request request = builder.url("http://11.158.11.164/services/simpleWebServiceSoap")
		Request request = builder.url("http://11.147.33.81/services/simpleWebServiceSoap")
	        .header("pfinder", "EAAAAAAAAAAAAAAAAAABMQ==")
		    .header("Content-Type", "application/json")
	        .method("POST", body)
	        .build();
		TestUtils.transactionBegin("test_tran");
		try {
		    response = client.newCall(request).execute();
		    String responseBodyStr = response.body().string();
		    if(response.code() == 200){ //TODO 请根据业务需求添加判断！(如：responseBodyStr.contains("success"))
		        TestUtils.transactionSuccess("test_tran");
		        logger.info("---------- Success! response body: {}", responseBodyStr);
		        logger.error("新内配批次下传(箱信息，序列号信息), 加入Task任务, 任务信息：DOWN-IBBNEW, response body: {}",responseBodyStr);
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

