# Java Hashing Cheat Sheet

### 💡 What is Hashing?

Hashing converts a key (like a String or object) into a fixed integer (hash code) using a hash function. It's used for fast data lookup in collections like `HashMap` and `HashSet`.

### How Hashing Works

1. Call `key.hashCode()`
2. Map hash to array index → `index = hash % array.length`
3. Store/retrieve value at that index
4. Use `equals()` to resolve collisions

### Best Practices

* Always override both `equals()` and `hashCode()` for custom keys
* Make key fields `final` (immutable)
* Use `Objects.hash(field1, field2)` or good custom logic
* Ensure equal objects always produce the same hash code
* Use `Objects.equals()` to avoid NPEs

### Common Pitfalls

* Overriding only one of `equals()` or `hashCode()`
* Changing key fields after insertion into a `HashMap`
* Poor hash code logic → many collisions → slower performance
* Null fields causing NPE in `hashCode()` or `equals()`

### Quick Rules

* Equal objects must have equal hash codes
* Unequal objects can have same hash code (collision is allowed)
* Don’t mutate keys once used in hash-based collections

### Example

```java
class User {
  final String name;
  final int age;

  public User(String name, int age) { this.name = name; this.age = age; }

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof User)) return false;
    User u = (User) o;
    return age == u.age && Objects.equals(name, u.name);
  }

  @Override
  public int hashCode() {
    return Objects.hash(name, age);
  }
}
```


### Best Practices

* Use `HashMap`, `HashSet`, `ConcurrentHashMap` for fast access
* Avoid using mutable objects as keys
* Use profiling tools to check for excessive collisions
