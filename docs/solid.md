The **SOLID principles** are five key object-oriented design principles that help you write **clean, maintainable, and scalable code**. Here's how each one applies in Java, with practical examples:

---

## 🟡 1. **S**ingle Responsibility Principle (SRP)

> A class should have **only one reason to change** — i.e., one responsibility.

### ✅ In Java:

Bad:

```java
class ReportService {
    public String generateReport() { ... }
    public void saveToDatabase(String report) { ... }
    public void sendEmail(String report) { ... } // 💥 Multiple responsibilities!
}
```

Good (Split into focused classes):

```java
class ReportGenerator {
    public String generate() { ... }
}

class ReportSaver {
    public void save(String report) { ... }
}

class ReportEmailer {
    public void send(String report) { ... }
}
```

---

## 🟡 2. **O**pen/Closed Principle (OCP)

> Software entities (classes, modules, functions) should be **open for extension** but **closed for modification**.

### ✅ In Java:

Use **interfaces, inheritance, or strategy pattern**.

```java
interface Notification {
    void send(String message);
}

class EmailNotification implements Notification {
    public void send(String message) {
        System.out.println("Sending Email: " + message);
    }
}

class SmsNotification implements Notification {
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}

class Notifier {
    private Notification notification;

    public Notifier(Notification notification) {
        this.notification = notification;
    }

    public void notifyUser(String message) {
        notification.send(message);
    }
}
```

You can add `PushNotification` without modifying `Notifier`.

---

## 🟡 3. **L**iskov Substitution Principle (LSP)

> Subtypes must be **substitutable** for their base types without breaking behavior.

### ✅ In Java:

Bad:

```java
class Bird {
    void fly() { ... }
}

class Ostrich extends Bird {
    void fly() { throw new UnsupportedOperationException(); } // 💥 Breaks LSP!
}
```

Better:

```java
interface Bird { }

interface FlyingBird extends Bird {
    void fly();
}

class Sparrow implements FlyingBird {
    public void fly() { ... }
}

class Ostrich implements Bird {
    // no fly method – no broken contract!
}
```

---

## 🟡 4. **I**nterface Segregation Principle (ISP)

> Clients should not be forced to depend on **interfaces they do not use**.

### ✅ In Java:

Bad:

```java
interface Machine {
    void print();
    void scan();
    void fax(); // 💥 What if some machines don't support fax?
}
```

Good (segregated interfaces):

```java
interface Printer {
    void print();
}

interface Scanner {
    void scan();
}

interface Fax {
    void fax();
}
```

Now a `BasicPrinter` can just implement `Printer` without empty methods.

---

## 🟡 5. **D**ependency Inversion Principle (DIP)

> High-level modules should not depend on low-level modules. **Both should depend on abstractions.**

### ✅ In Java:

Bad:

```java
class MySQLDatabase {
    void save(String data) { ... }
}

class ReportService {
    private MySQLDatabase db = new MySQLDatabase(); // Tight coupling 💥
}
```

Good:

```java
interface Database {
    void save(String data);
}

class MySQLDatabase implements Database {
    public void save(String data) { ... }
}

class ReportService {
    private Database db;

    public ReportService(Database db) {
        this.db = db;
    }
}
```

You can now inject any DB implementation (e.g., MongoDB, Oracle) via constructor or Spring’s DI.
