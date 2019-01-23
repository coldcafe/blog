---
title: MAC系统下vscode搭建java开发环境
---

1. 安装jdk 1.8。
2. 安装maven，直接使用Homebrew安装。
```shell
  brew install maven
```
3. 安装vscode，以及相关插件。
* Language Support for Java(TM) by Red Hat
* Debugger for Java
* Maven for Java
* Java Test Runner
4. maven配置私服。
MAC系统，maven的仓库目录默认在~/.m2下。
maven配置，我们需要在~/.m2下添加settings.xml文件。
示例：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

        <localRepository>/Users/lihuikang/.m2/repository</localRepository>

        <pluginGroups>
        </pluginGroups>
        <proxies>
        </proxies>
        <servers>
                <server>
                        <id>nexus</id>
                        <username>********</username>
                        <password>********</password>
                </server>
        </servers>
        <mirrors>
                <mirror>
                        <id>nexus</id>
                        <mirrorOf>*</mirrorOf>
                        <name>maven-public</name>
                        <url>你的私有仓库URL</url>
                </mirror>
        </mirrors>
  <profiles>
      <profile>
        <id>nexus</id>
        <activation>
                <activeByDefault>true</activeByDefault>
                <jdk>1.8</jdk>
        </activation>
        <properties>
                <maven.compiler.source>1.8</maven.compiler.source>
                <maven.compiler.target>1.8</maven.compiler.target>
                <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
        </properties>
        <repositories>
                <repository>
                        <id>nexus</id>
                        <name>local private nexus</name>
                        <url>你的私有仓库URL</url>
                        <releases>
                                <enabled>true</enabled>
                        </releases>
                        <snapshots>
                                <enabled>false</enabled>
                        </snapshots>
                </repository>
        </repositories>
        <pluginRepositories>
                <pluginRepository>
                        <id>releases</id>
                        <name>local private nexus</name>
                        <url>你的私有仓库URL</url>
                        <releases>
                                <enabled>true</enabled>
                        </releases>
                        <snapshots>
                                <enabled>true</enabled>
                        </snapshots>
                </pluginRepository>
        </pluginRepositories>
      </profile>
  </profiles>
        <activeProfiles>
                <activeProfile>nexus</activeProfile>
        </activeProfiles>
</settings>
```
5. java应用日志输出调成使用terminal输出。(根据个人喜好设置)
修改项目的.vscode目录下的launch.json文件，添加 "console": "integratedTerminal"。
示例
```json
{
  "configurations": [
    {
      "type": "java",
      "name": "CodeLens (Launch) - Application",
      "request": "launch",
      "mainClass": "com.***.Application",
      "projectName": "saas",
      "console": "integratedTerminal",
      "env": {
        "SAAS_ENV": "DEV"
      }
    }
  ]
}
```

### 现在开始你的JAVA编程之旅吧！
