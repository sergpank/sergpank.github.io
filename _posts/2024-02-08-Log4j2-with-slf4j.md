---
layout: post
title: Log4j2 with SLF4j Minimal Project
date: 2024-02-08
---

# How to configure Minimal Project with Log4j2 and SLF4j logging

It is very known annoying problem - when you are starting some POC or MVP project to quickly test or prove some technology/library/hypothesis 
you need to all logging.

But very few people can get the logger configuration right from the top of their head. Most of us get typical errors and spend several minutes 
fot googling and fixing the configuration/dependency issue.

Here is a quick, brief and complete istruction how to configure logging using Log4j2 and SLF4j in any scratch project.

1 . Create an empty project using `mvn archetype:generate`
  - Archetype `org.apache.maven.archetypes:maven-archetype-quickstart` will be just fine, and it is the default one.

2 . Add the following dependencies to your `pom.xml` file:
```xml
<dependencies>
    <!-- LOG4J -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.22.1</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.22.1</version>
    </dependency>
    <!-- The Log4j 2 SLF4J Binding allows applications coded to the SLF4J API to use Log4j 2 as the implementation.
         https://logging.apache.org/log4j/2.x/log4j-slf4j-impl.html
    -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-slf4j2-impl</artifactId>
        <version>2.22.1</version>
    </dependency>
    <!-- To fix error:
         ERROR StatusConsoleListener Could not create plugin of type class
         org.apache.logging.log4j.core.async.AsyncLoggerConfig$RootLogger
         for element AsyncRoot:
         java.lang.NoClassDefFoundError: com/lmax/disruptor/EventHandler
    -->
    <dependency>
        <groupId>com.lmax</groupId>
        <artifactId>disruptor</artifactId>
        <version>3.4.4</version>
    </dependency>

    <!-- SLF4J -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>2.0.10</version>
    </dependency>
</dependencies>
```

3 . Add `log42.xml` config file to `/src/main/resources` dir :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
    <Properties>
        <Property name="pattern">%d{ISO8601} %5p %c{1}:%L %m%n</Property>
    </Properties>

    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
            <PatternLayout pattern="${pattern}" />
        </Console>

        <RollingRandomAccessFile name="FILE"
                                 fileName="log/app.log"
                                 filePattern="log/app.%d{yyyy-MM-dd_HH-mm-ss}.log"
                                 append="true"
                                 bufferSize="1024"
                                 immediateFlush="true">
            <PatternLayout pattern="${pattern}" />
            <Policies>
                <OnStartupTriggeringPolicy />
            </Policies>
        </RollingRandomAccessFile>
    </Appenders>

    <Loggers>
        <AsyncRoot level="info">
            <AppenderRef ref="STDOUT" />
            <AppenderRef ref="FILE" />
        </AsyncRoot>
    </Loggers>
</Configuration>
```

4 . Now you can use logger:
```java
package laboratory;


import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class App
{
    private static final Logger log = LoggerFactory.getLogger(App.class);

    public static void main( String[] args )
    {
        log.info( "Hello Log!" );
    }
}
```

5 . And you shoud get the following log message in terminal and in log/app.log file:
```bash
2024-02-08T16:14:51,298  INFO App: Hello Log!
```

6 . Some working ecample can be found in this repository : <https://github.com/sergpank/log4j2-slf4j-logging-mvp/>
7 . Happy Logging!
