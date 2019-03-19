---
layout: post
title: Spring Cache (with Redis | EhCache | Memcahed)
date: 2018-10-24
category: spring
collection: spring
---

## 0. The very first is `pom.xml`:
```xml
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
        <!--REDIS-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.3.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-redis</artifactId>
            <version>1.8.15.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>2.9.0</version>
        </dependency>

        <!--EHCACHE-->
        <dependency>
            <groupId>net.sf.ehcache</groupId>
            <artifactId>ehcache</artifactId>
            <version>2.8.3</version>
        </dependency>

        <!--MEMCACHED-->
        <dependency>
            <groupId>com.google.code.simple-spring-memcached</groupId>
            <artifactId>spring-cache</artifactId>
            <version>3.1.0</version>
        </dependency>
        <dependency>
            <groupId>com.google.code.simple-spring-memcached</groupId>
            <artifactId>xmemcached-provider</artifactId>
            <version>3.1.0</version>
        </dependency>
        <dependency>
            <groupId>com.google.code.simple-spring-memcached</groupId>
            <artifactId>spymemcached-provider</artifactId>
            <version>3.1.0</version>
        </dependency>

        <!--LOGGING-->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-slf4j-impl</artifactId>
            <version>2.11.1</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-api</artifactId>
            <version>2.11.1</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.11.1</version>
        </dependency>
        <dependency>
            <groupId>com.lmax</groupId>
            <artifactId>disruptor</artifactId>
            <version>3.4.2</version>
        </dependency>
    </dependencies>

</project>
```

## 1. And here is `app-context.xml` (key and value serializers are not necesary but String are more readable than default bytearrays):
```xml
<?xml version="1.0" encoding="UTF-8" ?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:cache="http://www.springframework.org/schema/cache"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/cache
                           http://www.springframework.org/schema/cache/spring-cache.xsd">

    <context:component-scan base-package="com.sergpank.test" />

    <cache:annotation-driven />

    <!-- REDIS cache configuration begin -->
    <bean id='jedisConnectionFactory'
          class='org.springframework.data.redis.connection.jedis.JedisConnectionFactory'
    />
    <bean id="keySerializer"
          class="org.springframework.data.redis.serializer.StringRedisSerializer"
    />
    <bean id="valueSerializer"
          class="org.springframework.data.redis.serializer.StringRedisSerializer"
    />
    <bean id='redisTemplate'
          class='org.springframework.data.redis.core.RedisTemplate'
          p:connection-factory-ref='jedisConnectionFactory'
          p:keySerializer-ref='keySerializer'
          p:valueSerializer-ref='valueSerializer'
    />
    <bean id='cacheManager'
          class='org.springframework.data.redis.cache.RedisCacheManager'
          c:redisOperations-ref='redisTemplate'
          p:loadRemoteCachesOnStartup="true"
          p:defaultExpiration="1200"
    />
    <!-- REDIS cache configuration end -->


    <!-- EHCACHE cache configuration begin -->
    <!--<bean id="ehcache" class="org.springframework.cache.ehcache.EhCacheManagerFactoryBean">-->
        <!--<property name="configLocation" value="classpath:ehcache.xml"/>-->
        <!--<property name="shared" value="false"/>-->
    <!--</bean>-->
    <!--<bean id="cacheManager" class="org.springframework.cache.ehcache.EhCacheCacheManager">-->
        <!--<property name="cacheManager" ref="ehcache"/>-->
    <!--</bean>-->
    <!--EHCACHE cache configuration end -->


    <!-- MEMCACHED cache configuration begin -->
    <!--<bean name="cacheManager" class="com.google.code.ssm.spring.SSMCacheManager">-->
        <!--<property name="caches">-->
            <!--<set>-->
                <!--<bean class="com.google.code.ssm.spring.SSMCache">-->
                    <!--<constructor-arg name="cache" index="0" ref="testCache" />-->
                    <!--&lt;!&ndash; 20 minutes &ndash;&gt;-->
                    <!--<constructor-arg name="expiration" index="1" value="1200" />-->
                    <!--&lt;!&ndash; @CacheEvict(..., "allEntries" = true) doesn't work &ndash;&gt;-->
                    <!--<constructor-arg name="allowClear" index="2" value="false" />-->
                <!--</bean>-->
            <!--</set>-->
        <!--</property>-->
    <!--</bean>-->
    <!--<bean name="testCache" class="com.google.code.ssm.CacheFactory">-->
        <!--<property name="cacheName" value="testCache" />-->
        <!--<property name="cacheClientFactory">-->
            <!--<bean name="cacheClientFactory"-->
                  <!--class="com.google.code.ssm.providers.xmemcached.MemcacheClientFactoryImpl" />-->
        <!--</property>-->
        <!--<property name="addressProvider">-->
            <!--<bean class="com.google.code.ssm.config.DefaultAddressProvider">-->
                <!--<property name="address" value="127.0.0.1:11211" />-->
            <!--</bean>-->
        <!--</property>-->
        <!--<property name="configuration">-->
            <!--<bean class="com.google.code.ssm.providers.CacheConfiguration">-->
                <!--<property name="consistentHashing" value="true" />-->
            <!--</bean>-->
        <!--</property>-->
    <!--</bean>-->
    <!-- MEMCACHED cache configuration end -->

</beans>
```

## 2. Now lets create a Cached method:
```java
package com.sergpank.test;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Component;

@Component
public class CacheTest
{
    private static final Logger log = LogManager.getLogger(CacheTest.class);

    @Cacheable(value = "testCache", key = "'testCache-' + #root.args")
    public String testCache(int val)
    {
        log.info("Not cached - " + val);
        return Integer.toString(val);
    }
}
```

## 3. And a simple tester for all that stuff:
```java
package com.sergpank.test;

import java.util.stream.IntStream;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App
{
    private static final Logger log = LogManager.getLogger(App.class);

    public static void main(String[] args)
    {
        log.info("Test !!!");

        ApplicationContext ctx = new ClassPathXmlApplicationContext("app-context.xml");
        CacheTest test = ctx.getBean(CacheTest.class);

        log.info("Not cached! ...");
        IntStream.rangeClosed(0, 10).forEach(i -> log.info(test.testCache(i)));

        log.info("CACHED! ...");
        IntStream.rangeClosed(0, 10).forEach(i -> log.info(test.testCache(i)));

        System.exit(0);
    }
}
```

## 4. Don't forget about configuration files:
`ehcache.xml`:
```xml
<ehcache>
    <diskStore path="java.io.tmpdir"/>
    <cache name="testCache"
           maxElementsInMemory="100000"
           eternal="false"
           timeToIdleSeconds="1200"
           timeToLiveSeconds="1200"
           overflowToDisk="true"
           maxElementsOnDisk="10000000"
           diskPersistent="false"
           diskExpiryThreadIntervalSeconds="1800"
           memoryStoreEvictionPolicy="LRU"/>
</ehcache>
```

`log4j2.xml`:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration packages="com.opera.fnd.util">
    <Appenders>
        <Console name="stdout" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{ABSOLUTE} %5p %c{1}:%L %m%n" />
        </Console>
    </Appenders>
    <Loggers>
        <AsyncRoot level="info">
            <AppenderRef ref="stdout" />
        </AsyncRoot>
    </Loggers>
</Configuration>
```

## 5. Now you can comment out necessary cache provider and check if it works

Expected output:
```
15:46:13,573  INFO App: Test !!!
15:46:14,336  INFO App: Not cached! ...
15:46:14,411  INFO CacheTest: Not cached - 0
15:46:14,424  INFO App: 0
15:46:14,425  INFO CacheTest: Not cached - 1
15:46:14,425  INFO App: 1
15:46:14,426  INFO CacheTest: Not cached - 2
15:46:14,427  INFO App: 2
15:46:14,427  INFO CacheTest: Not cached - 3
15:46:14,428  INFO App: 3
15:46:14,428  INFO CacheTest: Not cached - 4
15:46:14,429  INFO App: 4
15:46:14,430  INFO CacheTest: Not cached - 5
15:46:14,430  INFO App: 5
15:46:14,431  INFO CacheTest: Not cached - 6
15:46:14,431  INFO App: 6
15:46:14,432  INFO CacheTest: Not cached - 7
15:46:14,433  INFO App: 7
15:46:14,434  INFO CacheTest: Not cached - 8
15:46:14,435  INFO App: 8
15:46:14,435  INFO CacheTest: Not cached - 9
15:46:14,436  INFO App: 9
15:46:14,436  INFO CacheTest: Not cached - 10
15:46:14,437  INFO App: 10
15:46:14,437  INFO App: CACHED! ...
15:46:14,440  INFO App: 0
15:46:14,440  INFO App: 1
15:46:14,441  INFO App: 2
15:46:14,442  INFO App: 3
15:46:14,442  INFO App: 4
15:46:14,443  INFO App: 5
15:46:14,443  INFO App: 6
15:46:14,444  INFO App: 7
15:46:14,444  INFO App: 8
15:46:14,445  INFO App: 9
15:46:14,445  INFO App: 10
```

Additinaly you need to have Redis and Memcached installed and running.  
No need to install EhCache as it runs in JVM with project itself.

Redis hints:
```
redis-server & -> start redis
redis-cli      -> redis commmand-line-interface
keys *         -> show all keys
flushall       -> clear redis cache
```

Memcached hints:
```
memcached &            -> start memecached
telnet 127.0.0.1 11211 -> connect to local memcached instance
stats items            -> show brief memecached stats
quit                   -> disconnect from memcached cli
```