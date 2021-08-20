---
title: SpringAOP - 动态代理
categories:
- Notebook
tags:
- java
- spring
- aop
- proxy
---

# 引入

相较于静态代理模式，所有的方法动态生成。

这里介绍基于JDK的方式实现动态代理。



# 实例

```java
public class ProxyInvocationHandler implements InvocationHandler {
    
    // 被代理的接口
    private Object target;

    // 传入被代理的接口
    public void setTarget(Object target) {
        this.target = target;
    }

    // 生成代理类
    public Object getProxy() {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
    }

    // 唤醒方法 返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        Object result = method.invoke(target, args);
        return result;
    }

    // 定义唤起方法执行前后进行装饰的方法
    private void log(String msg) {
        System.out.println("[Log] " + msg);
    }
}
```

## Proxy

返回代理类，需要传入：

- 当前作为Proxy的`Class加载器`.
- 需要代理的`对象的接口`。
- 自定义的InvocationHandler，实现了`InvocationHandler`接口。

```java
public Object getProxy() {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
}
```

## InvocationHandler

唤起事务处理。

```java
@Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        Object result = method.invoke(target, args);
        return result;
    }
```

实现了`InvocationHandler`，需要重写`invoke`方法。

使用反射的方法执行`method.invoke`传入

- 被代理的接口(target)
- 执行时的形参(args)

在唤起执行**方法前后**，可以加入**自定义的事务处理**。



# 自定义

可以实现将一个`ProxyInvocationHandler`分成一个`CustomProxy`和一个`CustomInvocationHandler`。

## CustomInvocationHandler

```java
public class CustomInvocationHandler implements InvocationHandler {

    private IndexService indexService;

    public CustomInvocationHandler(IndexService indexService) {
        this.indexService = indexService;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  // TODO Proxy的作用？
        logBefore(method.getName());
        Object res = method.invoke(indexService, args);
        logAfter(indexService.getClass().getName());
        return res;
    }

    // 以下定义 前后装饰方法
    private void logBefore(String msg) {
        System.out.println("[Log] " + msg);
    }

    private void logAfter(String msg) {
        System.out.println("[Log] " + msg + "has been done!");
    }
}
```

## CustomProxy

```java
public class CustomProxy {

    public Object getProxy(IndexService indexService, InvocationHandler invocationHandler) {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), indexService.getClass().getInterfaces(), invocationHandler);
    }
}
```

## Client

```java
public class CustomClient {
    public static void main(String[] args) {
        // 需要代理的接口
        IndexService indexService = new IndexServiceImpl();
        // 实例化 自定义代理接口需要进行的处理类
        CustomInvocationHandler customInvocationHandler = new CustomInvocationHandler(indexService);
        // 实例化 自定义的代理类
        CustomProxy customProxy = new CustomProxy();
        // 获取代理 需要传入代理的接口及自定义的处理类
        IndexService proxy = (IndexService) customProxy.getProxy(indexService, customInvocationHandler);
        // 执行测试
        proxy.add();
    }
}
```

`Proxy`与`InvocationHandler`可以分离。

- `InvocationHandler`必须要传需要代理的接口。
- `Proxy`必须要传需要代理的接口，以及自定义的`Handler`。



# 问题

`Proxy`与`InvocationHandler`可以解藕，但是他们都需要被代理的接口。

进行**接口实现接口**的设计？

`InvocationHandler`中实现了`invoke`后其中传的`Proxy`的作用？