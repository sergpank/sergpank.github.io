---
layout: post
title: Maven Quickstart
date: 2018-01-06
---

### 1. Чтобы созжать maven проект введите следующую команду и следуйте инструкциям:
`mvn archetype:generate`

### 2. Выберите archetype по умолчанию: 
`org.apache.maven.archetypes:maven-archetype-quickstart`

### 3. Параметры __groupId__, __artifactId__,  __version__, __package__ установите на свой вкус:
```
Confirm properties configuration:
groupId: quickstart
artifactId: experiment
version: 1.0-SNAPSHOT
package: com.quickstart.experiment
 Y: : y
```

**//TODO** написать что такое __maven coordinates__ и описать значение каждой из них

### 4. Сгенерирован проект следующей структуры:
```
experiment
|-src
| |-main
| | |-java
| |   |-com
| |     |-quickstart
| |       |-experiment
| |         |-App.java
| |-test
|   |-java
|     |-com
|       |-quickstart
|         |-experiment
|           |-AppTest.java
|-pom.xml
```

### 5. __pom.xml__ выглядит следующим образом:
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>quickstart</groupId>
  <artifactId>experiment</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>experiment</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

Это лишь верхушка айсберга конфигурации, полная конфигурация содержится в __effective pom__'e. Выполнение команды `mvn help:effective-pom` выведет его в консоль.

### 6. Для того чтобы скомпилировать проект выпилните следующую команду:
`mvn install`

Будет сгенерирована папка **target** в которой кроме всего прочего находится скомпилированный jar файл **experiment-1.0-SNAPSHOT.jar**:
```
experiment
|-src
| |-...
|-target
| |-experiment-1.0-SNAPSHOT.jar
| |-...
|-pom.xml
```

### 7. Теперь давайте запустим сгенерированный файл:
```
> java -cp experiment-1.0-SNAPSHOT.jar com.quickstart.experiment.App

Hello World!
```

Скомпилированный проект успешно запустился.