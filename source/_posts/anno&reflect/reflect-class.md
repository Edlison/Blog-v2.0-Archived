---
title: Java反射 - 获取及使用属性与方法
categories:
- Notebook
tags:
- java
- 反射
---

# 获取

```java
public class ReflectGet {
    public static void main(String[] args) throws NoSuchMethodException {
        Class c = Person.class;

        // 获得类名
        System.out.println("c.getName() = " + c.getName());
        System.out.println("c.getSimpleName() = " + c.getSimpleName());

        // 获得类的属性
        Field[] fields = c.getFields();  // public属性
        for (Field field : fields) {
            System.out.println("field = " + field);
        }
        fields = c.getDeclaredFields();  // 所有属性
        for (Field field : fields) {
            System.out.println("declaredField = " + field);
        }

        // 获得类的方法
        Method[] methods = c.getMethods();  // 本类及父类全部的public方法
        for (Method method : methods) {
            System.out.println("method = " + method);
        }
        methods = c.getDeclaredMethods();  // 本类的所有方法
        for (Method method : methods) {
            System.out.println("method = " + method);
        }
        Method method = c.getMethod("setAge", int.class);  // 需要指定形参的类型，因为可能会重载！！！
        System.out.println("method = " + method);

        // 获得构造器
        Constructor[] constructors = c.getConstructors();  // 获得public方法
        for (Constructor constructor : constructors) {
            System.out.println("constructor = " + constructor);
        }
        constructors = c.getDeclaredConstructors();  // 获得本类全部
        for (Constructor constructor : constructors) {
            System.out.println("constructor = " + constructor);
        }
        Constructor constructor = c.getConstructor(String.class, int.class);  // 获取指定构造器，同样需要传形参的class类，可能会重载！！！
        System.out.println("constructor = " + constructor);
    }
}
```





# 使用

```java
public class ReflectUse {
    public static void main(String[] args) throws IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException, NoSuchFieldException {
        // 拿到类的Class对象
        Class c = Person.class;

        // 获取构造器（在Java9后使用Class对象直接创建实例的方法已经被抛弃了）
        Constructor constructorA = c.getDeclaredConstructor(null);
        Constructor constructorB = c.getDeclaredConstructor(String.class, int.class);

        // 通过构造器创建实例
        Person person = (Person) constructorA.newInstance(null);
        System.out.println("person = " + person);
        Person personB = (Person) constructorB.newInstance("Edlison", 18);
        System.out.println("personB = " + personB);

        // 通过Class对象获取其中的方法
        Method setName = c.getMethod("setName", String.class);

        // 执行该方法： 执行的对象 形参
        setName.invoke(personB, "edlison");
        System.out.println("personB = " + personB);

        // 通过Class对象获取其中的属性
        Field age = c.getDeclaredField("age");
        age.setAccessible(true);  // 由于private限制 需要先设置Accessible才可以对其进行操作
        age.set(personB, 10);
        System.out.println("personB = " + personB);
    }
}
```

**注意**：

- 通过`invoke`执行获得的类的方法：(需要执行的对象，形参...)
- 通过`set`对类的属性进行赋值：(需要赋值的对象，形参)
- 如果属性或方法的可见性为`private`，可以通过设置`setAccessible(true)`来访问。

