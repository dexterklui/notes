---
date: 2024-09-07 (Sat)
---

# Spring Boot

## Initializr

- [Spring Initializr](https://start-spring.io) - create bare bone spring project

  It by default comes with spring core and spring testing dependencies

## Three Basic Files of Spring Boot Project

- `<ProjectName>Application.java` - The application's bootstrap class and
  primary Spring configuration class
- `<ProjectName>ApplicationTests.java` - A basic integration test class
- `application.properties` - A place to configure application and Spring Boot
  properties

## Spring Boot Application Class

Marked by `@SpringBootApplication`, which combines three annotations:

- `@Configuration` - As a configuration class replacing XML config file
- `@ComponentScan` - Enable component scanning - auto read annotations in other
  classes
- `@EnableAutoConfiguration` - Enable Spring Boot's auto-configuration

## Spring Boot Application Test Class

```java
@Test
public void contextLoads() {} // test that context has been loaded
```

## Application Properties

Where you can override auto-configuration properties from Spring Boot.

You can import config from another file by
`spring.config.import=optional:file:.local.properties`. Order of the import
statement doesn't matter, as imported values always override the importing file.

## 🧭 Navigation

- [🔼 Back to top](#spring-boot)
- [◀️ Back](spring.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
