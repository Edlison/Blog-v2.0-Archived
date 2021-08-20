---
title: 注解 - Start
categories:
- Notebook
tags:
- java
- 注解
---

# 元注解

Java内置了几个元注解

- `@Target`定义了**作用域**，可以使用的范围。
- `@Retention`定义了注解的**生命周期**。
- `@Document`定义注解将被包含在JavaDoc中。
- `@Inherited`说明子类可以**继承**父类的注解。



# 自定义注解

注解内可以规定要传哪些值。

```java
@Target(value = {ElementType.METHOD, ElementType.TYPE})
@Retention(value = RetentionPolicy.RUNTIME)
@interface MyAnnoPlus {
    String value();
    String name() default "edlison";
    int id() default -1;
    int age();
}
```

- `value`对应的可以不用写`value = ""`, 可以直接传值。
- `default`默认则可以不传值。
- 传值的顺序没有要求。



# 用处

注解可以添加在package, class, method, field上，相当于给他们**添加了额外的信息**，我们可以通过**反射机制**编程来实现对这些元数据的访问。