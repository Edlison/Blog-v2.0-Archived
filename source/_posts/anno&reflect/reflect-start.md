---
title: 反射 - Start
categories:
- Notebook
tags:
- java
- 反射
---

# 介绍

Java是静态语言。

反射机制的引入，使Java能在执行期借助反射API**获取任何类**的内部信息，并能直接操作任何类的内部属性及方法。

正常方法：

引入'包类'名称 -> new实例化 -> 获得实例化对象

反射方法：

实例化对象 -> `getClass()`方法 -> 得到'包类'名称

**优点**：实现动态创建对象和编译。

**缺点**：性能有影响。



# 获得反射对象

```java
Class c1 = Class.forName("com.edlison.design.v1.reflection.User");
Class c2 = Class.forName("com.edlison.design.v1.reflection.User");

System.out.println(c1 == c2);  // true
```

- **一个类**在内存中只有**一个**`Class`对象
- 类被加载后，类的**所有信息**都封装在`Class`对象中

## 1. 通过对象获得

```java
Class c1 = person.getClass();
```

## 2. 通过`forName`获得

```java
Class c2=Class.forName("com.edlison.design.v1.reflection.Student");
```

## 3. 通过类.class获得

```java
Class c3 = Student.class;
```

## 4. 内置类型的Type属性

```java
Class c4 = Integer.TYPE;
```

## 5. 通过子类获取父类

```java
Class c5 = Student.class.getSuperclass();
```



# 类的初始化

## 过程

加载 -> 链接 -> 初始化

- 类的主动引用（一定会发生类的初始化）
- 类的被动引用（不会发生类的初始化）

结合JVM进行理解。



# 类加载器

类加载器是用来把类(Class)装载进内存的，JVM自顶向下定义了如下加载器。

- 引导类加载器：C++编写，负责Java核心库，**类加载器获取不到**。
- 扩展类加载器：负责`jre/lib/ext`下的Jar包的导入。
- 系统类加载器：负责`java --classpath`所指目录下类与Jar包的导入。

系统类加载器 -父类> 扩展类加载器 -父类> 引导类加载器

可以依次根据`ClassLoader.getParent()`取得其父类。

**注意**：

- 只有类加载器加载的路径下的Jar包才可以使用。
- 双亲委派机制。



