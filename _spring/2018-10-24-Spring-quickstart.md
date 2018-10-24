---
layout: post
title: Spring quickstart
date: 2018-10-24
category: spring
collection: spring
---

## Primitive project using context.xml

### 1. Initialize mvn quickstart project and get the following `pom.xml`

```
mvn archetype:generate
...
bla bla bla ... more in the Maven section ---> https://sergpank.github.io/maven/Maven-Quickstart
...

TADA!
```

```
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.sergpank.test</groupId>
  <artifactId>spring.playground</artifactId>
  <version>0.1</version>

  <name>spring.playground</name>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>8</maven.compiler.source>
    <maven.compiler.target>8</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.0.7.RELEASE</version>
    </dependency>
  </dependencies>

</project>
```

### 2. Create `app-context.xml` in `src\main\resources` directory

```
<?xml version="1.0" encoding="UTF-8" ?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="provider" class="com.sergpank.test.MessageProvider"/>

    <bean id="renderer" class="com.sergpank.test.MessageRenderer">
        <constructor-arg name="provider" ref="provider"/>
    </bean>

</beans>
```

### 3. Java source files should be located at `src/main/java` directory
```
package com.sergpank.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App 
{
    public static void main( String[] args )
    {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("app-context.xml");

        MessageRenderer renderer = ctx.getBean("renderer", MessageRenderer.class);
        renderer.render();
    }
}
```

```
package com.sergpank.test;

public class MessageRenderer
{
    private MessageProvider provider;

    public MessageRenderer(MessageProvider provider)
    {
        this.provider = provider;
    }

    public void render()
    {
        System.out.println(provider.getMessage());
    }
}
```

```
package com.sergpank.test;

public class MessageProvider
{
    public String getMessage()
    {
        return "Hello Spring ! ! !";
    }
}
```

### 4. Now run the `App` class and enjoy the power of `Spring!` :
```
Oct 24, 2018 5:57:06 PM org.springframework.context.support.AbstractApplicationContext prepareRefresh
INFO: Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@544fe44c: startup date [Wed Oct 24 17:57:06 EEST 2018]; root of context hierarchy
Oct 24, 2018 5:57:06 PM org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
INFO: Loading XML bean definitions from class path resource [app-context.xml]

Hello Spring ! ! !

Process finished with exit code 0
```
