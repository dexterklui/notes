---
date: 2024-08-19 (Mon)
---

# Spring

## ToC

- [Spring Web](spring-web.md)
- [Spring Validation](spring-validation.md)
- [Rest Client](rest-client.md)
- [Spring Security](spring-security.md)

## Intro

#### Spring

- Dependency on Maven
- Inversion of Control - helps achieve loose coupling
  - The framework handling for us instead
- A lot of modules - spring boot, security, data

**Spring beans** are just regular objects that are managed by spring core
container

Spring can be Configure by:

- XML
- Annotated Class (if you have multi annotated configuration class, Spring just
  combined them together and see as one configuration. So you can separate
  different kinds of configuration in different classes)

`SpringApplication.run({config class}, args)`

Three files are required for a Spring application

- Your `@SpringBootApplication` Bootstrap class
- `@SpringBootTest` test class - you just write like junit and mockito inside
- `src/main/resources/application.properties` stores config data, like database
  url, port number, etc

With Spring Boot, there is auto configuration based on what spring boot
dependencies you add. E.g. with `data` module Spring Boot will automatically
configure a datasource for you and try to connect to it.

Package hierarchy is important, the @ComponentScan architecture only pick up
subpackages from your root application

## Spring Beans

### Annotations

- `@Component` - general objects that don't fit the below categories
- `@Service` - service layer - involving business logic
- `@Repository` - that provide CRUD with a storage space such as a DAO
- `@Controller` - handle http requests and response

The

`@Bean` - is for general objects like `@Component`. With `@Bean`, the bean name
by default is the method name.

With there are multiple beans, you can mark one as `@Primary`. By default the
first four component annotations are primary over ones defined by `@Bean`. Then
`@Autowired` will use the `@Primary` bean matching the type. Or you can use
`@Qualifier("beanName")` under `@Autowired` to specify which bean to use.

### Auto wiring

Autowiring is

Can be field-, constructor, or setter-injection. Placing `@Autowired` above any
one.

- For setter-injection, only inject when the dependency bean exist, otherwise
  null
- For field-injection, it throws an error when dependency bean does not exist
- For constructor-injection, it also throws an error when dependency bean does
  not exist. But it can handle setting `final` fields, unlike field-injection.

As a general rule of thumb, you usually pick constructor injection.

With newer version, you don't even need to provide `@Autowired` annotation,
Spring will automatically finds the appropriate constructor you defined and use
constructor-injection.

### Common Spring Issues

Issues and exceptions with Spring are mostly two:

- Bean not found
- Multiple beans found

## Spring Data

### Managed Entity and Detached Entity

When you call pass an entity instance to the `save` method of `JpaRepository`,
the entity becomes a **_managed entity_** until the session is closed and any
changes will be persisted to the database during this period.

When an entity is not managed, it is called a **_deteched entity_**, and any
change to the entity will not be persisted until you `save` it.

The session is closed when the transaction is committed, or when the method
calling the `save` method returns.

## Spring Boot Project

### Initializr

- [Spring Initializr](https://start-spring.io) - create bare bone spring project

  It by default comes with spring core and spring testing dependencies

### Three Basic Files of Spring Boot Project

- `<ProjectName>Application.java` - The application's bootstrap class and
  primary Spring configuration class
- `<ProjectName>ApplicationTests.java` - A basic integration test class
- `application.properties` - A place to configure application and Spring Boot
  properties

### Spring Boot Application Class

Marked by `@SpringBootApplication`, which combines three annotations:

- `@Configuration` - As a configuration class replacing XML config file
- `@ComponentScan` - Enable component scanning - auto read annotations in other
  classes
- `@EnableAutoConfiguration` - Enable Spring Boot's auto-configuration

### Spring Boot Application Test Class

```java
@Test
public void contextLoads() {} // test that context has been loaded
```

### Application Properties

Where you can override auto-configuration properties from Spring Boot.

You can import config from another file by
`spring.config.import=optional:file:.local.properties`. Order of the import
statement doesn't matter, as imported values always override the importing file.

## JPA

Java Persistant API

JPA is abstraction on top of JDBC, which is an abstraction above databases

ORM - Object Relational Mapping

JPA requires persistence provider such as Hibernate, Eclipse Link or Apache Open
JPA. Spring supports JPA and Hibernate by default.

### Entity

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

`@Column()` can have many more arguments, like `nullable`, `unique`, `length`.
Other libraries provide overlapped feature, like `@NotNull`

### Spring dependency

- `Spring Data JPA`
- `MySQL Driver`

## Personal Project

### Product Backlog

- [ ] As a seller, I want to post a trade so that I can sell my product
- [ ] As a buyer, I want to view a trade so that I can buy a product

## Local Directory Storing Downloaded Dependencies

On Linux, its downloaded to `~/.m2/repository`

## Links and Resources

- [Reference - Spring Boot](https://docs.spring.io/spring-boot/reference/)
- [Learning Spring with Spring Boot - LinkedIn](https://www.linkedin.com/learning/learning-spring-with-spring-boot-13886371/learn-rapid-development-with-spring-boot?u=78163626)

## Q&A

- Do we know which bean is being created first? Does the order matter?
- How do lazy initializing work?
- Is it a good idea to add `@Transactional` to all service methods modifying any
  rows?
  - Yes. This ensures that the transaction is rolled back if an exception occurs
    during the method execution.

## 🧭 Navigation

- [🔼 Back to top](#spring)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
