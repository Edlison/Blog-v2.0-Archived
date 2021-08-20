---
title: Singleton单例模式
categories:
- Notebook
tags:
- design
- singleton
---

# 饿汉模式

在项目一开始就初始化对象。

```java
public class Hungry {
    private final static Hungry HUNGRY = new Hungry();
    private Hungry() {

    }
    public static Hungry getInstance() {
        return HUNGRY;
    }
}
```

使用私有的构造方式确保对象无法在外部被实例化。使用一个静态方法获取内部的静态实例。



**不足**：的是如果对象太大，会造成大量资源被占用。



# 懒汉模式

在需要时实例化对象。

```java
public class Lazy {
    private static Lazy LAZY;
    private Lazy() {

    }
    public static Lazy getInstance() {
        if (LAZY == null) {
            LAZY = new Lazy();
        }
        return LAZY;
    }
}
```

此时线程不安全



# 双重检验锁的懒汉模式

Double-Checked locking Lazy Mode

```java
public class LazyDCL {
    private volatile static LazyDCL LAZYDCL;
    private LazyDCL() {

    }
    public static LazyDCL getInstance() {
        if (LAZYDCL == null) {
            synchronized (LazyDCL.class) {
                if (LAZYDCL == null) {
                    LAZYDCL = new LazyDCL();
                }
            }
        }
        return LAZYDCL;
    }
}
```

此时线程安全，但**还是有问题**，`new LazyDCL()`的时候不是**原子操作**。

实例化需要经过的步骤：

1. 分配内存空间
2. 执行构造方法 初始化对象
3. 把这个对象指向这个空间

也就会在底层，CPU执行时发生**指令重排**的错误。

1->2->3（正确）

1->3->2

如果有以上的顺序，当线程A执行到3步骤，此时如果有线程B进来，则会误判`LAZYDCL==null`从而直接返回。由于线程A并没有完成实例化的操作，因此可能会发生错误。

**注意**：此时必须对`LAZYDCL`加上`volatile`从而避免指令重排而产生的错误！！！



# 终极懒汉模式

## 1.0

**破坏方式**：通过**反射**可以强行改变构造器的可视性。以此再来新建一个对象。

```java
public class UltimateLazy {

    private volatile static UltimateLazy ULTIMATELAZY;

    private UltimateLazy() {
        synchronized (UltimateLazy.class) {
            if (ULTIMATELAZY != null) {
                throw new RuntimeException("error");
            }
        }
    }

    private static UltimateLazy getInstance() {
        ...
    }
}
```

**解决方案**：但是可以再在私有的构造方法里**再加一层判断**。



## 2.0

**破坏方式**：如果**都使用反射**来破坏单例模式。

```java
public class UltimateLazy {

    private volatile static UltimateLazy ULTIMATELAZY;

    private UltimateLazy() {
        synchronized (UltimateLazy.class) {
            if (flag == false) {
                flag = true;
            } else {
                throw new RuntimeException("error");
            }
        }
    }

    private static UltimateLazy getInstance() {
        ...
    }
}
```

**解决方案**：可以加一个标志位来确保单例。



## 3.0 终极解决方案

**破坏方式**：使用**反射**来改变标志位的值。

```java
public enum EnumLazy {
    INSTANCE;

    public EnumLazy getInstance() {
        return INSTANCE;
    }
}

class Test {
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        EnumLazy instanceA = EnumLazy.INSTANCE;
        Constructor<EnumLazy> constructor = EnumLazy.class.getDeclaredConstructor(String.class, int.class);
        constructor.setAccessible(true);
        EnumLazy instanceB = constructor.newInstance();

        System.out.println(instanceA == instanceB);
    }
}
```

**解决方案**：使用单例模式来解决。看源码可以得知，**不能使用反射来构建枚举类**。



----

Reference

https://www.bilibili.com/video/BV1K54y197iS

