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

### Wait-Notify API:
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

Сначала нужно нанести на мащину воск, потом можно произвести полировку, и так пока не надоест.

Поток **WaxOn** отвечает за нанесение воска и ожидание полировки.

Поток **WaxOff** отвечает за полировку и ожидание нанесения следующего слоя воска.

Давайте рассмотрим такой пример:
1. Приготовить завтрак 
  * Поставить кипятиться чайник -> makeTea() = () -> {slep(3 seconds); notify()}
  * Поставить жариться яичницу  -> makeOmlet() ...
  * Сделать бутерброды          -> makeToast() ...
2. Поесть

Действия из пункта 1 будут выполняться параллено, а пункт 2 может начать выполнения только после завершения всех подпроцессов из п1.

```java
// Тут должен быть исходный код примера и комментарии к нему
// Получается что у нас есть один блокируемый объект доступ к которому будет ждать поток из пункта 2
// Пусть этим объектом будет человек, нет, это должен быть завтрак или кухня
// или все таки человек и его статус будет isCooking() {teaReady && omletReady && toastReady}

// Таким образом while(isCooking()) {wait()}
```