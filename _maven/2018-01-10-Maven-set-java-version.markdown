---
layout: post
title: Maven use Java 8 (or any other specific version) for project compilation and execution
date: 2018-01-10
---

## There are 2 ways for setting compiler `source` and `target` java versions:

### 1. Configuring corresponding project properties:

```
<project>
  [...]
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
  [...]
</project>
```

### 2. Configuring **compile** plugin:

```
<project>
  [...]
  <build>
    [...]
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.7.0</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>
    [...]
  </build>
  [...]
</project>
```

Default java version for compiler plugin is **1.5**  
More details in [documentation](https://maven.apache.org/plugins/maven-compiler-plugin/compile-mojo.html#source).