---
layout: post
title: Maven Commands and Tuning
date: 2018-01-09
---

## Подборка команд для запуски приложения из консоли

---

### 1. Запустить скомпилированный проект (сработает только с примитивными сборками)

`java -cp application.jar org.sonatype.Main`

---

### 2. Чтобы сбилдить проект, даже если некоторые unit-тесты падают

`mvn test -Dmaven.test.failure.ignore=true`

или

```
<project>
    [...]
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <testFailureIgnore>true</testFailureIgnore>
                </configuration>
            </plugin>
        </plugins>
    </build>
    [...]
</project>
```

---

### 3. Сбилдить проект без выполнения unit-тестов

`mvn install -Dmaven.test.skip=true`

или

```
<project>
    [...]
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <skip>true</skip>
                </configuration>
            </plugin>
        </plugins>
    </build>
    [...]
</project>
```

---

### 4. Быстро запустить собранный проект

```
mvn clean install

mvn exec:java -Dexec.mainClass=org.sonatype.mavenbook.weather.Main
```

---

### 5. Получить документацию по плагину и всем его целям
```
mvn help:describe -Dplugin=<plugin-name> -Dfull

<-Dfull> - опциональный параметр, выдает, пожалуй, слишком много подробностей.
```

---

### 6. Работа с зависимостями

`mvn dependency:tree` - отобразить дерево зависимостей

`mvn dependency:resolve` - получить все зависимости из репозитория

`mvn install -X` - получить детальную информацию о работе управления зависомостями (в процессе сборки)

Добавить зависимость для тестирования:
```
<project>
    ...
    <dependencies>
        ...
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-io</artifactId>
            <version>1.3.2</version>
            <scope>test</scope>
        </dependency>
        ...
    </dependencies>
</project>
```

---

### 7. Собрать проект со всеми зависимостями

  * Объявить формат сборки `jar-with-dependencies`:

```no-highlight
<project>
  [...]
  <build>
    <plugins>
      <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
    </plugins>
  </build>
  [...]
</project>
```

  * После этого выполнить **assembly:assembly** goal:

`mvn install assembly:assembly`

  * И запустить собранный пакет

```
java -cp simple-weather-0.8-SNAPSHOT-jar-with-dependencies.jar org.sonatype.mavenbook.weather.Main
```

  * Однако правильнее будет делать эту сборку на **package** стадии, добавив элемент `execution` к `assembly` плагину, и получить аналогичный результат просто при помощи команды `mvn package` (или `mvn install`):

```
<plugin>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <descriptorRefs>
      <descriptorRef>jar-with-dependencies</descriptorRef>
    </descriptorRefs>
  </configuration>
  <executions>
    <execution>
      <id>simple-command</id>
      <phase>package</phase>
      <goals>
        <goal>attached</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

---

