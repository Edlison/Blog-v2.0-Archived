---
title: Idea编译Java版本出错
categories:
- Notebook
- Error
tags:
- java
- idea
---

# Idea控制Java版本

## 1. 包管理中

- Project

- Modules

两个地方都有设置Java版本的位置。

## 2. 偏好设置中

`Cmd` + `,`

在`Java Compiler`中也可以指定Java版本

# Maven控制Java版本

如果是Maven项目，很有可能是因为Maven默认的Java的版本而出错。

在`pom.xml`中进行配置

```xml
<build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>11</source>
                    <target>11</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

只要刷新Maven项目，就可以解决Java版本问题。

