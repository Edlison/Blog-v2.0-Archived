---
title: Install Neo4j on linux by Docker
categories:
- Docker
tags:
- linux
- neo4j
---

# Java Environment

Make sure the java environment on you server is updated to `java11`, if you are going to install neo4j-4.0 or above.

```
sudo apt install openjdk-11-jre-headless
```



# docker-compose

```
version: '3'
services:

  neo4j:
    image: neo4j
    volumes:
      - ./conf:/var/lib/neo4j/conf
      - ./mnt:/var/lib/neo4j/import
      - ./plugins:/plugins
      - ./data:/data
      - ./logs:/var/lib/neo4j/logs
    restart: always
    ports:
      - 7474:7474
      - 7687:7687
    environment:
      - NEO4J_dbms_memory_heap_maxSize=2G
      - NEO4J_AUTH=neo4j/123456  #修改默认用户密码
```







---

https://blog.csdn.net/Blanchedingding/article/details/89890663