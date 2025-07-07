## AOP (Aspect-Oriented Programming) 


AOP helps you **separate cross-cutting concerns** (like logging, security, transactions) from your business logic.
Instead of repeating the same logic in many places, you define it once in an **Aspect**.


> AOP in Spring lets you inject reusable logic like logging or security **around method calls**, without cluttering your code.



| Concept    | Meaning                              | Example                          |
| ---------- | ------------------------------------ | -------------------------------- |
| Aspect     | Class with AOP logic                 | `LoggingAspect` class            |
| Advice     | Code to run at join points           | `@Before`, `@After`, `@Around`   |
| Join Point | Method call where advice can apply   | `JoinPoint joinPoint` param      |
| Pointcut   | Expression that selects join points  | `execution(* com.service.*(..))` |
| Weaving    | Applying aspect to the target method | Done via proxy (Spring AOP)      |



### Common Advice Types

```java
@Before      // Runs before method
@After       // Runs after method
@AfterReturning // After successful return
@AfterThrowing  // If method throws exception
@Around      // Wraps the method (full control)
```


**Example**

```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.bank.service.*.*(..))")
    public void logCall(JoinPoint jp) {
        System.out.println("Calling: " + jp.getSignature());
    }
}
```

This logs all method calls inside `com.bank.service`.



### Best Practices

* ✅ Use AOP for cross-cutting logic only
* ✅ Use custom annotations (like `@LogExecution`) for clarity
* ✅ Keep aspects small and focused
* ✅ Avoid internal method calls (they bypass AOP)
* ✅ Don’t use broad pointcuts like `*.*(..)` — be specific
* ✅ Prefer Annotation-Based Pointcuts, Instead of using fragile execution() pointcuts, define custom annotations like @LogExecution, @TrackPerformance, etc.



### 🔸 When Not to Use

* Business logic → write directly in services
* Per-request logic → use filters/interceptors instead
