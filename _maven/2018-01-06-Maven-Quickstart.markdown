---
layout: post
title: Maven Quickstart
description: Translation and adaption of the official guide
date: 2018-01-06
category: maven
collection: maven
---

### 1. Чтобы создать maven проект введите следующую команду и следуйте инструкциям:
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

Это **maven coordinates** - набор индентификаторов которые вместе используются для создания уникального индетификатора проекта, зависимости или плагина в POM файле. Рассмотрим каждый из них по отдельности:

- **groupId** - Группа, компания, организация, проект или любая иная группа. Обычно это домен, написанный в обратном порядке. Например, проекты Apache будут иметь __groupId__ *org.apache*.

- **artifactId** - Уникальный индентификатор проекта внутри группы.

- **version** - Конкретная версия релиза проекта. Для каждого релиза фиксируется отдельная версия, которая соответствует определенной версии проекта. Проекты, находящиеся в фазе активной разработки, используют специальный индентификатор - **SNAPSHOT**.

- **package** - Корневой пакет данного пректа.

- **packaging** - Тип упаковки проекта, по умолчанию это *jar*. Хоть формат упакивки и является важным компонентом координат Maven, он не является частью его уникального индентификатора. Только **groupId.artifactId.version** составляют уникальный индентификатор проекта.

- **classifier** - Иногда вы будете встречать этот опциональный и произвольный параметр в координатах пректа. В полном виде такой проект будет иметь название вида **groupId:artifactId:packaging:classifier:version**. Classifier позволяет различать артефакты собранные из одного POM файла, но отличные по содержанию. Как пример изпользования данного элемента, давайте рассмотрим случай когда проект предназначен для JRE-8, но в то же время он должет продолжать поддержку JRE-6. Таким образом будут скомпилированы 2 артефакта с классификаторами _jdk18_ и _jdk16_, соответственно. Или же в случае если нам нужно прикрепить к основному артефакту вторичные. В таком случае мы добавляем к зависимости, например, пункт `classifier>sources</classifier>` или `<classifier>javadoc</classifier>`. Эти классификаторы позволяют загрузить в репозиторий и скачать из него исходники и документацию проекта.

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

### 6. Для того чтобы скомпилировать проект, выполните следующую команду:
`mvn install`

Будет сгенерирована папка **target** в которой, кроме всего прочего, находится скомпилированный jar файл **experiment-1.0-SNAPSHOT.jar**:
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
~> java -cp experiment-1.0-SNAPSHOT.jar com.quickstart.experiment.App

Hello World!
```

Наш тестовый проект успешно запустился.

---

Чтобы узнать все цели плагина выполните команду:

```mvn help:describe -Dplugin=install -Dfull=true```
