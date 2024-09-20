---
date: 2024-09-07 (Sat)
---

# Spring Data

## JPA, JDBC and Persistant Provider

Stands for **_Java Persistant API_**.

JPA is abstraction on top of **_JDBC_**, which is an abstraction above
databases, which aims to provide uniform API to interact with different Database
vendors, and execute SQL commands directly. But JDBC still has a lot of
boilerplate codes, such as setting up connections.

JPA focuses on **_Object Relational Mapping_** (**_ORM_**), allowing you to
interact with database in a more object-oriented way, using Java code instead of
SQL.

JPA itself is just a set of interfaces, and requires a **_persistence
provider_** to provide actual implementation of the JPA specification. Examples
of persistence providers include Hibernate, Eclipse Link and Apache Open JPA.
Spring supports JPA and **_Hibernate_** by default.

## Entity

Entity classes are data we want to persist

```java
@Entity
// @Table(name = "employee")
public class Employee {
  // you don't need to care about case sensitivity, cause the table is created
  // by Hibernate automatically

  @Id
  @Column(name = "emp_id")
  private int id;
  private string mame; // map to column "name"

  @Transient // not to be persisted, i.e. stored in database
  private int age;
}
```

`@Column()` can have many more arguments, like `nullable`, `unique`, `length`,
these specify the constraints set in the database.

### Managed Entity and Detached Entity

When you call pass an entity instance to the `save` method of `JpaRepository`,
the entity becomes a **_managed entity_** until the session is closed and any
changes will be persisted to the database during this period.

When an entity is not managed, it is called a **_deteched entity_**, and any
change to the entity will not be persisted until you `save` it.

The session is closed when the transaction is committed, or when the method
calling the `save` method returns.

## Entity Attributes

### GeneratedValue

`@GeneratedValue(strategy = ...)` specifies strategy for generating primary key
values.

Available strategies:

- `GenerationType.IDENTITY`: relies on an auto-incremented database column.
  Supporting databases include MySQL, PostgreSQL, etc.
- `GenerationType.SEQUENCE`: relies on a database sequence.

## Required Spring Dependencies

- `Spring Data JPA`
- A driver for the database vendor, e.g. `MySQL Driver`

## 🧭 Navigation

- [🔼 Back to top](#spring-data)
- [◀️ Back](spring.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
