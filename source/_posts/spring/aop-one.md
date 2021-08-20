---
title: SpringAOP - 使用Spring的API接口实现AOP
categories:
- Notebook
- Spring
tags:
- java
- spring
- aop
---

# 术语

Spring AOP最重要的是可以让用户**自定义切面**。

![image-20210202221824940](/Users/edlison/Library/Application Support/typora-user-images/image-20210202221824940.png)

# 使用Spring的API接口实现AOP

## 需要实现的接口

![image-20210202221950162](/Users/edlison/Library/Application Support/typora-user-images/image-20210202221950162.png)

## 前置通知

```java
public class LogBefore implements MethodBeforeAdvice {
    @Override
    // method: 要执行的目标对象的方法，objects: 参数，o: 目标对象
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println("[Log Before]ClassName: " + o.getClass().getSimpleName() + " MethodName: " + method.getName());
    }
}
```

## 后置通知

```java
public class LogAfter implements AfterReturningAdvice {
    @Override
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("[Log After]ClassName: " + target.getClass().getSimpleName() + " MethodName: " + method.getName());
    }
}
```

## 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
    <bean id="userService" class="com.edlison.design.spring.aop.style_one.service.UserServiceImpl"/>
    <bean id="logBefore" class="com.edlison.design.spring.aop.style_one.log.LogBefore"/>
    <bean id="logAfter" class="com.edlison.design.spring.aop.style_one.log.LogAfter"/>
    <aop:config>
        <aop:pointcut id="first"
                      expression="execution(* com.edlison.design.spring.aop.style_one.service.UserServiceImpl.* (..))"/>
        <aop:advisor advice-ref="logBefore" pointcut-ref="first"/>
        <aop:advisor advice-ref="logAfter" pointcut-ref="first"/>
    </aop:config>
</beans>
```

配置文件中，首先注册`Bean`，引入AOP约束，对AOP进行配置。

- 定义切入点(PointCut).
- 写表达式(Expression).
- 配置通知器(advisor)，并将其与切入点绑定。

通知器执行前后，完全靠**实现的接口**决定。



**注意**：

表达式见Google

