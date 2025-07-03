Spring manages transactions using @Transactional, which creates a proxy around your bean and wraps method calls inside a transactional boundary.

When a method annotated with @Transactional is called:

- A transaction begins (or joins an existing one).
- If the method completes normally, the transaction commits.
- If an unchecked exception (RuntimeException) is thrown, it rolls back by default.
- Checked exceptions don’t trigger rollback unless explicitly specified.

Spring supports 7 types of propagation via the Propagation enum. Here are the most used ones:


| Propagation     | Behavior                                                 |
| --------------- | -------------------------------------------------------- |
| `REQUIRED`      | Joins existing or creates new transaction                |
| `REQUIRES_NEW`  | Always starts a new transaction                          |
| `NESTED`        | Uses savepoint inside parent transaction                 |
| `NOT_SUPPORTED` | Suspends transaction                                     |
| `MANDATORY`     | Fails if no existing transaction                         |
| `NEVER`         | Fails if a transaction exists                            |
| `SUPPORTS`      | Runs in transaction if available, else non-transactional |
