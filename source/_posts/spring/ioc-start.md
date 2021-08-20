---
title: SpringIOC - Start
categories:
- Notebook
tags:
- java
- spring
- ioc
---

# IOC的本质

![image-20210201102417224](/Users/edlison/Library/Application Support/typora-user-images/image-20210201102417224.png)

![image-20210201102430827](/Users/edlison/Library/Application Support/typora-user-images/image-20210201102430827.png)

![image-20210201102454419](/Users/edlison/Library/Application Support/typora-user-images/image-20210201102454419.png)

# IOC创建对象的方式

使用配置文件设置变量的值。将类注册为 `Bean`，通过`ApplicationContext`获取上下文。再用上下文取出`Bean`。



## 变量赋值

简单的通过Setter方法赋值。

1. 直接赋值

```xml
<bean id="user" class="com.edlison.design.spring.ioc.pojo.UserDAO">
		<property name="username" value="this is username"/>
</bean>
```

2. 引用

```xml
<bean id="userService" class="com.edlison.design.spring.ioc.service.UserService">
        <property name="userDAO" ref="user"/>
</bean>
```

## 创建对象

默认是无参构造。如果有构造函数，则有以下三种构造方式。

1. 通过下标构造

```xml
<bean id="user" class="com.edlison.design.spring.ioc.pojo.UserDAO">
        <constructor-arg index="0" value="this is username by constructor"/>
</bean>
```

2. 通过类型构造
3.  直接通过参数名

```xml
<bean id="user" class="com.edlison.design.spring.ioc.pojo.UserDAO">
        <constructor-arg name="username" value="this is username by constructor"/>
</bean>
```

# 总结

在配置文件加载的时候，容器里的实例就已经被创建了，并且内存中**仅有一个实例**。

