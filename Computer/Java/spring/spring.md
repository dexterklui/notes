---
date: 2024-08-19 (Mon)
---

# Spring

## ToC

- [Spring Boot](spring-boot.md)
- [Spring Data](spring-data.md)
- [Spring Web](spring-web.md)
- [Spring Validation](spring-validation.md)
- [Rest Client](rest-client.md)
- [Spring Security](spring-security.md)
- [Lombok](lombok.md)

## Intro

- Dependency on Maven
- **Inversion of Control** (**IoC**) - helps achieve loose coupling
  - The framework handling for us instead
- A lot of modules - spring boot, security, data

**Spring beans** are just regular objects that are managed by spring core
container

Spring can be Configure by:

- XML
- Annotated Class (if you have multi annotated configuration class, Spring just
  combined them together and see as one configuration. So you can separate
  different kinds of configuration in different classes)

`SpringApplication.run({configClass}, args)`

Three files are required for a Spring application

- Your `@SpringBootApplication` Bootstrap class
- `@SpringBootTest` test class - you just write like junit and mockito inside
- `src/main/resources/application.properties` stores config data, like database
  url, port number, etc

With Spring Boot, there is auto configuration based on what spring boot
dependencies you add. E.g. with `data` module Spring Boot will automatically
configure a datasource for you and try to connect to it.

**Package hierarchy** is important, the `@ComponentScan` architecture only pick
up subpackages from your root application

## Spring Beans

### Annotations

- `@Component` - general objects that don't fit the below categories
- `@Service` - service layer - involving business logic
- `@Repository` - that provide CRUD with a storage space such as a DAO
- `@Controller` - handle http requests and response

`@Bean` - is for general objects like `@Component`. With `@Bean`, the bean name
by default is the method name.

When there are multiple beans, you can mark one as `@Primary`. By default the
first four component annotations are primary over ones defined by `@Bean`. Then
`@Autowired` will use the `@Primary` bean matching the type. Or you can use
`@Qualifier("beanName")` under `@Autowired` to specify which bean to use.

### Autowiring

Autowiring is the process of injecting the object dependency. Spring framework
automatically creates the bean and injects all the necessary dependencies.

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

## Local Directory Storing Downloaded Dependencies

On Linux, its downloaded to `~/.m2/repository`

## Web Server and Spring Architecture

### Dispatcher Servlet and Handler Interceptor

![Handler Interceptor Flow](interceptor.png)

## Links and Resources

- [Reference - Spring Boot](https://docs.spring.io/spring-boot/reference/)
- [Learning Spring with Spring Boot - LinkedIn](https://www.linkedin.com/learning/learning-spring-with-spring-boot-13886371/learn-rapid-development-with-spring-boot?u=78163626)
- [Servlet things every Java Developer must know - Servlet, Container, Filter, and Listener - Medium](https://medium.com/javarevisited/servlet-things-every-java-developer-must-know-servlet-container-filter-and-listener-374a460169bd)
- [Spring Framework - Filter vs Dispatcher Servlet vs Interceptor vs Controller - Medium](https://medium.com/javarevisited/spring-framework-filter-vs-dispatcher-servlet-vs-interceptor-vs-controller-745aa34b08d8)

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
