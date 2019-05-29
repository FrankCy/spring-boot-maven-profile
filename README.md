# Spring Boot Maven Profile #
## 前言 ##
通过maven管理多环境，达到快速切换和部署效果。

## 实现步骤 ##
- 一、修改pom.xml
**```基于Spring Boot 2+ 实现，pom中会包含对应包```**

```xml
<!-- Boot 依赖 -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.3.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<dependencies>
    <!-- Boot Web 依赖 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!--spring freemarker依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-freemarker</artifactId>
    </dependency>
</dependencies>

<!-- 配置不同环境配置，这里设置了4个环境dev、sit、prd1、prd2 -->
<profiles>
    <profile>
        <id>dev</id>
        <properties>
            <profiles.active>dev</profiles.active>
            <maven.test.skip>true</maven.test.skip>
        </properties>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>
    <profile>
        <id>sit</id>
        <properties>
            <profiles.active>sit</profiles.active>
            <maven.test.skip>true</maven.test.skip>
        </properties>
    </profile>
    <profile>
        <id>prd1</id>
        <properties>
            <profiles.active>prd1</profiles.active>
            <maven.test.skip>true</maven.test.skip>
            <scope.jar>provided</scope.jar>
        </properties>
    </profile>
    <profile>
        <id>prd2</id>
        <properties>
            <profiles.active>prd2</profiles.active>
            <maven.test.skip>true</maven.test.skip>
            <scope.jar>provided</scope.jar>
        </properties>
    </profile>
</profiles>

<!-- 添加Build，用于动态读取*.yml -->
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <filtering>true</filtering>
            <excludes>
                <exclude>application-dev.yml</exclude>
                <exclude>application-sit.yml</exclude>
                <exclude>application-prd1.yml</exclude>
                <exclude>application-prd2.yml</exclude>
            </excludes>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <filtering>true</filtering>
            <includes>
                <!-- ${profiles.active}对应上面的profiles中的profiles.active节点 -->
                <include>application-${profiles.active}.yml</include>
                <include>application.yml</include>
            </includes>
        </resource>
    </resources>
</build>

<!--注意： 这里必须要添加，否则各种依赖有问题 -->
<repositories>
    <repository>
        <id>spring-milestones</id>
        <name>Spring Milestones</name>
        <url>https://repo.spring.io/libs-milestone</url>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
```

- 二、编写对应配置
对应编写application-dev、application-sit、application-prd1、application-prd2四个配置文件

**```完成```**