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

### 8. Проверить все плагины и получить рекомендации по их обновлению

```bash
mvn versions:display-plugin-updates
```

```bash
~/laboratory/hibernate-lab  $ mvn versions:display-plugin-updates
[INFO] Scanning for projects...
[INFO]
[INFO] ---------------------< org.example:hibernate-lab >----------------------
[INFO] Building hibernate-lab 1.0-SNAPSHOT
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- versions:2.16.2:display-plugin-updates (default-cli) @ hibernate-lab ---
[INFO]
[INFO] All plugins with a version specified are using the latest versions.
[INFO]
[INFO] All plugins have a version specified.
[INFO]
[WARNING] Project does not define minimum Maven version required for build
[INFO] Plugins require minimum Maven version of: 3.0
[INFO]
[ERROR] Project does not define required minimum version of Maven.
[ERROR] Update the pom.xml to contain maven-enforcer-plugin to
[ERROR] force the Maven version which is needed to build this project.
[ERROR] See https://maven.apache.org/enforcer/enforcer-rules/requireMavenVersion.html
[ERROR] Using the minimum version of Maven: 3.0
[INFO]
[INFO] Require Maven 2.0 to use the following plugin updates:
[INFO]   maven-clean-plugin ................................... 3.1.0 -> 2.2
[INFO]   maven-compiler-plugin .............................. 3.8.0 -> 2.0.2
[INFO]   maven-deploy-plugin .................................. 2.8.2 -> 2.4
[INFO]   maven-install-plugin ................................. 2.5.2 -> 2.2
[INFO]   maven-jar-plugin ..................................... 3.0.2 -> 2.1
[INFO]
[INFO] Require Maven 2.0.2 to use the following plugin updates:
[INFO]   maven-site-plugin ............................. 3.7.1 -> 2.0-beta-7
[INFO]
[INFO] Require Maven 2.0.4 to use the following plugin updates:
[INFO]   maven-project-info-reports-plugin .................. 3.0.0 -> 2.0.1
[INFO]
[INFO] Require Maven 2.0.6 to use the following plugin updates:
[INFO]   maven-clean-plugin ................................... 3.1.0 -> 2.5
[INFO]   maven-deploy-plugin ................................ 2.8.2 -> 2.8.1
[INFO]   maven-install-plugin ............................... 2.5.2 -> 2.5.1
[INFO]   maven-jar-plugin ..................................... 3.0.2 -> 2.5
[INFO]   maven-project-info-reports-plugin .................. 3.0.0 -> 2.1.2
[INFO]   maven-resources-plugin ............................... 3.0.2 -> 2.6
[INFO]   maven-site-plugin .................................. 3.7.1 -> 2.0.1
[INFO]   maven-surefire-plugin ............................. 2.22.1 -> 2.4.3
[INFO]
[INFO] Require Maven 2.0.9 to use the following plugin updates:
[INFO]   maven-compiler-plugin ................................ 3.8.0 -> 3.2
[INFO]   maven-surefire-plugin .............................. 2.22.1 -> 2.17
[INFO]
[INFO] Require Maven 2.1.0 to use the following plugin updates:
[INFO]   maven-project-info-reports-plugin .................... 3.0.0 -> 2.2
[INFO]   maven-site-plugin .................................. 3.7.1 -> 2.1.1
[INFO]
[INFO] Require Maven 2.2.0 to use the following plugin updates:
[INFO]   maven-project-info-reports-plugin .................... 3.0.0 -> 2.6
[INFO]   maven-site-plugin .................................... 3.7.1 -> 3.0
[INFO]
[INFO] Require Maven 2.2.1 to use the following plugin updates:
[INFO]   maven-clean-plugin ................................. 3.1.0 -> 2.6.1
[INFO]   maven-compiler-plugin ................................ 3.8.0 -> 3.3
[INFO]   maven-deploy-plugin ......................................... 2.8.2
[INFO]   maven-install-plugin ........................................ 2.5.2
[INFO]   maven-jar-plugin ..................................... 3.0.2 -> 2.6
[INFO]   maven-project-info-reports-plugin .................... 3.0.0 -> 2.9
[INFO]   maven-resources-plugin ............................... 3.0.2 -> 2.7
[INFO]   maven-site-plugin ........................................... 3.7.1
[INFO]   maven-surefire-plugin ............................ 2.22.1 -> 2.22.2
[INFO]
[INFO] Require Maven 3.0 to use the following plugin updates:
[INFO]   maven-clean-plugin .......................................... 3.1.0
[INFO]   maven-compiler-plugin .............................. 3.8.0 -> 3.8.1
[INFO]   maven-deploy-plugin ............................. 2.8.2 -> 3.0.0-M2
[INFO]   maven-install-plugin ............................ 2.5.2 -> 3.0.0-M1
[INFO]   maven-jar-plugin ................................... 3.0.2 -> 3.2.0
[INFO]   maven-project-info-reports-plugin .................. 3.0.0 -> 3.2.2
[INFO]   maven-resources-plugin ............................. 3.0.2 -> 3.1.0
[INFO]   maven-site-plugin .................................. 3.7.1 -> 3.9.0
[INFO]   maven-surefire-plugin .......................... 2.22.1 -> 3.0.0-M5
[INFO]
[INFO] Require Maven 3.0.5 to use the following plugin updates:
[INFO]   maven-site-plugin ................................. 3.7.1 -> 3.11.0
[INFO]
[INFO] Require Maven 3.1.0 to use the following plugin updates:
[INFO]   maven-jar-plugin ................................... 3.0.2 -> 3.2.2
[INFO]   maven-resources-plugin ............................. 3.0.2 -> 3.2.0
[INFO]
[INFO] Require Maven 3.2.5 to use the following plugin updates:
[INFO]   maven-clean-plugin ................................. 3.1.0 -> 3.3.2
[INFO]   maven-compiler-plugin ............................. 3.8.0 -> 3.11.0
[INFO]   maven-deploy-plugin ................................ 2.8.2 -> 3.1.1
[INFO]   maven-install-plugin ............................... 2.5.2 -> 3.1.1
[INFO]   maven-jar-plugin ................................... 3.0.2 -> 3.3.0
[INFO]   maven-project-info-reports-plugin .................. 3.0.0 -> 3.5.0
[INFO]   maven-resources-plugin ............................. 3.0.2 -> 3.3.1
[INFO]   maven-site-plugin .............................. 3.7.1 -> 4.0.0-M12
[INFO]   maven-surefire-plugin ............................. 2.22.1 -> 3.2.3
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.157 s
[INFO] Finished at: 2023-12-15T21:57:01+02:00
[INFO] ------------------------------------------------------------------------
```
