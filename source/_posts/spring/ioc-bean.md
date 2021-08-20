---
title: SpringIOC - Bean
categories:
- Notebook
tags:
- java
- spring
- ioc
- bean
---

# Bean Scope

![image-20210201120028673](/Users/edlison/Library/Application Support/typora-user-images/image-20210201120028673.png)

Bean默认的作用域是**单例模式**，也就是内存中只有一个实例。



# Bean的自动装配

自动装配是Spring满足Bean的依赖的一种方式。Spring会在上下文中寻找并自动给Bean装配属性。

Spring有三种装配方式

- xml中显式的装配
- java中显示的装配
- 隐式的**自动装配**

## 自动装配byName

要保证Bean的id唯一，Spring会在上下文中寻找和自己对象所需要Setter注入属性的相同id的Bean。

```xml
<bean name="peopleAutoByName" class="com.edlison.design.spring.ioc.pojo.People" autowire="byName"/>
```

## 自动装配byType

要保证Bean的类型唯一，Spring会在上下文中寻找自己对象需要Setter注入的值的类型相同的Bean。

```xml
<bean name="peopleAutoByType" class="com.edlison.design.spring.ioc.pojo.People" autowire="byType"/>
```

## 通过注解自动装配

首先引入规范

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

    <bean class="example.SimpleMovieCatalog" primary="true">
        <!-- inject any dependencies required by this bean -->
    </bean>

    <bean class="example.SimpleMovieCatalog">
        <!-- inject any dependencies required by this bean -->
    </bean>

    <bean id="movieRecommender" class="example.MovieRecommender"/>

</beans>
```

对需要自动装配的对象打上注解`@Autowired`

```java
public class People {
    @Autowired
    @Qualifier(value = "dog2")
    private Dog dog;

    @Autowired
    @Qualifier(value = "cat")
    private Cat cat;
}
```

`@Autowired`可以通过byType或byName的方式实现自动装配。



# 注解开发

使用注解首先要引入context约束，Spring4.0后还需要AOP的包。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
    <context:component-scan base-package="com.edlison.design.spring.ioc"/>

</beans>
```

## 1. Bean

```java
@Component
public class Role {

    @Value("admin")
    private String role_name;

    public String getRole_name() {
        return role_name;
    }
}
```

`@Component`相当于

```xml
<bean name="role" class="com.edlison.design.spring.ioc.pojo.Role"/>
```

**注意：**
注入后的`Bean`的name为相应的**驼峰**形式。

## 2. 属性的注入

```java
@Component
public class Role {

    @Value("admin")
    private String role_name;

    public String getRole_name() {
        return role_name;
    }
}
```

`@Value`相当于

```xml
<property name="username" value="This is username"/>
```

## 3. 衍生的注解

`@Component`有几个衍生注解，在Web开发中有MVC三层架构分层。

- `@Repository`DAO层
- `@Service`Service层
- `@Controller`Controller层

这些注解与`@Component`完全一致，只是Alias.

## 4. 自动装配

见上面

## 5. 作用域

```java
@Component
@Scope("prototype")
public class Role {

    @Value("admin")
    private String role_name;

    public String getRole_name() {
        return role_name;
    }
}
```

`@Scope`相当于

```xml
<bean id="role" class="com.edlison.design.spring.ioc.pojo.Role" scope="prototype">
```

## 6. 总结

- xml更万能适用于任何场合。维护简单方便。
- 注解不是自己的类使用不了，维护复杂。

最佳实践：

- xml用来管理Bean
- 注解用来注入属性



# 注解开发-完全舍弃XML

完全使用Java类实现配置。

**配置类**

```java
@Configuration
@ComponentScan("com.edlison.design.spring.ioc.pojo")
@Import(AppConfigPlus.class)
public class AppConfig {

    @Bean
    @Scope("prototype")
    public Student getStu() {
        return new Student();
    }

    @Bean
    public Card card() {
        return new Card();
    }
  
    @Autowired
    public Student student;
}
```

`@Configuration会使Spring容器托管，注册到容器中，它本身也有`@Component`。

`@Configuration`是一个配置类，相当于xml开发时的`applicationContext.xml`。

因此这个类可以配置各种在xml文件中的配置，比如`@ComponentScan`可以用来扫描指定包下的`@Component`，`@Import`可以导入其他配置类，导入其他配置类中注册的`Bean`。

`@Scope`可以对`Bean`的作用域做出规定。

还可以通过`@Autowired`直接对`Bean`进行装载。



**Bean对象**

```java
@Component
public class Student {
    @Value("edlison")  // 字符对象
    private String name;
    @Value("#{card}")  // 复杂类
    private Card card;
    @Autowired
    private Bike bike;
}
```

`@Value`可以对任何类型的属性进行注入。

也可以通过`@Autowired`对复杂类型进行自动装配。



**测试类**

```java
    @Test
    public void testConfig() {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        Student student = context.getBean("getStu", Student.class);
        System.out.println(student);
    }
```

完全使用配置类，需要通过`AnnotationConfig`上下文来获取容器，通过传入配置类的`class`对象加载。

也可以直接获取`config`类，通过其方法获取实例。



# 总结

Spring的IOC机制实质上就是一个容器，统一管理项目开发中的各种类型，可以很方便的控制其是单例模型还是远行模型。

**自动装配**机制很大的简化了**依赖注入**，可以直接匹配名称或类型，使得开发进一步简化。



# 问题

如果一个对象有一个复杂的数据成员对象，且该对象已经装配为`Bean`，也可以用`@Autowired`对其进行自动装配？与`@Value`功能/实质一样？

