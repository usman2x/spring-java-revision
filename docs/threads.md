# Concurrency & Threading Cheat Sheet


## Why Use Threads, Concurrency & Parallelism

| Concept         | Purpose                                                   | Real-World Benefit                                        |
| --------------- | --------------------------------------------------------- | --------------------------------------------------------- |
| **Threading**   | Run tasks concurrently in same app                        | Handle multiple tasks (e.g., serve 100s of requests)      |
| **Concurrency** | Structure program to do multiple things logically at once | Improved responsiveness (UI, backend tasks)               |
| **Parallelism** | Execute tasks physically in parallel using multiple cores | Speed up CPU-heavy tasks (image processing, calculations) |


## Deciding Thread Pool Size (Rule of Thumb)

### 1. **CPU-bound tasks**

> e.g., computation, encryption
> **Formula:**

```java
Threads ≈ CPU cores + 1
```

### 2. **I/O-bound tasks**

> e.g., DB calls, HTTP, file access
> **Formula:**

```java
Threads ≈ Cores × (1 + Wait time / Compute time)
```

**Alternative:** Use non-blocking/reactive (WebFlux, NIO)


## ⚙Implementing in Java

### 1. **ExecutorService (Best Practice)**

```java
ExecutorService pool = Executors.newFixedThreadPool(10);
pool.submit(() -> {
    // your task logic here
});
pool.shutdown();
```

### 2. **Callable + Future (Async Result)**

```java
Future<String> future = pool.submit(() -> "Result");
String result = future.get(); // blocking
```

### 3. **CompletableFuture (Non-blocking Async)**

```java
CompletableFuture.supplyAsync(() -> fetchData())
                 .thenApply(data -> process(data))
                 .thenAccept(System.out::println);
```

### 4. **Thread-safe Data Structures**

* `ConcurrentHashMap`
* `CopyOnWriteArrayList`
* `BlockingQueue` (e.g., `ArrayBlockingQueue`)
* `AtomicInteger`, `ReentrantLock`

---

## Monitoring Threads in Production

### Tools

| Tool                      | Usage                               |
| ------------------------- | ----------------------------------- |
| `jconsole` / `jvisualvm`  | Visual thread state analysis        |
| `jstack`                  | Dump current thread stack traces    |
| `ThreadMXBean`            | Programmatically access thread info |
| `Micrometer` + Prometheus | Metrics for thread pool usage       |

### Metrics to Track

* Active threads
* Pool size
* Queue size (if bounded)
* Task execution time
* Rejected task count

```java
ThreadPoolExecutor exec = (ThreadPoolExecutor) pool;
System.out.println(exec.getActiveCount());
```

---

## Quick Tips

* Use `submit()` instead of `execute()` for error tracking.
* Avoid blocking — use `CompletableFuture` where possible.
* Monitor for deadlocks with `jstack` or lock diagnostics.
* Always shut down executors (`shutdown()` or `shutdownNow()`).
* Use thread naming (`ThreadFactory`) for easier debugging.
