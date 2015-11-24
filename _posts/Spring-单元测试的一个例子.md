title: Spring 单元测试的一个例子
tags: Spring
categories: java
feature: img/spring_000001.jpg
description: Spring 单元测试的例子解析.
comments: true
recommend: true
declaration: true
toc: true
date: 2015-11-10 16:07:39
---

在使用Spring时经常会用到它的单元测试，但总是不是很理解其中的用一些特性；这里通过一个示例简单说明其一般的使用。

<!--more-->

### 源码
先贴上源码

```java
/**
 * 
 *汇付天下有限公司
 * Copyright (c) 2006-2015 ChinaPnR,Inc.All Rights Reserved.
 */
package com.huifu;
import java.util.HashMap;
import java.util.Map;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.AbstractJUnit4SpringContextTests;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import com.huifu.scp.commons.event.SCPTradeEvent;
import com.huifu.scp.commons.utils.ContextHolder;
import com.huifu.scp.commons.utils.DateUtil;
import com.huifu.scpcore.common.facade.model.SCPResponseEvent;
import com.huifu.scpcore.control.SCPControlManager;
/**
 * 
 * @author kalven.meng
 * @version $Id: B2CRuleTest.java, v 0.1 2015-10-14 上午11:44:36 kalven.meng Exp $
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations={"classpath:/applicationContext*.xml",
                                 "classpath:/applicationContext-dao-mongoDB.xml",
                                 "classpath:/applicationContext-service.xml",
                                 "classpath:/applicationContext-resources.xml",
                                 "classpath:/applicationContext-integration.xml",
                                 "classpath:/applicationContext-biz.xml",
                                 "classpath:/applicationContext-dao.xml",
                                 "classpath:/applicationContext-resources-jdbc.xml",
                                 "classpath:/applicationContext-placeholder.xml",
                                 "classpath:/applicationContext-resource-runtime.xml"
                                 })
public class B2CRuleTest extends AbstractJUnit4SpringContextTests {
    private SCPTradeEvent reqTradeEvent;
    private SCPTradeEvent respTradeEvent;
    
    @Autowired
    private SCPControlManager   scpControlManager;
    
    
    
    @Before
    public void init() {
        ContextHolder.applicationContext = applicationContext;
        reqTradeEvent = new SCPTradeEvent();
        respTradeEvent = new SCPTradeEvent();
        buildReqTradeEvent(reqTradeEvent);
        buildRepTradeEvent(respTradeEvent,reqTradeEvent);
    }
    
    @Test
    public void testB2CRule() {
        //事前交易
        SCPResponseEvent respEvent = scpControlManager.scpEventProdAction(reqTradeEvent);
        System.out.println("订单号:"+respEvent.getEventRefNo());
        System.out.println("规则执行结果:"+respEvent.getResultCode());
        SCPResponseEvent result = scpControlManager.scpEventProdAction(respTradeEvent);
        
    }
    
    /**
     * 构建事前事件
     * @param tradeEvent
     */
    private void buildReqTradeEvent (SCPTradeEvent tradeEvent) {
        tradeEvent.setTradeType("1000");
        tradeEvent.setTradeAmount(Long.parseLong("3000000"));
        tradeEvent.setEventRefNo(DateUtil.getCurrentDateTime());
        tradeEvent.setEventType("03");
        tradeEvent.setEventSubType("b2c");
        tradeEvent.setSyscode("02");
        tradeEvent.setEventMode("01");
        tradeEvent.setRequesttime(Long.parseLong(DateUtil.getCurrentDateTimeMs()));
        tradeEvent.setRequestType("01");
        tradeEvent.setCustId("15503254521");
        tradeEvent.setTargetGateId("T2");
        tradeEvent.setPriority(5);
        Map<String, Object> extandMap = new HashMap<String , Object>();
        extandMap.put("ipAddr", "192.168.1.23");
        tradeEvent.setExtandMap(extandMap);
    }
    /**
     * 构建事后事件
     * @param reqTradeEvent
     */
    private void buildRepTradeEvent (SCPTradeEvent respTradeEvent , SCPTradeEvent reqTradeEvent) {
        respTradeEvent.setEventRefNo(reqTradeEvent.getEventRefNo());
        respTradeEvent.setCustId(reqTradeEvent.getCustId());
        respTradeEvent.setEventType("03");
        respTradeEvent.setSyscode("02");
        respTradeEvent.setEventSubType("b2c");
        respTradeEvent.setEventMode("01");
        respTradeEvent.setRequesttime(reqTradeEvent.getRequesttime());
        respTradeEvent.setTxDate(DateUtil.getCurrentDate());
        respTradeEvent.setRequestType("03");
        respTradeEvent.setTermRespCode("001");
        respTradeEvent.setTradeStat("S");
    }
    
    /**
     * 从Spring中获取实例
     * @param type
     * @return
     */
    public <T> T getBean(Class<T> type) {
        return applicationContext.getBean(type);
    }
}
```
### 解析
下面顺序的对一些要点进行说明

1.  `@RunWith(SpringJUnit4ClassRunner.class)`注解
    
    该注解表示让测试运行于Spring测试环境

2.  `@ContextConfiguration`注解
    
    该注解用于设置加载Spring的配置文件

3.  `extends AbstractJUnit4SpringContextTests`
    
    有时我们需要获取Spring的`applicationContext`，继承类'AbstractJUnit4SpringContextTests'后就可以直接使用`applicationContext`
