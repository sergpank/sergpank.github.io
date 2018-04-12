---
layout: post
title: Java Concurrency Waiting and Notification
description: Java core concurrency in brief
date: 2018-03-14
category: concurrency
collection: concurrency
---

Java предоставляет механизм, позволяющий потокам синхронизировать свои действия. Причем вместо пустой траты ресурсов в цикле вроде:
```java
while(condition)
{
  // do nothing
}
```

Поток остановит свою работу, и более того освободит монитор (в отличие от `sleep()`, который продолжает держать лок):
```java
while(condition)
{
  wait();
}
```

## Wait-Notify API:
+ `wait(), wait(millis), wait(millis, nanos)` - поток уходит в режим ожидания пока не будет разбужен вызовом **notify** или **notifyAll**, или не истечет время ожидания.
+ `notify()` - разбудить один из поток, монитор которого держит данный поток.
+ `notifyAll()` - разбудить все потоки, мониторы которых держит данный поток.

`wait/notify/notifyAll` могут быть вызваны только внутри синхонизированного метода или блока (в отличие от `sleep`). Так надо, ибо **они** работают с локом объекта, а **sleep** нет. В противном случае ты получишь `IllegalMonitorException`.

У Эккеля есть достаточно наглядный пример, показывающий что механизм wait-notify хорошо подходит для случаев, когда нужно соблюсти последовательность действий в многопоточной среде. Когда каждое последующее действие можно начинать по достижению ряда критериев с предыдущих этапов.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

class Car
{
  private boolean waxOn = false;

  public synchronized void waxed()
  {
    waxOn = true; // Ready to buff
    notifyAll();
  }

  public synchronized void buffed()
  {
    waxOn = false; // Ready for another coat of wax
    notifyAll();
  }

  public synchronized void waitForWaxing()
  throws InterruptedException
  {
    while (waxOn == false)
      wait();
  }

  public synchronized void waitForBuffing()
  throws InterruptedException
  {
    while (waxOn == true)
      wait();
  }
}

class WaxOn implements Runnable
{
  private Car car;

  public WaxOn(Car c)
  {
    car = c;
  }

  public void run()
  {
    try
    {
      while (!Thread.interrupted())
      {
        Logger.log("Wax On! ");
        TimeUnit.MILLISECONDS.sleep(200);
        car.waxed();
        car.waitForBuffing();
      }
    }
    catch (InterruptedException e)
    {
      Logger.log("Exiting via interrupt");
    }
    Logger.log("Ending Wax On task");
  }
}

class WaxOff implements Runnable
{
  private Car car;

  public WaxOff(Car c)
  {
    car = c;
  }

  public void run()
  {
    try
    {
      while (!Thread.interrupted())
      {
        car.waitForWaxing();
        Logger.log("Wax Off! ");
        TimeUnit.MILLISECONDS.sleep(200);
        car.buffed();
      }
    }
    catch (InterruptedException e)
    {
      Logger.log("Exiting via interrupt");
    }
    Logger.log("Ending Wax Off task");
  }
}

public class WaxOMatic
{
  public static void main(String[] args) throws Exception
  {
    Car car = new Car();
    ExecutorService exec = Executors.newCachedThreadPool();
    exec.execute(new WaxOff(car));
    exec.execute(new WaxOn(car));
    TimeUnit.SECONDS.sleep(5); // Run for a while...
    exec.shutdownNow(); // Interrupt all tasks
  }
}

class Logger
{
  private static long start = System.currentTimeMillis();
  
  public static void log(String msg)
  {
    System.out.printf("%5d :: %s\n", System.currentTimeMillis() - start, msg);
  }
}

/*
Sample output:

    0 :: Wax On! 
  232 :: Wax Off! 
  436 :: Wax On! 
  639 :: Wax Off! 
  842 :: Wax On! 
 1046 :: Wax Off! 
 1248 :: Wax On! 
 1451 :: Wax Off! 
 1654 :: Wax On! 
 1860 :: Wax Off! 
 2065 :: Wax On! 
 2269 :: Wax Off! 
 2473 :: Wax On! 
 2677 :: Wax Off! 
 2882 :: Wax On! 
 3087 :: Wax Off! 
 3291 :: Wax On! 
 3495 :: Wax Off! 
 3699 :: Wax On! 
 3901 :: Wax Off! 
 4106 :: Wax On! 
 4309 :: Wax Off! 
 4512 :: Wax On! 
 4717 :: Wax Off! 
 4921 :: Wax On! 
 5004 :: WaxOn: Exiting via interrupt
 5004 :: WaxOff: Exiting via interrupt
 5005 :: Ending Wax On task
 5005 :: Ending Wax Off task
*/
```

Сначала нужно нанести на машину воск, потом можно произвести полировку, и так пока не надоест.

Поток **WaxOn** отвечает за нанесение воска и ожидание полировки.

Поток **WaxOff** отвечает за полировку и ожидание нанесения следующего слоя воска.

## И для закрепления, давайте рассмотрим такой пример:

Вася и Петя хотят хорошо позавтракать. Но завтрак нужно сначала приготовить. А для этого ребятам придется подождать.

1. Приготовить завтрак 
  * Поставить кипятиться чайник -> makeTea() = () -> {slep(3 seconds); notify()}
  * Поставить жариться яичницу  -> makeOmlet() ...
  * Сделать бутерброды          -> makeToast() ...
2. Поесть

Действия из пункта 1 будут выполняться параллено, а пункт 2 может начать выполнения только после завершения всех подпроцессов из п1.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class Notification {
    boolean teaReady;
    boolean omletReady;
    boolean toastReady;

    enum Dish {
        TEA,
        OMLET,
        TOAST
    }

    public static void main(String[] args) {
        Notification n = new Notification();
        Runnable teaCooker = n.makeCooker(Dish.TEA, 5);
        Runnable omletCooker = n.makeCooker(Dish.OMLET, 7);
        Runnable toastCooker = n.makeCooker(Dish.TOAST, 3);
        Runnable vasya = n.makeEater("Вася");
        Runnable petya = n.makeEater("Петя");

        ExecutorService pool = Executors.newCachedThreadPool();

        pool.execute(vasya);
        pool.execute(petya);
        pool.execute(teaCooker);
        pool.execute(omletCooker);
        pool.execute(toastCooker);

        pool.shutdown();
    }

    Runnable makeCooker(Dish dish, long delay) {
        return () -> {
            System.out.printf("Start cooking [%s] ...\n", dish.name());

            try {
                TimeUnit.SECONDS.sleep(delay);
            } catch (Throwable t) {
                // NOTHING TO DO HERE
            }

            System.out.printf("[%s] is ready!\n", dish.name());

            switch (dish) {
                case TEA  : teaReady   = true; break;
                case OMLET: omletReady = true; break;
                case TOAST: toastReady = true; break;
            }
            synchronized (this) {
                notifyAll();
            }
        };
    }

    Runnable makeEater(String name) {
        return () -> {
            while (!catTakeBreakfast()) {
                System.out.printf("%s : Waiting for my breakfast ...\n", name);
                synchronized (this) {
                    try {
                        wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
            System.out.printf("%s : FINALLY! num - num - num !\n", name);
        };
    }

    boolean catTakeBreakfast() {
        return teaReady && omletReady && toastReady;
    }
}


/* Execution result:

Вася : Waiting for my breakfast ...
Start cooking [TOAST] ...
Start cooking [OMLET] ...
Start cooking [TEA] ...
Петя : Waiting for my breakfast ...
[TOAST] is ready!
Петя : Waiting for my breakfast ...
Вася : Waiting for my breakfast ...
[TEA] is ready!
Вася : Waiting for my breakfast ...
Петя : Waiting for my breakfast ...
[OMLET] is ready!
Петя : FINALLY! num - num - num !
Вася : FINALLY! num - num - num !

*/
```

Если вам интересно что будет если в методе `makeCooker()` заменить `notifyAll()` на `notify()` - дерзайте! 
И вина за то что один из друзей навсегда останется голоднным всецело лежит на вас:

```java
/*

Вася : Waiting for my breakfast ...
Start cooking [TOAST] ...
Start cooking [OMLET] ...
Start cooking [TEA] ...
Петя : Waiting for my breakfast ...
[TOAST] is ready!
Вася : Waiting for my breakfast ...
[TEA] is ready!
Петя : Waiting for my breakfast ...
[OMLET] is ready!
Вася : FINALLY! num - num - num !

*/
```

Потому что `notify()` выводит из спячки только один поток, наблюдающий за данной блокировкой.