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

## 2. Все эти проблемы можно решить при помощи синхронизации.

### Синхронизировать можно отделыные блоки кода.
В этом случае нужно указать какой объект следует использовать в качестве лока:
```java
class IdGenerator {
  private Integer id = 0;
  
  public Integer getId() {
    synchronized(id) {
      id++;
    }
    return id;
  }
}
```

### А можно и методы целиком:
В таком случае лок будет привязан к объекту в котором вызван метод:
```java
class IdGenerator {
  private Integer id = 0;
  
  public synchronized Integer getId() {
    id++;
    return id;
  }
}  
```

## 3. Однако синхронизация не может решить всех проблем взаимодействия поток, напротив, она подеидывает нам еще несколько новых:
+ **Deadlock** - Поток А ждет пока поток Б разблокирует один ресурс, в то время как поток Б ждет пока поток А разблокирует другой ресурс. В результате ни один из поток ничего не делает.
+ **Livelock** - Поток пытается выполнить операцию, которая постоянно выдает ошибку. В результате поток ничего не делает.
+ **Starvation** - Поток с низким приоритетом не запускается, потому что все ресурсы забрали потоки с большим приоритетом, и планировщик задач никак не запустит поток с низким приоритетом.

Стоит правда отметить, что в двух последних сучаях синхронизация ни при чем.

Но давайте вернемся к dealock'у. Эта проблема возникает из-за неразумной оверсинхронизации. Вот вам классический пример:
```java
public class DeadlockDemo
{
   private final Object lock1 = new Object();
   private final Object lock2 = new Object();

   public void instanceMethod1()
   {
      synchronized(lock1)
      {
         System.out.println("A :: lock1 hodl");
         synchronized(lock2)
         {
            System.out.println("A :: lock2 hodl");
            // critical section guarded first by
            // lock1 and then by lock2
         }
         System.out.println("A :: lock2 release");
      }
      System.out.println("A :: lock1 release");
   }

   public void instanceMethod2()
   {
      synchronized(lock2)
      {
         System.out.println("B :: lock2 hodl");
         synchronized(lock1)
         {
            System.out.println("B :: lock1 hodl");
            // critical section guarded first by
            // lock2 and then by lock1
         }
         System.out.println("A :: lock1 release");
      }
      System.out.println("A :: lock2 release");
   }

   public static void main(String[] args)
   {
      final DeadlockDemo dld = new DeadlockDemo();

      Runnable r1 = () -> {
         while(true)
         {
            dld.instanceMethod1();
            try
            {
               Thread.sleep(50);
            }
            catch (InterruptedException ie)
            {
            }
         }
      };

      Runnable r2 = () -> {
         while(true)
         {
            dld.instanceMethod2();
            try
            {
               Thread.sleep(50);
            }
            catch (InterruptedException ie)
            {
            }
         }
       };

      Thread thdA = new Thread(r1);
      Thread thdB = new Thread(r2);
      thdA.start();
      thdB.start();
   }
}

/*
  Sample output:

  B :: lock2 hodl
  A :: lock1 hodl
*/
```

Из примера прекрасно видно, что поток-Б захватывает лок2, после чего поток-А захватывает лок1 ... и ВСЁ! Больше никто никуда не идёт. После этого приложение попадает в состояние deadlock'a и его выполнение прекращается. 

Примерно такаяже ситуация у Молдовы с Приднестровьем, у Украины с Донбассом, у Кипра с Северным Кипром, у северных корейцев с южными, у России с Японией Курилы и далее везде. Поэтому deadlock очень жизненная ситуация. И для того чтобы исправить эту ошибку тебе придется изрядно попотеть, друг мой.

## 4. Volatile

Поверь мне на слово, что каждое ядро процессора может сожержать копию значения переменной, и иногда может возникнуть ситуация при которой изменение значения переменной в одном потоке, не отразится в другом. Потому что у каждого ядра закешированы свои значения переменной.

В таком случае необходимо использовать или синхронизацию или ключевое слово **volatile**.

Например, следующий код может не сработать:

```java
public class ThreadStopping
{
  public static void main(String[] args)
  {
    class StoppableThread extends Thread
    {
      private boolean stopped; // defaults to false
      
      @Override
      public void run()
      {
        while(!stopped)
        System.out.println("running");
      }

      void stopThread()
      {
        stopped = true;
      }
    }

    StoppableThread thd = new StoppableThread();
    thd.start();

    try
    {
      Thread.sleep(1000); // sleep for 1 second
    }
    catch (InterruptedException ie) {}

    thd.stopThread();
  }
}
```
Есть вероятность такого исхода, из-за того что изменение значения переменной из потока **main** может не отразиться в потоке **thd**, или произойдет с опозданием.

В свою очередь **volatile** заставит JVM хранить переменную в общей памяти, что приведет к эффекту мягкой синхронизации.

Имей ввиду что *volatile* переменная не может быть *final*, кроме того не стоит на **32**-битной JVM использовать `volatile` c `long` и `double` переменными, так как это небезопасно из-за того что в таком случае потребуется две опеации для доступа или изменения переменной. В таком случае предпочтительно использовать синхронизацию.