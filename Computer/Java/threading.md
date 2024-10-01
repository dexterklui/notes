---
date: 2024-07-29 (Mon)
---

# Threading

## Multi-Threading Programming

### Concurrent, Parallel and Multi-threading

**_Multi-threading programming_** is a programming technique that allows a
single process to have multiple threads of execution. It allows a program to
perform multiple tasks simultaneously.

**_Concurrent computing_** is a form of computing in which several computations
are executed during overlapping time periods - concurrently - instead of
sequentially (one completing before the next starts).

**_Parallel programming_** is a programming technique that dedicates a separate
CPU to each thread of execution.

In multi-threading, CPU cores switch between threads, giving the illusion that a
single core is executing multiple threads simultaneously.

### CPU Registers and Context Switch

**_Register_** is the CPU memory. **_Context switch_** is the process of saving
the current state of the CPU and loading the state of another process. (Register
stores the states)

A thread has its own program counter, thread id, register and stack.

## Basic Thread Management

### Creating and Running Threads

#### Threads and Runnable

A `Thread` is like a worker, and a `Runnable` is like a job. A worker can work
on a job.

#### Creating Threads

Two methods:

- Extending `Thread` class

  Needs to override the `run()` method.

- Implementing `Runnable` interface, and pass it to a `Thread` object

  Needs to implement the `run()` method.

  Since a runnable is also a
  [functional interface](lambda-expression.md#functional-interface), we can use
  a **lambda expression** to define a runnable in place:
  `Thread thread = new Thread(() -> { /* code */ });`.

> [!NOTE]
>
> Since a thread can be interrupted by another thread, the `run()` method should
> be enclosed in a try-catch block to catch `InterruptedException`.

#### Running Threads

Call the `start()` method of a thread object.

#### Thread Names

You can give a name to a thread:

- Passing to the constructor: `Thread(myRunnable, "myThreadName")`, etc.
- Setting it: `myThread.setName("myThreadName")`

More than one thread may have the same name. If a name is not specified when a
thread is created, a new name is generated for it.

#### Examples of Creating and Running Threads

##### Extending `Thread` class

```java
class MyThread extends Thread {
    @Override
    public void run() {
        // Code to be executed in the new thread
        for (int i = 1; i <= 5; i++) {
            System.out.println("Thread " + Thread.currentThread().getId() + ": Count " + i);
        }
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread();
        MyThread thread2 = new MyThread();

        thread1.start(); // Starts the first thread
        thread2.start(); // Starts the second thread
    }
}
```

##### Implementing `Runnable` interface

```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Thread " + Thread.currentThread().getId() + ": Count " + i);
        }
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread thread1 = new Thread(myRunnable);
        Thread thread2 = new Thread(myRunnable);

        thread1.start(); // Starts the first thread
        thread2.start(); // Starts the second thread
    }
}
```

### Lifecycle of a Thread

```text
          ┌──────────┐
NEW → RUNNABLE → RUNNING -> TERMINATED
          └ BLOCKED ←┘
```

Other than blocked, _sleep_ and _waiting_ are two more temporary states.

### Thread Priority

JVM uses thread priority to decide which thread to execute first when multiple
threads are waiting to be executed.

- `Thread.MAX_PRIORITY` (10)
- `Thread.NORM_PRIORITY` (5)
- `Thread.MIN_PRIORITY` (1)

`myThread.setPriority(7)` sets the priority of a thread to be 7.

### Daemon Thread

All threads are either _user_ thread or **_daemon thread_**. When all the
non-daemon threads finish, the JVM exits.

A newly created thread inherits the daemon status of its parent thread.

Related methods:

- `myThread.setDaemon(myBoolean)` marks the thread as a daemon thread if
  `myBoolean` is true.
- `myThread.isDaemon()` checks if a thread is a daemon thread.

### Thread Group

A thread can belong to a
[thread group](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/ThreadGroup.html).
We can use `ThreadGroup` to manage threads.

```java
ThreadGroup threadGroup = new ThreadGroup("MyThreadGroup");
Thread thread = new Thread(threadGroup, "MyThread");
```

### Getting Current Thread

Use the static method `Thread.currentThread()` to get the current thread.

### Handling Uncaught Exceptions

`myThread.setUncaughtExceptionHandler(myExceptionHandler)` sets the handler
invoked when this thread abruptly terminates due to an uncaught exception.

## Preventing a race condition

- Synchronization (monitor)
- Locks

### Synchronized Methods

When a thread calls a synchronized method, other threads are blocked from
calling **any** of the object's synchronized methods. We use the keyword
`synchronized`.

```java
private int counter;

public synchronized void increment () {
  counter++;
}

public synchronized void decrement () {
  counter--;
}
```

### Synchronized Blocks

More granular than using synchronized method.

When any thread is inside any "apple" synchronized block
`synchronized("apple") { /* code */ }`, no other thread can enter "apple"
synchronized block.

```java
public void increment() {
  synchronized(this) {
    counter++;
  }
}
```

### Reentrant Locks

```java
ReentrantLock reentrantLock = new ReentrantLock();
```

Some methods of re-entrant lock:

- `lock()`
- `tryLock()`
- `lockInterruptibly()`
- `unlock()`

Recommended to use try-finally block to ensure the lock is released.

```java
class X {
  private final ReentrantLock lock = new ReentrantLock();
  public void m() {
    lock.lock();  // block until condition holds
    try {
        // ... method body
    } finally {
        lock.unlock();
    }
  }
}
```

### Reentrant ReadWrite Locks

## Problem with synchronization and locks

- Deadlock
- Livelock
- Starvation

## Thread Methods

### join()

Executing `myOtherThread.join()` on a thread means the thread will wait until
`myOtherThread` finishes before continuing execution.

### interupt()

- `myOtherThread.interupt()` will send causes `myOtherThread` to receive a
  `InterruptedException`. It's up to `myOtherThread` to decide how to handle the
  exception. Some may breaks its own loop and finishes the thread, some may do
  some clean up and terminates.

### sleep()

`Thread.sleep(milliSeconds)` is a static method that will cause the currently
executing thread to sleep for an minimum of `milliSeconds` before continuing
execution.

## Thread Communication

- wait()
- notify()
- notifyAll()

## Functional Interfaces

Interfaces marked by annotation `@FunctionalInterface` can be used as the
assignment target for a lambda expression or method reference.

`Callable` and `Runnable` are examples of functional interfaces.

## Concurrent Collections

### Concurrent Map

Implementations:

- ConcurrentHashMap
- ConcurrentSkipListMap
- ConcurrentNavigableMap

#### ConcurrentHashMap

Retrieval operations (including get) generally do not block, so may overlap with
update operations (including put and remove).

An update operation for a given key bears a happens-before relation with any
(non-null) retrieval for that key reporting the updated value.

For aggregate operations such as putAll and clear, concurrent retrievals may
reflect insertion or removal of only some entries.

So I guess it means don't read a value, and use that value to do calculation and
then write the result back if there is another thread that can update the same
value in between.

### Synchronized Sorted Set

`Collections` has a static method `synchronizedSortedSet()` that wraps a sorted
set to return a synchronized (thread-safe) sorted set.

Still, it is imperative that the user manually synchronize on the returned
sorted set when traversing it or any of its subSet, headSet, or tailSet views
via Iterator, Spliterator or Stream:

## Threads Managements

Use `ExecutorService` interface to manage threads. Check the tutorial and docs
in [Links and Resources](#links-and-resources) below. Use this when you want to
follow one-thread-per-task model.

Join and fork are used for improving performance in cases where a task can be
broken down to smaller tasks recursively.

## Synchronizers

- [Semaphore](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/concurrent/Semaphore.html)

## Comparisons

### HashTable, HashMap and ConcurrentHashMap

`HashTable` was the first implementation of a hash table in Java. It is fully
synchronized in the sense any operation locks the entire table. Does not allow
null as key.

Later, `HashMap` was introduced to implement the `Map` interface, becoming a
member of _Java Collections Framework_. Unlike `HashTable`, `HashMap` is not
synchronized, and thus not thread-safe. Allows null as key.

`ConcurrenthashMap` also implements the `Map` interface and is thread-safe by
providing fine-grained synchronization to support high concurrency. It is more
efficient than `HashTable`.

### ArrayList, Vector and CopyOnWriteArrayList

`ArrayList` is not synchronized. `Vector` is synchronized.
`CopyOnWriteArrayList` is a thread-safe variant of `ArrayList`.

`Collections` has `synchronizedList()` method that wraps a list to return a
synchronized (thread-safe) list.

## Links and Resources

- [Package java.util.concurrent - Java 17 SE Docs](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/concurrent/package-summary.html)
- [A Guide to the Java ExecutorService - Baeldung](https://www.baeldung.com/java-executor-service-tutorial)

## 🧭 Navigation

- [🔼 Back to top](#threading)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
