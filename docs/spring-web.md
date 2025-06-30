
### **Spring MVC (Servlet-Based)**

* Built on **Servlet API** and traditional **blocking I/O**.
* Each HTTP request is handled by a **dedicated thread** from a thread pool.
* **Thread waits** (blocks) during I/O like DB or HTTP calls.
* Best suited for **low to moderate traffic**, traditional web apps (e.g., admin panels, CRMs).
* Easier to debug, **wider third-party compatibility** (JDBC, libraries).
* **Security/session** stored using **ThreadLocal**.

---

### **Spring WebFlux (Reactive)**

* Built on **Project Reactor** and **non-blocking I/O** (Netty, Undertow).
* Uses a **small, event-loop thread pool** to handle many concurrent requests.
* Threads are **never blocked**; work is paused/resumed as data is ready.
* Ideal for **high-concurrency**, **real-time**, or **streaming** systems (e.g., payment gateways, FX rates, IoT).
* Embraces **functional, declarative** reactive streams (`Mono`, `Flux`).
* **Security/session** stored in **Reactor Context**, not thread.

