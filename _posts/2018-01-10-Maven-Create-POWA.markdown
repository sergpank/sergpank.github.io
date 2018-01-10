---
layout: post
title: Create POWA (Plain Old Web Application) using Maven
date: 2018-01-10
---

## Вот небольшой рецепт по созданию POWA

---

### 1. Для начала, сгенерируем костяк проекта:

```
mvn archetype:generate

find the following archetype ---> maven-archetype-webapp
```

Мы получим пустой проект со следуюцим POM файлом:

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>sergpank</groupId>
  <artifactId>webapp</artifactId>
  <packaging>war</packaging>
  <version>0.1</version>
  <name>webapp Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <finalName>webapp</finalName>
  </build>
</project>
```

### 2. Теперь обновим версию библиотеки JUnit настроим проект на Java 8

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>sergpank</groupId>
  <artifactId>webapp</artifactId>
  <packaging>war</packaging>
  <version>0.1</version>
  <name>webapp Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <finalName>webapp</finalName>
    <plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.3</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

### 3. Настроим Jetty plugin

```
<project>
  [...]
  <build>
    <plugins>

      <plugin>
        <groupId>org.mortbay.jetty</groupId>
        <artifactId>maven-jetty-plugin</artifactId>
      </plugin>

    </plugins>
  </build>
  [...]
</project>
```

Теперь запустим Jetty `mvn jetty:run` и откроем `http://localhost:8080/webapp/`  
Мы должны увидеть `Hello World!` в окне браузера.

Следует учесть что maven ожидает найти корень веб-приложения в `src/main/webapp`, где сейчас находится только `index.jsp`:

```
<html>
<body>
<h2>Hello World!</h2>
</body>
</html>
```

А в `src/main/webapp/WEB-INF` находится минимально возможный `web.xml`:

```
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
</web-app>
```

### 4. Добавление простого сервлета

  - Для начала добавим новую зависимость:

```
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>servlet-api</artifactId>
  <version>2.5</version>
</dependency>
```

  - Далее создадим папку `src/main/java` и поместим в нее `SimpleServlet`:

```
package com.sergpank.web;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class SimpleServlet extends HttpServlet
{
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException
  {
    PrintWriter out = resp.getWriter();
    out.println("SimpleServlet executed!");
    out.flush();
    out.close();
  }
}
```

  - После чего зарегистрирует сервлет в `web.xml`:

```
<!DOCTYPE web-app PUBLIC
    "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
    "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>

  <display-name>Archetype Created Web Application</display-name>

  <servlet>
    <servlet-name>simple</servlet-name>
    <servlet-class>com.sergpank.web.SimpleServlet</servlet-class>
  </servlet>

  <servlet-mapping>
    <servlet-name>simple</servlet-name>
    <url-pattern>/simple</url-pattern>
  </servlet-mapping>

</web-app>
```

  - Теперь нужно пересобрать проект, и перезапустить Jetty:

```
mvn clean install
mvn jetty:run

curl http://localhost:8080/webapp/simple
-> SimpleServlet executed!
```