---
title: SpringAOP - 注解实现AOP
categories:
- Notebook
- Spring
tags:
- java
- spring
- aop
---

# 注解实现AOP

## 配置类

```java
@Configuration
@EnableAspectJAutoProxy
public class AnnoAOPConfig {
    @Bean
    public LogAnno logAnno() {
        return new LogAnno();
    }
    @Bean
    public LoginService loginService() {
        return new LoginServiceImpl();
    }
}
```

注意`@EnableAspectJAutoProxy`**开启自动Proxy**。

如果是使用xml进行配置，也需要开启自动Proxy。

业务类应该返回**相应的接口**，动态路由的底层实现即是对接口进行调用。

## 切面类

```java
@Aspect
@Component
public class LogAnno {
    @Before("execution(* com.edlison.design.spring.aop.style_anno.LoginServiceImpl.*(..))")
    public void before() {
        System.out.println("[Log] Before...");
    }
    @After("execution(* com.edlison.design.spring.aop.style_anno.LoginServiceImpl.*(..))")
    public void after() {
        System.out.println("[Log] After...");
    }
    @Around("execution(* com.edlison.design.spring.aop.style_anno.LoginServiceImpl.*(..))")
    public void around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("[Log] Point Start");
        joinPoint.proceed();
        System.out.println("[Log] Point End");
        System.out.println("joinPoint.getSignature() = " + joinPoint.getSignature());
    }
}
```

`@Aspect`实际上是引用的`org.aspectj`包。

切面类`@Component`注册为`Bean`。



# 问题

- 执行先后顺序与视频中的不一样，与Python装饰器的不一样，使用配置类的原因？
- `@Component`与`@Bean`重复？

