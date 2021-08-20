---
title: SpringAOP - 自定义切面实现AOP
categories:
- Notebook
- Spring
tags:
- java
- spring
- aop
---

# 自定义切面实现AOP

通过切面来实现AOP

## Aspect

```java
public class CustomAspect {
    public void before() {
        System.out.println("[Log] Before...");
    }
    public void after() {
        System.out.println("[Log] After...");
    }
}
```

## 配置文件

```xml
<aop:config>
        <aop:aspect ref="customAspect">
            <aop:pointcut id="first"
                          expression="execution(* com.edlison.design.spring.aop.style_api.service.UserServiceImpl.*(..))"/>

            <aop:before method="before" pointcut-ref="first"/>
            <aop:after method="after" pointcut-ref="first"/>
        </aop:aspect>
    </aop:config>
```

这种方式更加简便，只需要定义一个切面类，类中的方法即可通过配置，来对切点进行操作，不再需要繁琐的实现接口。

但是无法实现更复杂的操作，比如打印调用的切点方法，类名等。