---
layout: post
title: Spring Java Configuration
date: 2020-04-07
category: spring
collection: spring
---

### 1. Lets start from `pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>io.github.sergpank</groupId>
  <artifactId>spring-java-config</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>spring-java-config</name>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>3.2.18.RELEASE</version> <!-- I use Spring 3 because on my job we can't migrate from this sh!t -->
    </dependency>
  </dependencies>

</project>

```

### 2. Create some sample java classes:

```java
// MessageProvider Interface:
package io.github.sergpank;

public interface MessageProvider {
  public String getMessage();
}

// ConfigurableMessageProvider Class:
package io.github.sergpank;

public class ConfigurableMessageProvider implements MessageProvider {
  private String message = "Default message";

  public ConfigurableMessageProvider() {
  }

  public ConfigurableMessageProvider(String message) {
    this.message = message;
  }

  public void setMessage(String message) {
    this.message = message;
  }

  @Override
  public String getMessage() {
    return message;
  }
}

// MessageRenderer Interface:
package io.github.sergpank;

public interface MessageRenderer {
  void render();
  void setMessageProvider(MessageProvider messageProvider);
  MessageProvider getMessageProvider();
}

// SysoutMessageRenderer Class:
package io.github.sergpank;

public class SysoutMessageRenderer implements MessageRenderer {
  private MessageProvider messageProvider;

  @Override
  public void render() {
    System.out.println(messageProvider.getMessage());
  }

  @Override
  public void setMessageProvider(MessageProvider messageProvider) {
    this.messageProvider = messageProvider;
  }

  @Override
  public MessageProvider getMessageProvider() {
    return messageProvider;
  }
}


```

### 3. Create Java Configuration (@Configuration):
```java
package io.github.sergpank.config;

import io.github.sergpank.ConfigurableMessageProvider;
import io.github.sergpank.MessageProvider;
import io.github.sergpank.MessageRenderer;
import io.github.sergpank.SysoutMessageRenderer;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SimpleJavaConfig {

  @Bean
  public MessageProvider messageProvider() {
    System.out.println("---> init message provider");
    return new ConfigurableMessageProvider("Java config example");
  }

  @Bean
  MessageRenderer messageRenderer() {
    System.out.println("---> init message renderer");
    SysoutMessageRenderer renderer = new SysoutMessageRenderer();
    renderer.setMessageProvider(messageProvider());
    return renderer;
  }
}
```

### 4. Verify that everything works fine:
```java
package io.github.sergpank.app;

import io.github.sergpank.MessageRenderer;
import io.github.sergpank.config.SimpleJavaConfig;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class AppJAVA {
  public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(SimpleJavaConfig.class);
    MessageRenderer renderer = ctx.getBean("messageRenderer", MessageRenderer.class);
    renderer.render();
  }
}

// ---> init message renderer
// ---> init message provider
// Java config example
```
