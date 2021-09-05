---
title: springboot-dubbo-nacos-demo
date: 2021-09-05 15:45:52
tags: [nacos, springboot, dubbo]
---

# Springboot-dubbo-nacos

## Library need

- Nacos-client (nacos server connection)
- Spring-boot-start (springboot application)
- Dubbo-spring-boot-starter (dubbo annotation/auto configuration)
- Dubbo (dubbo core)

## Modules

- springboot-dubbo-api (service interfaces)
- springboot-dubbo-consumer
- springboot-dubbo-producer

<!-- more -->
## Maven dependencies

1. Parent
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.4</version>
    </parent>

    <groupId>com.xw</groupId>
    <artifactId>springboot-dubbo-nacos-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot-dubbo-nacos-demo</name>
    <description>springboot-dubbo-nacos-demo</description>

    <packaging>pom</packaging>

    <modules>
        <module>springboot-dubbo-consumer</module>
        <module>springboot-dubbo-producer</module>
        <module>springboot-dubbo-api</module>
    </modules>

    <properties>
        <druid.version>1.2.6</druid.version>
        <dubbo.version>3.0.1</dubbo.version>
        <nacos.version>2.0.3</nacos.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo-spring-boot-starter</artifactId>
                <version>${dubbo.version}</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba.nacos</groupId>
                <artifactId>nacos-client</artifactId>
                <version>${nacos.version}</version>
            </dependency>
            <dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo</artifactId>
                <version>${dubbo.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>


</project>
```
2. Module-Producer
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.xw</groupId>
        <version>0.0.1-SNAPSHOT</version>
        <artifactId>springboot-dubbo-nacos-demo</artifactId>
    </parent>

    <artifactId>springboot-dubbo-producer</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot-dubbo-producer</name>
    <description>springboot-dubbo-producer</description>

    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.nacos</groupId>
            <artifactId>nacos-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo</artifactId>
        </dependency>
    </dependencies>
</project>
```
3. Module-Consumer
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <artifactId>springboot-dubbo-nacos-demo</artifactId>
        <groupId>com.xw</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>

    <artifactId>springboot-dubbo-consumer</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot-dubbo-consumer</name>
    <description>springboot-dubbo-consumer</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.nacos</groupId>
            <artifactId>nacos-client</artifactId>
        </dependency>

        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

## Configuration

1. Dubbo Producer
```yaml
spring:
  application:
    name: dubbo-nacos-producer
dubbo:
  scan:
    base-packages: com.xw.producer.service
  protocol:
    name: dubbo
    port: 9990
  registry:
    address: nacos://${nacos.server-address}:${nacos.port}/?username=${nacos.username}&password=${nacos.password}

nacos:
  server-address: 127.0.0.1
  port: 8848
  username: nacos
  password: nacos
```
2. Dubbo Consumer
```yaml
server:
  port: 81
spring:
  application:
    name: springboot-dubbo-consumer
dubbo:
  application:
    name: ${spring.application.name}
  registry:
    address: nacos://${nacos.server-address}:${nacos.port}/?username=${nacos.username}&password=${nacos.password}

nacos:
  server-address: 127.0.0.1
  port: 8848
  username: nacos
  password: nacos
```
## Practice

1. echo api-interface
```java
public interface EchoService {
    String echo(String something);
} 
```
2. producer implement
```java
package com.xw.producer.service.impl;

import com.xw.service.EchoService;
import org.apache.dubbo.config.annotation.DubboService;

@DubboService(version = "1.0.0")
public class EchoServiceImpl implements EchoService {

    @Override
    public String echo(String something) {
        return something;
    }

}
 
```
3. consumer remote invoke
```java
package com.xw.demo.controller;

import com.xw.service.EchoService;
import org.apache.dubbo.config.annotation.DubboReference;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/v1.0")
public class EchoController {

    @DubboReference(version = "1.0.0")
    EchoService echoService;

    @GetMapping("/echo")
    public String echo(@RequestParam("something") String something) {
        return echoService.echo(something);
    }
} 
```