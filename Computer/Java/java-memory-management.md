---
date: 2024-08-26 (Mon)
---

# Java Memory Management

## Handling Escaping References

### Escaping References of Collections

1. Implement `Iterable`
2. Return a copy of the collection
3. Return an unmodifiable collection

#### Iterable

```java
public class CustomerRecords implements Iterable<Customer> {
  private Map<String, Customer> records;

  // constructors, getters and setters

  @Override
  public Iterator<Customer> iterator() {
    return records.values().iterator();
  }
}
```

But you can still remove elements by `customerRecords.iterator().remove()`.

#### Return a copy

```java
public class CustomerRecords {
  private Map<String, Customer> records;

  public Map<String, Customer> getRecords {
    return new HashMap<>(records);
  }
}
```

But this only returns a shallow copy. And the underlying Customer object can
still be changed.

Also, it may cause confusion for some user may think they are changing the
original collection.

#### Return an unmodifiable collection

Java provides two methods `Collections.unmodifiableMap` and
`Collections.unmodifiableList` and many more that return an unmodifiable
collection.

```java
public class CustomerRecords {
  private Map<String, Customer> records;

  public Map<String, Customer> getRecords {
    return Collections.unmodifiableMap(records);
  }
}
```

### Escaping References of Objects

A partial fix is to return a copy. One way is to define a copy constructor.

But better still, you can create and return an interface that only have
read-only methods.

```java
interface CustomerReadOnly {
  public String getName();
}

class CustomerRecords {
  private Map<String, Customer> records;

  // Return the read-only interface
  public CustomerReadOnly getCustomer(String name) {
    return records.get(name);
  }
}
```

## 🧭 Navigation

- [🔼 Back to top](#java-memory-management)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
