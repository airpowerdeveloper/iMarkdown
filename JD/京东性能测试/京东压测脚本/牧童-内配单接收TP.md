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
import java.text.SimpleDateFormat;
import java.util.Date;

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
    int orderIdType=(int) (Math.random() * (4 - 1 + 1)) + 1;
    logger.info("当前orderIdType类型:{}",orderIdType);
    long orderIdTemp = System.currentTimeMillis();
    long orderId;
    if(orderIdType == 1){
        orderId = orderIdTemp-1000;
    }else if (orderIdType == 2){
        orderId = orderIdTemp-10000;
    }else if (orderIdType == 3){
        orderId = orderIdTemp-2000;
    }else if (orderIdType == 4){
        orderId = orderIdTemp-20000;
    }
    
    //long orderId = System.currentTimeMillis()-1000;
    logger.info("当前orderId值: {}", orderId);
    String createDate = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date());
    String createTime = createDate.substring(0,10);
    String createTime1 = createTime.replaceAll("-","/");
    logger.info("当前createDate和createTime值: {}", createDate);
    
    //cky2,partnerid对应wms_distribution_no,mobileStoreId对应mini_warehouse_no
    //so_main和so_item表查询
    
    String requestmsg="<Envelope xmlns=\"http://schemas.xmlsoap.org/soap/envelope/\">\n" +
                "    <Body>\n" +
                "        <queryWs xmlns=\"http://wms3.360buy.com\">\n" +
                "            <arg0 xmlns=\"\">{\"bizType\":\"receivedSoMain\",\"callCode\":\"JD.MINIWMS.WS.CALLCODE\",\"uuid\":\"1\"}</arg0>\n" +
                "            <arg1 xmlns=\"\">\n" +
                "                <![CDATA[\n" +
                "                {\n" +
                "  \"address\" : \"京东4号楼\",\n" +
                "  \"inputAddress\" : null,\n" +
                "  \"cky2\":364,\n" +
                "  \"createDate\" : \""+createDate+"\",\n" +
                "  \"createTime\" : \""+createDate+"\",\n" +
                "  \"customerName\" : \"zhangyu1540\",\n" +
                "  \"di\" : 70,\n" +
                "  \"email\" : \"\",\n" +
                "  \"factPrice\" : 65.55,\n" +
                "  \"id\" : 0,\n" +
                "  \"isJdShip\" : 0,\n" +
                "  \"jsonStr\" : \"{\\\"cardInfo\\\":\\\"\\\",\\\"promiseMsg\\\":\\\"06月09日18:50前送达\\\",\\\"transferStationName\\\":\\\"\\\",\\\"transferStationId\\\":\\\"0\\\",\\\"expproperty\\\":\\\"00000000000000000000000000000000000000000000000000\\\",\\\"greetingCardContent\\\":\\\"\\\",\\\"jsonText\\\":\\\"{\\\\\\\"codDate\\\\\\\":\\\\\\\""+createTime+"\\\\\\\",\\\\\\\"promise_date\\\\\\\":\\\\\\\""+createTime+"\\\\\\\",\\\\\\\"road\\\\\\\":\\\\\\\"0\\\\\\\",\\\\\\\"batch\\\\\\\":\\\\\\\"2小时达\\\\\\\",\\\\\\\"ivct\\\\\\\":\\\\\\\"1\\\\\\\",\\\\\\\"batchTime\\\\\\\":\\\\\\\"立即送达\\\\\\\"}\\\",\\\"energy\\\":\\\"\\\"}\",\n" +
                "  \"jyn\" : 89,\n" +
                "  \"locNo\" : \"\",\n" +
                "  \"memberId\" : \"ripple_611\",\n" +
                "  \"mobile\" : \"10086\",\n" +
                "  \"odInvoicesList\" : [ {\n" +
                "    \"cky2\":364,\n" +
                "    \"content\" : \"{\\\"cky2\\\":0,\\\"iarea\\\":0,\\\"message\\\":\\\"订单没有需要开具的发票,orderId="+orderId+",ivcState=5,invoiceType:3\\\",\\\"nextIcode\\\":0,\\\"orderId\\\":0,\\\"orgId\\\":0,\\\"power\\\":false,\\\"result\\\":\\\"error\\\",\\\"storeId\\\":0,\\\"wmsType\\\":2}\",\n" +
                "    \"drawer\" : null,\n" +
                "    \"groupNo\" : 1,\n" +
                "    \"id\" : 0,\n" +
                "    \"jyn\" : 89,\n" +
                "    \"lockFlag\" : 0,\n" +
                "    \"orderId\" : \""+orderId+"\",\n" +
                "    \"processStatus\" : 0,\n" +
                "    \"retryNum\" : 0,\n" +
                "    \"shipmentId\" : 0,\n" +
                "    \"storeid\":501,\n" +
                "    \"stu\" : 0,\n" +
                "    \"suffix\" : null,\n" +
                "    \"types\" : 3,\n" +
                "    \"updateDate\" : null,\n" +
                "    \"cdate\" : \""+createDate+"\"\n" +
                "  } ],\n" +
                "  \"odOrderDetailList\" : [ {\n" +
                "    \"id\" : 0,\n" +
                "    \"orderid\" : \""+orderId+"\",\n" +
                "    \"price\" : 49.90,\n" +
                "    \"productId\" : \"EMG1800490002\",\n" +
                "    \"productName\" : \"大希地 芝士披萨组合3口味540g/盒（美式培根 果蔬牛肉 蜜汁鸡肉）pizza 马苏里拉奶酪 披萨半成品\",\n" +
                "    \"quantity\" : 1,\n" +
                "    \"serialId\" : 1,\n" +
                "    \"isVMI\" : 0,\n" +
                "    \"mobileStoreId\":\"2020253\",\n" +
                "    \"extTagsMap\" : {\n" +
                "      \"skuUuid\" : \"1012_F1g3Z3y1091764786635882496\",\n" +
                "      \"xm\" : null,\n" +
                "      \"BrandID\" : \"52894\"\n" +
                "    },\n" +
                "    \"categoryId\" : null,\n" +
                "    \"packInfo\" : null,\n" +
                "    \"skuUuid\" : null\n" +
                "  }, {\n" +
                "    \"id\" : 0,\n" +
                "    \"orderid\" : \""+orderId+"\",\n" +
                "    \"price\" : 24.80,\n" +
                "    \"productId\" : \"EMG1800490002\",\n" +
                "    \"productName\" : \"大希地 精制培根肉片200g/袋 早餐食材 三明治手抓饼伴侣 火锅涮锅烧烤\",\n" +
                "    \"quantity\" : 1,\n" +
                "    \"serialId\" : 2,\n" +
                "    \"isVMI\" : 0,\n" +
                "    \"mobileStoreId\":\"2020253\",\n" +
                "    \"extTagsMap\" : {\n" +
                "      \"skuUuid\" : \"1012_F2t2t3G1091766426215288832\",\n" +
                "      \"xm\" : null,\n" +
                "      \"BrandID\" : \"52894\"\n" +
                "    },\n" +
                "    \"categoryId\" : null,\n" +
                "    \"packInfo\" : null,\n" +
                "    \"skuUuid\" : null\n" +
                "  } ],\n" +
                "  \"odPrintInfo\" : {\n" +
                "    \"cky2\":364,\n" +
                "    \"id\" : 0,\n" +
                "    \"invoiceDetail\" : \"\",\n" +
                "    \"orderid\" : \""+orderId+"\",\n" +
                "    \"printInfo\" : \"H4sIAAAAAAAAAL1Ue0/bVhT/KlH+qDbx6LWx82BCUxzCIyUhDhQKVTU59oW4OLZzfU0IDKmsq0qA\\nFrYi0a3p2Ea7tmu39TExCox+mdhJ/upX2L1xWmhpaaVKO4pOzv3d8/ido3M9589IWM726xOGv9P/\\nGQ+Yz9vOn6/8+8PL/R9BOwAXfF/7Lvhb/YWc1Q2xpGoDqoX9nefn/CZSZUhiuHB7GBAPExmKLWNV\\nIRgDAGAD4WAwyAYOr3QpRwOcO/ecnRWn/NhXW7rtbD10lx7W1u5X97511hY7nNUt57tnPAcmT1dv\\nff9yf7F6cN3ZX3U2n7s/P/e5P5Vr64+qpVu1b0q+WrnsPlmo7/xCDi/3S6Y6Oyv56g8e1ZZX61dX\\n3OWSc3e7fuV3n5ffubbkLq45NxYIn7wt6VjFRcqU9EgQy8692c9862GHLNceemeHIdDBhoMndVh9\\neuAsbnvsCc1q6SoLSGu1X5d97saD+p21+tame3vNV9kpuTevu0/33NKyu3Sjfne/sv935WDLV114\\nWF+/4m7/SXT18r3q5Tsf4u+xnb9AQBVToLJTdjavOyu7bcQxIyHZUCgsSChKrHbJMme+jBpmMa1O\\nZvEwnMFdpwTPa7howq7oYHeMYUPCqW4JS10sF+JYjgU8x4RJOmxgSUs1KwOPiyxpGh3C2m/kYCAF\\non6lweuNSEJXx8NqY14sYNk2EGgDYR8T6ORBJxciHgjKUJ2GiDjQSlDzDAVqFC02Y92bm255s7Jz\\nqXb1WdPeXSdukqIgaFleDII5CU15trfvlBEIuOVFEHY37jIhUtUpXatfWqgdHLwOmDW9ENm2sJGL\\nenOjZwQlDN9HvoMuhG1BlPS2gZJXsdZYjJWNyu6j6r09Z+l+/QotV9n5w3lxuY28O2f1H2f3RptT\\nvl/dfeGu/1XZu/lqwEk7RwuRo5qTJhtFncer7sY2IfvV+7popWsBdSUl0T0BTWHBW0IAusugg/6Y\\nQ5wJHLHp7YnC0FAGcB9w+2AWqjqOYcdInxDekI8M+BRh3ij4lvwP9T9CjryhWDJKliASGeaBmk5o\\nLfxMz8VzDMDiRIQfHpB1dBYlhDFkDLZMgVkt3TthGdNidEyKdNGnmjV06GUgp5yRUTV4csJARIDT\\nLelE1p5k00PjeCQdv6gMCdNZ0N91+DZPzpHHeGgGTqcilsCOc7PDOrbGelOZdCLWYomawdlZ6SI7\\nlNDzcDRvB60RBZlCUQ4JMaW/dySn9tpnhGj8ND8xaPfIo3098VQ01j1YiCXkuK5n+tFZUeweEEcL\\nmVBs3E4mBPbIo/2kaUFdRkUTQ2WooJIvTfPhvkb7dQXOEHDuVN428BexV3iiMdbGrXfT6f1l8pEU\\nX8gIBTsvSlo8gyIZiPjEK30ElzKBYndhXB3pK4hSyO5Ii57t5Wl9qx7t83g1Oz8yxRdsJIwRfaYn\\nNVp4d/RoFqJ3hSOtjwRy4VAiYnNMgui8CWlCLU91coToAXx6lNjMJPVkxolPA8EoLgepv0lwKTkZ\\nEzGf7is2m7CRqfMFnJ8pEh/OTJIoxPAECfToRREHkueKojcSWc8mghFvAJ7dHOFsvCcmNnUD8TjP\\n++f/AzVdfoYFCQAA\",\n" +
                "    \"type\" : 1\n" +
                "  },\n" +
                "  \"orderid\" : \""+orderId+"\",\n" +
                "  \"partnerid\" : \"364\",\n" +
                "  \"payment\" : 4,\n" +
                "  \"payremk\" : null,\n" +
                "  \"paysureDate\" : \""+createDate+"\",\n" +
                "  \"phone\" : \"\",\n" +
                "  \"printInfoFlag\" : 0,\n" +
                "  \"printflag\" : 1,\n" +
                "  \"printx\" : 0,\n" +
                "  \"remark\" : \"\",\n" +
                "  \"qlShipNo\" : \"JD0074728915250\",\n" +
                "  \"sendpay\" : \"00000000200000000000000002001000030030100000000001600000000001030000000000000000000000000000000101001040000000000000000000000000000000000000000000000000000000000000010000130000000000000100020000000000000000000000000000000000000010000000002000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000100000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\",\n" +
                "  \"sheetType\" : 89,\n" +
                "  \"sndDate\" : \""+createTime1+"\",\n" +
                "  \"sndTime\" : \"\",\n" +
                "  \"state\" : 7,\n" +
                "  \"state2\" : 9,\n" +
                "  \"status\" : 0,\n" +
                "  \"storeid\":501,\n" +
                "  \"totalPrice\" : 74.70,\n" +
                "  \"ucity\" : 2802,\n" +
                "  \"ucounty\" : 2821,\n" +
                "  \"updateDate\" : \""+createDate+"\",\n" +
                "  \"uprovince\" : 1,\n" +
                "  \"version\" : null,\n" +
                "  \"volume\" : 3356.0000,\n" +
                "  \"weight\" : 0.7900,\n" +
                "  \"yn\" : 0,\n" +
                "  \"youhui\" : 21.15,\n" +
                "  \"yun\" : 12,\n" +
                "  \"zip\" : \"\",\n" +
                "  \"ziti\" : 0,\n" +
                "  \"shoppingInfo\" : null,\n" +
                "  \"extendMessage\" : {\n" +
                "    \"proType\" : \"\",\n" +
                "    \"cardInfo\" : \"[]\",\n" +
                "    \"plusUserFlag\" : \"0000000000000000201000000000010900100000000000006300401000000280000000000000000000000000000000000000\",\n" +
                "    \"finalRobberTime\" : \"2022-06-09 17:05:49\",\n" +
                "    \"batch\" : \"2小时达\",\n" +
                "    \"add_lng\" : \"116.435478\",\n" +
                "    \"extraParams\" : {\n" +
                "    },\n" +
                "    \"vendorRemarkInfo\" : \"[]\",\n" +
                "    \"add_lat\" : \"39.890347\",\n" +
                "    \"batchTime\" : \"立即送达\",\n" +
                "    \"sortingMessage\" : {\n" +
                "      \"stationType\" : \"0\",\n" +
                "      \"waybillCode\" : \"JD0074728915250\"\n" +
                "    },\n" +
                "    \"codDate\" : \""+createTime+"\",\n" +
                //"    \"finalProductionTime\" : \""+createDate+"\",\n" +
                "    \"finalProductionTime\" : \"2023-05-17 23:30:00\",\n" +
                "    \"OrderPayShipmentInfo\" : \"[{\\\"packageType\\\":1,\\\"idPaymentType\\\":4,\\\"paymentWay\\\":0,\\\"idShipmentType\\\":71,\\\"promiseType\\\":\\\"0004\\\",\\\"codDate\\\":\\\""+createTime+"\\\",\\\"batch\\\":\\\"立即送达\\\",\\\"batchId\\\":0,\\\"codTime\\\":null,\\\"carrier\\\":null,\\\"offset\\\":0,\\\"menDianId\\\":0,\\\"stationId\\\":0,\\\"halfReceive\\\":0,\\\"cyjx\\\":0,\\\"supportedUuIdList\\\":\\\"1012_F1g3Z3y1091764786635882496,1012_F2t2t3G1091766426215288832\\\",\\\"idPickSite\\\":0,\\\"idPickSiteName\\\":null,\\\"cutOrder\\\":0,\\\"venderId\\\":0,\\\"isNewVersion\\\":1,\\\"selectedItem\\\":1,\\\"mobilestoretype\\\":4,\\\"finalDelieveredTime\\\":\\\"2022-06-09 18:50:34\\\",\\\"timeRange\\\":null,\\\"delieveredTime\\\":null,\\\"productionEndTime\\\":null,\\\"cutOrderTime\\\":null,\\\"newStoreType\\\":2,\\\"promisesendpay35\\\":\\\"0\\\",\\\"bondedArea\\\":null,\\\"speedHour\\\":\\\"2\\\",\\\"speedMark\\\":null,\\\"businessBeginTime\\\":null,\\\"businessEndTime\\\":null,\\\"tagType\\\":null,\\\"tagContent\\\":null,\\\"shipType\\\":0,\\\"venderType\\\":null,\\\"siteId\\\":null,\\\"bondedAreaName\\\":null,\\\"selfPickSiteType\\\":0,\\\"transferDays\\\":null,\\\"promiseSendPay\\\":{\\\"366\\\":\\\"0\\\",\\\"367\\\":\\\"2\\\",\\\"172\\\":\\\"3\\\"},\\\"serviceSiteId\\\":null,\\\"deliverType\\\":null,\\\"consolidationCode\\\":null,\\\"consolidationName\\\":null,\\\"promiseMsg\\\":null,\\\"lightBatchType\\\":null,\\\"containCalendarBatchs\\\":\\\"1\\\",\\\"lastDeliveryDate\\\":0,\\\"netWorkType\\\":-1,\\\"workerIn\\\":true,\\\"crssDay\\\":false}]\",\n" +
                "    \"userLevel\" : \"105\",\n" +
                "    \"finalDelieveredTime\" : \""+createDate+"\",\n" +
                "    \"sendPayMap\" : \"{\\\"26\\\":\\\"2\\\",\\\"29\\\":\\\"1\\\",\\\"820\\\":\\\"1\\\",\\\"190\\\":\\\"2\\\",\\\"171\\\":\\\"1\\\",\\\"172\\\":\\\"3\\\",\\\"239\\\":\\\"2\\\",\\\"338\\\":\\\"1\\\",\\\"50\\\":\\\"1\\\",\\\"51\\\":\\\"6\\\",\\\"96\\\":\\\"1\\\",\\\"98\\\":\\\"1\\\",\\\"34\\\":\\\"3\\\",\\\"37\\\":\\\"3\\\",\\\"39\\\":\\\"1\\\",\\\"17\\\":\\\"2\\\",\\\"186\\\":\\\"1\\\",\\\"166\\\":\\\"1\\\",\\\"342\\\":\\\"1\\\",\\\"101\\\":\\\"1\\\",\\\"103\\\":\\\"4\\\",\\\"367\\\":\\\"2\\\",\\\"9\\\":\\\"2\\\",\\\"229\\\":\\\"1\\\",\\\"62\\\":\\\"1\\\",\\\"64\\\":\\\"3\\\"}\",\n" +
                "    \"endOrderDate\" : \""+createDate+"\"\n" +
                "  },\n" +
                "  \"extendInfo\" : {\n" +
                "    \"qingLongInfo\" : [ {\n" +
                "      \"consolidator_info\" : null\n" +
                "    } ],\n" +
                "    \"distributionInfo\" : {\n" +
                "    }\n" +
                "  },\n" +
                "  \"shipmentType\" : 0,\n" +
                "  \"siteCode\" : 0,\n" +
                "  \"siteName\" : null,\n" +
                "  \"telephone\" : null,\n" +
                "  \"docSource\" : 0,\n" +
                "  \"memo\" : null,\n" +
                "  \"deliveryTime\" : null,\n" +
                "  \"bizType\" : 0,\n" +
                "  \"environment\" : null,\n" +
                "  \"stateName\" : null,\n" +
                "  \"cityName\" : null,\n" +
                "  \"areaName\" : null,\n" +
                "  \"townName\" : null,\n" +
                "  \"townId\" : 0,\n" +
                "  \"paymentWay\" : null,\n" +
                "  \"orgId\" : null,\n" +
                "  \"carrierNo\" : null,\n" +
                "  \"thirdDistributionInfo\" : null,\n" +
                "  \"mOrderType\" : null,\n" +
                "  \"shortageFlag\" : null,\n" +
                "  \"intervalEndTime\" : null,\n" +
                "  \"jitDate\" : null,\n" +
                "  \"jitEndDate\" : null,\n" +
                "  \"jitLatestEndDate\" : null,\n" +
                "  \"handoverSlip\" : null,\n" +
                "  \"latitude\" : 29.100199076381397,\n" +
                "  \"longitude\" : 89.25184523772896,\n" +
                "  \"organizerShopName\" : null\n" +
                "}\n" +
                "]]>        \n" +
                "            </arg1>\n" +
                "        </queryWs>\n" +
                "    </Body>\n" +
                "</Envelope>";             
    logger.info("当前参数化初始化报文SoMainInit：{}",requestmsg);
    
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
            logger.info("接收订单成功: {}"+responseBodyStr);
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

