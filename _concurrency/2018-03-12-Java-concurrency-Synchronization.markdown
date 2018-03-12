---
layout: post
title: Java Concurrency Synchronization
description: Java core concurrency in brief
date: 2018-03-12
category: concurrency
collection: concurrency
---

## 1. При параллельном взаимодеиствии нескольких потоков с обшими ресурсами, возникает множество проблем:

+ **Race Conditions** - Случается когда разные потоки несинхронно модифицируют один и тот же ресурс, что приводит к тому что модифицируемый ресурс попадает в неправильное состояние:

```java
if (a == 10.0)
{
  b = a / 2.0;
}
```

+ **Data Races** - Случается когда несколько потоков пишут данные в один ресурс без синхронизации:

```java
private static Parser parser;

public static Parser getInstance()
{
  if (parser == null)
  parser = new Parser();
  return parser;
}
```

+ **Cached Variables** - Из соображений производительности, значение переменной может храниться не в оперативной памяти а в регистрах процессора, в таком случае у разных потоков значение одной и той же переменной может отличаться.