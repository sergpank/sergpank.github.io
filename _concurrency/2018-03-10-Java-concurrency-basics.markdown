---
layout: post
title: Java Concurrency Basics
description: Java core concurrency in brief
date: 2018-03-10
category: concurrency
collection: concurrency
---

## 1. Существует много способов создать поток в Java:
```java
Runnable r = () -> System.out.println("Hello Runnable!");
Thread t = new Thread(r);
t.start();
```

```java
new Thread(new Runnable(){
  @Override
  public void run()
  {
    System.out.println("Hello Thread!");
  }
}).start();
```

```java
class MyThread extends Thread
{
  @Override
  public void run()
  {
    System.out.println("My Thread: " + Thread.currentThread());
  }
}

public class Test
{
  public static void main(String[] asdf)
  {
    new MyThread().start();
    System.out.println("Main Thread: " + Thread.currentThread());
  }
}

/*
 * В консоли отобразится следующее:
 *
 * My Thread: Thread[Thread-0,5,main]
 * Main Thread: Thread[main,5,main]
 */
```

## 2. Потоки демоны.

Приложение не может завершить свою работу пока все его потоки не завершатся. Исключением являются потоки демоны, даже если они не завершили свою работу, приложение все равно завершит выполнение.

По умолчанию все потоки являются обычными, для того чтобы изменить их тип нужно указать `thread.setDaemon(true)`. Проверить статус потока также просто `thread.isDaemon()`.

## 3. Thread.join() and Thread.join(milliseconds) and Thread.join(milliseconds, nanoseconds)

Если необходимо приостановить один поток пока не завершиться выполнение другого потока, то нам на помошь приходит метод `join()`:
```java
public static void main(String[] asdf) throws InterruptedException
{
  Thread threadA = new Thread(() -> exec(3), "Thread A");
  Thread threadB = new Thread(() -> exec(5), "Thread B");

  threadA.start();
  threadB.start();

  threadA.join();

  exec(2);
}

private static void exec(int cnt)
{
  for (int i = 0; i <= cnt; i++)
  {
    try
    {
      Thread.sleep(100);
      System.out.println(Thread.currentThread().toString() + i);
    }
    catch (InterruptedException e)
    {
      e.printStackTrace();
    }
  }
}

/*
Sample output:

Thread[Thread B,5,main]0
Thread[Thread A,5,main]0
Thread[Thread A,5,main]1
Thread[Thread B,5,main]1
Thread[Thread A,5,main]2
Thread[Thread B,5,main]2
Thread[Thread A,5,main]3
Thread[Thread B,5,main]3
Thread[main,5,main]0
Thread[Thread B,5,main]4
Thread[main,5,main]1
Thread[Thread B,5,main]5
Thread[main,5,main]2
*/
```

Как видно из примера, главный поток `main` дождался заверншения работы потока `A` после чего начал выполнение. При этом поток `B` продолжил работу параллельно с потоками `A` и `main`. Блокировка коснулась только потока `main`.

Также не обязательно ждать завершенния заjoinненного потока, можно задать таймаут в миллисекундах или даже в наносекундах, для этого имеются соответствующие методы `join(millis)` und `join(millis, nanos)`. ХЗ для чего точность до наносекунда, видать кому-то при написании JDK эта фича была нужна, я не могу привести разумный пример из реальной жизни когда вам это пригодится.

## 4. Прерывание потока.

Для прерывания потока, существует примитивный механизм. Он заключается в том чтобы во времы выполнения потока постоянно проверять значение флага `interrupted`, и если он остановлен завершать работу потока.

Для этого имеются два метода:

1. `Thread.interrupted()` - Возвращает значение флага **interrupted** и сбрасывает его значение в **false**.
2. `Thread.isInterrupted()` - Возвращает значение флага **interrupted** не изменяя его.

Вот примеры выполнения:

```java
import java.util.concurrent.TimeUnit;

class MyThread extends Thread
{
  @Override
  public void run()
  {
    while (!interrupted()) {
      System.out.println(Thread.currentThread() + " ... running");
    }
    System.out.println(Thread.currentThread() + " ... interrupted ! ! !");
    System.out.println("IS INTERRUPTED: " + Thread.currentThread().isInterrupted());
  }
}

public class Test
{
  public static void main(String[] asdf) throws InterruptedException
  {
    MyThread myThread = new MyThread();
    myThread.start();

    TimeUnit.SECONDS.sleep(3);

    myThread.interrupt();
  }
}

/*
Sample output:
...
Thread[Thread-0,5,main] ... running
Thread[Thread-0,5,main] ... running
Thread[Thread-0,5,main] ... running
Thread[Thread-0,5,main] ... interrupted ! ! !
IS INTERRUPTED: false
*/
```

```java
import java.util.concurrent.TimeUnit;

class MyThread extends Thread
{
  @Override
  public void run()
  {
    while (!isInterrupted()) {
      System.out.println(Thread.currentThread() + " ... running");
    }
    System.out.println(Thread.currentThread() + " ... interrupted ! ! !");
    System.out.println("IS INTERRUPTED: " + Thread.currentThread().isInterrupted());
  }
}

public class Test
{
  public static void main(String[] asdf) throws InterruptedException
  {
    MyThread myThread = new MyThread();
    myThread.start();

    TimeUnit.SECONDS.sleep(3);

    myThread.interrupt();
  }
}

/*
Sample output:
...
Thread[Thread-0,5,main] ... running
Thread[Thread-0,5,main] ... running
Thread[Thread-0,5,main] ... running
Thread[Thread-0,5,main] ... interrupted ! ! !
IS INTERRUPTED: true
*/
```

Поток может находиться в одном из следующих состояний:
+ **NEW** - Поток создан, но еще не запущен.
+ **RUNNABLE** - Поток работающий в JVM (запущен методом *start()*).
+ **BLOCKED** - Поток заблокирован и ожидает разблокировки монитора.
+ **WAITING** - Поток, ожидающий разблокировки от другого потока неопределенное время.
+ **TIMED_WAITING** - Поток, ожидающий разблокировки от другого потока неопределенное время.
+ **TERMINATED** - Поток завершивний свою работу.