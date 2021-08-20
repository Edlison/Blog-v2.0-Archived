---
title: SpringIOC - Dependency Injection
categories:
- Notebook
tags:
- java
- spring
- ioc
---

# 依赖注入

## 构造器注入

见上一篇

在创建对象的时候注入。



## Setter方法注入

可以注入任何的类型，如Bean(复杂对象)，一般类型，集合等数据结构。

### String

```xml
<property name="username" value="This is username"/>
```

### Bean

```xml
<property name="address" ref="address"/>
```

### Array

```xml
<property name="books">
            <array>
                <value>Java</value>
                <value>Python</value>
                <value>Scala</value>
            </array>
</property>
```

### List

```xml
<property name="hobbies">
            <list>
                <value>movies</value>
                <value>tennis</value>
            </list>
</property>
```

### Set

```xml
<property name="games">
            <set>
                <value>SC2</value>
                <value>LOL</value>
                <value>CSGO</value>
            </set>
        </property>
```

### Map

```xml
<property name="cards">
            <map>
                <entry key="idcard" value="320325"/>
                <entry key="stucard" value="1801"/>
            </map>
</property>
```

### Properties

```xml
<property name="info">
            <props>
                <prop key="Title">CEO</prop>
            </props>
</property>
```

### Null

```xml
<property name="wife">
            <null>null_value</null>
</property>
```

### 注意

发现对Array或List赋值，标签`<array>`和`list`可以互换，没有影响。



## 扩展方式注入

### P命名空间

首先引入规范

```xml
xmlns:p="http://www.springframework.org/schema/p"
```

其实就对应Setter方法注入，只不过简化了，使用`p:`.

```xml
<bean id="userServicePlus" class="com.edlison.design.spring.ioc.service.UserService" p:userDAO-ref="user" p:userServiceLog="This is a log."/>
```

### C命名空间

首先引入规范

```xml
xmlns:c="http://www.springframework.org/schema/c"
```

其实就对应构造器注入，只不过简化了，使用`c:`.

使用有参构造。

```xml
<bean id="addressPlus" class="com.edlison.design.spring.ioc.pojo.Address" c:city="Nanjing"/>
```



