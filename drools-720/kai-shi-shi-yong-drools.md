# 开始使用Drools

* 创建一个规则包

我们在project-&gt;src-&gt;main-&gt;resources文件夹下创建一个规则包，创建完后，我们需要在配置文件中生命一个该配置包的session，配置文件所在的目录为project-&gt;src-&gt;main-&gt;resources-&gt;META-INF-&gt;kmodule.xml,Drools jar包默认从此路径的的此文件中读取此配置XML文件。

配置示例：

```markup
 <?xml version="1.0" encoding="UTF-8"?>
<kmodule xmlns="http://jboss.org/kie/6.0.0/kmodule">
    <kbase name="rules" packages="nestlerules">
        <ksession name="ksession-rules"/>
    </kbase>
    <kbase name="rules2" packages="nestlerules">
        <ksession type="stateless" name="ksession-rules-stateless"/>
    </kbase>
    <!--  订单确认页规则 -->
    <kbase name="rules1" packages="checkrules">
        <ksession name="ksession-rules1"/>
    </kbase>
    <!-- 订单保存时间的价格校验规则 -->
    <kbase name="ordconfirmrules" packages="orderconfirmrules">
        <ksession name="ksession-orderconfirm"/>
    </kbase>
</kmodule>
```

* 创建调用规则的帮助类

调用规则引擎的过程中，有一些通用的方法，我们先封装一下，方便使用，具体代码如下：

```java
package com.drools.util;

import org.drools.core.event.AgendaGroupPoppedEvent;
import org.drools.core.event.AgendaGroupPushedEvent;
import org.drools.core.event.RuleFlowGroupActivatedEvent;
import org.drools.core.event.RuleFlowGroupDeactivatedEvent;
import org.kie.api.KieServices;
import org.kie.api.event.rule.AfterMatchFiredEvent;
import org.kie.api.event.rule.AgendaEventListener;
import org.kie.api.event.rule.BeforeMatchFiredEvent;
import org.kie.api.event.rule.MatchCancelledEvent;
import org.kie.api.event.rule.MatchCreatedEvent;
import org.kie.api.event.rule.ObjectDeletedEvent;
import org.kie.api.event.rule.ObjectInsertedEvent;
import org.kie.api.event.rule.ObjectUpdatedEvent;
import org.kie.api.event.rule.RuleRuntimeEventListener;
import org.kie.api.runtime.KieContainer;
import org.kie.api.runtime.KieSession;
import org.kie.api.runtime.StatelessKieSession;

public class KnowledgeSessionHelper {
    /**
     * @description 创建一个填装规则的方法
     * @return
     */
    public static KieContainer createRuleBase() {

        KieServices ks = KieServices.Factory.get();
        KieContainer kieContainer = ks.getKieClasspathContainer();
        return kieContainer;
    }

    /**
     * @description 创建一个无状态的与规则引擎交互的session
     * StateFul 和 StateLess 的区别在于 Stateful 是可以完成推理的，即一条规则可以导致另一条规则的触发
     * 所以也需要显示的调用下FireRules() 而StateLess是不支持规则推理的，所以规则自动触发
     * @param kieContainer
     * @param sessionName
     * @return
     */
    public static StatelessKieSession getStatelessKnowledgeSession(KieContainer kieContainer, String sessionName) {
        StatelessKieSession kSession = kieContainer.newStatelessKieSession(sessionName);

        return kSession;
    }

    /**
     * @description 创建一个有状态的与规则引擎交互的session
     * @param kieContainer
     * @param sessionName
     * @return
     */
    public static KieSession getStatefulKnowledgeSession(KieContainer kieContainer, String sessionName) {
        KieSession kSession = kieContainer.newKieSession(sessionName);

        return kSession;
    }

    public static KieSession getStatefulKnowledgeSessionWithCallback(KieContainer kieContainer, String sessionName) {
        KieSession session = getStatefulKnowledgeSession(kieContainer, sessionName);
        session.addEventListener(new RuleRuntimeEventListener() {

            @Override
            public void objectDeleted(ObjectDeletedEvent arg0) {
                // TODO Auto-generated method stub
                System.out.println("Object retracted \n"
                        + "new Content \n" + arg0.getOldObject().toString());
            }

            @Override
            public void objectInserted(ObjectInsertedEvent arg0) {
                // TODO Auto-generated method stub
                System.out.println("Object inserted \n"
                        + arg0.getObject().toString());
            }

            @Override
            public void objectUpdated(ObjectUpdatedEvent arg0) {
                // TODO Auto-generated method stub
                System.out.println("Object was updated \n"
                        + "new Content \n" + arg0.getOldObject().toString());
            }

        });

        session.addEventListener(new AgendaEventListener() {
            public void matchCreated(MatchCreatedEvent event) {
                System.out.println("The rule "
                        + event.getMatch().getRule().getName()
                        + " can be fired in agenda");
            }
            public void matchCancelled(MatchCancelledEvent event) {
                System.out.println("The rule "
                        + event.getMatch().getRule().getName()
                        + " cannot b in agenda");
            }
            public void beforeMatchFired(BeforeMatchFiredEvent event) {
                System.out.println("The rule "
                        + event.getMatch().getRule().getName()
                        + " will be fired");
            }
            public void afterMatchFired(AfterMatchFiredEvent event) {
                System.out.println("The rule "
                        + event.getMatch().getRule().getName()
                        + " has be fired");
            }
            @Override
            public void afterRuleFlowGroupActivated(org.kie.api.event.rule.RuleFlowGroupActivatedEvent arg0) {
                // TODO Auto-generated method stub

            }
            @Override
            public void afterRuleFlowGroupDeactivated(org.kie.api.event.rule.RuleFlowGroupDeactivatedEvent arg0) {
                // TODO Auto-generated method stub

            }
            @Override
            public void agendaGroupPopped(org.kie.api.event.rule.AgendaGroupPoppedEvent arg0) {
                // TODO Auto-generated method stub

            }
            @Override
            public void agendaGroupPushed(org.kie.api.event.rule.AgendaGroupPushedEvent arg0) {
                // TODO Auto-generated method stub

            }
            @Override
            public void beforeRuleFlowGroupActivated(org.kie.api.event.rule.RuleFlowGroupActivatedEvent arg0) {
                // TODO Auto-generated method stub

            }
            @Override
            public void beforeRuleFlowGroupDeactivated(org.kie.api.event.rule.RuleFlowGroupDeactivatedEvent arg0) {
                // TODO Auto-generated method stub

            }
        });

        return session;
    }

}
```

* 使用示例

```java
package com.drools.connectors;

import java.util.List;

import org.kie.api.runtime.KieContainer;
import org.kie.api.runtime.KieSession;
import org.kie.api.runtime.StatelessKieSession;

import com.drools.util.KnowledgeSessionHelper;
import com.gpersist.entity.ReturnValue;
import com.nestle.entity.act.InvoiceTrans;
import com.nestle.entity.act.InvoiceTransdetail;
import com.nestle.entity.ctm.CtmBase;
import com.nestle.entity.ord.OrdBase;
import com.nestle.entity.ord.OrdSubdetail;

public class OrderChange {

    private String rulename = "";
    public OrderChange(String name) {
        rulename = name;
    }

    StatelessKieSession sessionStateless = null;
    KieSession sessionStatefull = null;
    static KieContainer kieContainer;

    private OrderBase orderbase;

    private ReturnValue rtv;

    public OrderBase getOrderbase() {
        return orderbase;
    }

    public void setOrderbase(OrderBase orderbase) {
        this.orderbase = orderbase;
    }

    public ReturnValue getRtv() {
        return rtv;
    }

    public void setRtv(ReturnValue rtv) {
        this.rtv = rtv;
    }

    public void excute() {
        kieContainer = KnowledgeSessionHelper.createRuleBase();
        sessionStatefull = KnowledgeSessionHelper.getStatefulKnowledgeSession(kieContainer, rulename);
        sessionStatefull.insert(orderbase);
        sessionStatefull.insert(rtv);
        int n = sessionStatefull.fireAllRules();
        sessionStatefull.dispose();
        System.out.println("共触发了"+n+"条规则");
    }


}
```

示例可见[github](https://github.com/jingchenxu/drools-lesson)

