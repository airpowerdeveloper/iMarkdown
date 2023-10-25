```java
import com.jd.forcebot.toolkit.parameterized.latest.AsciiFileAccessArbitrarily;
import com.jd.forcebot.toolkit.parameterized.latest.AsciiFileAccessCircularly;
import com.jd.forcebot.toolkit.parameterized.latest.AsciiFileAccessCommonArbitrarily;
import com.jd.forcebot.engine.groovy.Lifecycle;
import com.jd.forcebot.engine.groovy.RatePolicy;
import com.jd.forcebot.engine.groovy.TestCase;
import com.jd.forcebot.engine.groovy.TestSuite;
import java.util.concurrent.TimeUnit;
import org.junit.Assert;
import com.jd.forcebot.engine.groovy.*;
import com.jd.forcebot.toolkit.*;
import com.jd.forcebot.engine.TestUtils;
import org.junit.Assert;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.alibaba.fastjson.JSONObject;
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.TypeReference;
import org.apache.commons.lang3.RandomUtils;
import com.jd.jsf.gd.config.ConsumerConfig;
import com.jd.jsf.gd.config.RegistryConfig;
import com.jd.rd.wms.jsf.out.service.OutTransferFacade;
import com.jd.jsf.gd.util.RpcContext;
import com.jd.rd.wms.jsf.out.dto.OutDetailQuery;

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
*
*/
@TestSuite
class Test {
    public final Logger logger = TestUtils.LOGGER;
    private static OutTransferFacade outTransferFacade;
    //读取参数化数据文件
    //static AsciiFileAccessCommonArbitrarily atomicAccess = new  AsciiFileAccessCommonArbitrarily("getOutboundDetailListByWareIdAndTransferId_data.txt");
    static AsciiFileAccessCommonArbitrarily atomicAccess = new  AsciiFileAccessCommonArbitrarily("getOutboundDetailListByWareIdAndTransferId_data_transferIdAndWareId.txt");

    static {
        RegistryConfig jsfRegistry = new RegistryConfig();
        jsfRegistry.setIndex("i.jsf.jd.com"); // 测试环境:test.i.jsf.jd.local
        // 服务提供者连接注册中心，设置属性
        ConsumerConfig<OutTransferFacade> consumerConfig = new ConsumerConfig<OutTransferFacade>();
        consumerConfig.setInterfaceId("com.jd.rd.wms.jsf.out.service.OutTransferFacade");
        consumerConfig.setAlias("RD_YFB");
        consumerConfig.setProtocol("jsf");
        // 设置序列化方式hessian or msgpack
        consumerConfig.setSerialization("msgpack");
        consumerConfig.setTimeout(10000);
        // 如果lib有xml配的注册中心，使用consumerConfig.setRegistry(RegistryFactory.getRegistryConfigs());
        consumerConfig.setRegistry(jsfRegistry);
        outTransferFacade = consumerConfig.refer();

    }

    @TestCase(record=true)
    void test_0() {
        String[] data = (atomicAccess.readLine()).split(",");
        logger.info("当前出库类型为:"+data[2]);
        //TODO 请根据测试需要构造入参
         //com.jd.rd.wms.jsf.out.dto.OutDetailQuery param1 = JSON.parseObject("{\"transferId\":1185930265,\"wareId\":100014109353}", com.jd.rd.wms.jsf.out.dto.OutDetailQuery.class);
         com.jd.rd.wms.jsf.out.dto.OutDetailQuery param1 = JSON.parseObject("{\"transferId\":"+ data[0] +",\"wareId\":"+ data[1] +"}", com.jd.rd.wms.jsf.out.dto.OutDetailQuery.class);
         logger.info("getOutboundDetailListByWareIdAndTransferId:39 - <===============call getOutboundDetailListByWareIdAndTransferId param: {}", JSON.toJSONString(param1));
        //byte[] b = Base64.getDecoder().decode("EAAAAAAAAAAAAAAAAAABMQ==");
        //RpcContext.getContext().setAttachment("pfinder", b);
        TestUtils.transactionBegin("getOutboundDetailListByWareIdAndTransferId");
        try {
           com.jd.rd.wms.jsf.common.ServiceResult<java.util.List<com.jd.rd.wms.jsf.out.result.OutDetailReturnEx>> retObj = outTransferFacade.getOutboundDetailListByWareIdAndTransferId(param1 );
            if (retObj != null) { //TODO 请根据业务需求添加判断，如code，msg等信息
                String resultStr = JSONObject.toJSONString(retObj);
                if(!"null".equals(resultStr)){
                    TestUtils.transactionSuccess("getOutboundDetailListByWareIdAndTransferId");
                    logger.info("[ASSERT-SUCCESS]: Tran:[getOutboundDetailListByWareIdAndTransferId], Expected:[null], Response:[{}] [ASSERT-CLOSED]", resultStr);
                    logger.info("===============>call getOutboundDetailListByWareIdAndTransferId success:{}",resultStr);
                }else{
                    TestUtils.transactionFailure("getOutboundDetailListByWareIdAndTransferId");
                    logger.error("[ASSERT-ERROR]: Tran:[getOutboundDetailListByWareIdAndTransferId], Expected:[null], Response:[{}] [ASSERT-CLOSED]", resultStr);
                }
            } else {
                TestUtils.transactionFailure("getOutboundDetailListByWareIdAndTransferId");
                logger.error("[ASSERT-ERROR]: Tran:[getOutboundDetailListByWareIdAndTransferId], Expected:[null], Response:[null] [ASSERT-CLOSED]");
            }
        } catch (Exception e) {
            TestUtils.transactionFailure("getOutboundDetailListByWareIdAndTransferId");
            logger.error("[TEST-ERROR]: Tran:[getOutboundDetailListByWareIdAndTransferId], ErrorMsg:{}[ErrorMsg]", e.getMessage(), e);
        }
    }
}
```

