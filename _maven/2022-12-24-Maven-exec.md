---
layout: post
title: Maven Exec
description: Run Java Main Method with maven-exec plugin
date: 2022-12-24
category: maven
collection: maven
---

Actually that is super easy:
```bash
mvn compile exec:java -Dexec.mainClass="com.serg.Main"

mvn compile exec:java -Dexec.mainClass="com.serg.Main" -Dexec.args="one two"

mvn compile exec:java -Dexec.mainClass="com.serg.Main" -Dexec.arguments="arg 1,arg 2"
```

To get some details about plugin (version and goals)
```bash
mvn help:describe -Dplugin=exec
```

Declare default Main class and arguments:
```xml
      <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>3.0.0</version>
            <configuration>
                <mainClass>com.serg.Main</mainClass>
                <arguments>
                    <argument>one</argument>
                    <argument>two</argument>
                    <argument>number three</argument>
                </arguments>
            </configuration>
      </plugin>
```

After that we can run exec plugin without explicit arguments:
```bash
mvn compile exec:java
```
