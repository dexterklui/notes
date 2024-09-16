---
date: 2024-08-22 (Thu)
---

# Spring Web

## Web server vs web container

- Web server serves static pages
- Web container sits within a web server that can serve dynamic pages
  (dynamically generated content)

## Apache Tomcat

- Tomcat is both a web server and a web container.
- The APIs for server-side programming are part of Java EE, now renamed to
  Jakarta

## RESTful Web Services

- Is a Web Services
- Follows REST architecture
- Response body can be in different format

The point is that it is standard, and no need extensive docs for people to
understand how to use it.

## Spring Boot Modules

- **_Spring Web_**
  - Jackson comes with spring web. So can serialize what you return into JSON
- **_Spring Boot DevTools_**
  - Allow hot module reloading
- **_Validation_**
  - Set up validation of JSON objects in request bodies using annotations in
    your Java class with which deserializations happen
  - It also validate when trying to persist the object into the database

## Controller

`@Controller` methods always return a string, which identifies a particular
document by default, unless `@ResponseBody` is used above the method.

If you use `@RestController`, then the return value is already `@ResponseBody`
by default. And Jackson is used to serialize the returned value into JSON by
default.

### Path Mapping

There is class level mapping and method mapping.

For class level mapping: `@RequestMapping("/path/to/endpoint")`

- `@GetMapping("")` - method level mapping: GET request under the class level
- `@PostMapping("/sub/path")` - POST request

### Dynamic Path

```java
 @GetMapping("/api/v1/trades/{id}")
 public Trade getTrade(@PathVariable long id) {
    // return ...
 }
```

You can give a different method parameter name, e.g. `contactId`, then you need
to mention the `@PathVariable("id")` annotation.

### Request Parameters

Used like `@PathVariable`

```java
public Trade getTrade(@RequestParam("id") long id) {
  // return ...
}
```

### Request Body

Deserialize the JSON request body into a Java object using `@RequestBody`.

```java
public ResponseEntity<URI> addTrade(@RequestBody Trade trade) {
  savedTrade = tradeService.addTrade(trade);
  URI location = ServletUriComponentsBuilder
    .fromCurrentRequest()
    .path("/{id}")
    .buildAndExpand(savedTrade.getId())
    .toUri();
  return ResponseEntity.created(location).body(savedTrade);
}
```

Missing field from the request body will be set to `null` or the default value
for primitive type field.

### Exceptions and Error Handling

You can create a custom exception and throws it in your controller methods.
Annotate your custom exception with `@ResponseStatus(HttpStatus.<status>)` to
set the response status.

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class TradeNotFoundException extends RuntimeException {
 public TradeNotFoundException() {
  super("Trade not found");
 }

 public TradeNotFoundException(long id) {
  super("Trade not found with id: " + id);
 }
}
```

When you are using OAuth2 to use JWT for user authorization, the
`@ResponseStatus` annotation will have no effect, because the security filter is
overriding the response. You can use a custom exception handler to handle the
exception.

#### Custom Exception Handlers

It allows you to handle exceptions and return a custom response.

```java
/**
 * Handles illegal values in the path variables in a dynamic path from the
 * request.
 */
@ExceptionHandler(MethodArgumentTypeMismatchException.class)
public ResponseEntity<String> handleMethodArgumentTypeMismatchException(MethodArgumentTypeMismatchException ex) {
  HttpStatus status = HttpStatus.BAD_REQUEST;

  Class<?> requiredType = ex.getRequiredType();
  if (requiredType.isEnum()) {
    String validValues = Arrays.stream(requiredType.getEnumConstants())
        .map(Object::toString)
        .collect(Collectors.joining(", "));
    String message = "Invalid value for '" + ex.getName() + "'. Valid values are: " + validValues;

    return new ResponseEntity<>(message, status);
  } else {

    return new ResponseEntity<>("Invalid value for " + ex.getName(), HttpStatus.BAD_REQUEST);
  }
}
```

### Custom Serialization and Deserialization

You can add `@JsonIgnore` to prevent circular reference.

You can use **DTO** (Data Transfer Object) to serialize the object into JSON in
your desired format.

For serialization, a good practice is to use `@JsonIgnore` to prevent exposing
nested objects, and sensitive or unnecessary data by default. And then use
custom DTO to expose more info as needed in each endpoint.

### Response Entity

`ResponseEntity<T>` is a generic class that can be used to return a response. In
the controller class, using this can allow you to control the response message,
like headers and status.

- `ResponseEntity.ok()` or `ok(T body)`
- `ResponseEntity.created(location)`
- `ResponseEntity.noContent()`
- `ResponseEntity.created(location).body(savedTrade)`

### CORS

First way is to use WebMvcConfigurer, which let **_Spring MVC_** to config CORS
globally.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/api/**")
                        .allowedOrigins("http://example.com")
                        .allowedMethods("GET", "POST", "PUT", "DELETE")
                        .allowedHeaders("header1", "header2")
                        .exposedHeaders("header1", "header2")
                        .allowCredentials(true)
                        .maxAge(3600);
            }
        };
    }
}
```

Annotations at class or method level:

```java
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class ApiController {

    // Can also be at class-level
    @CrossOrigin(origins = "http://example.com")
    @GetMapping("/data")
    public String getData() {
        return "Hello, World!";
    }
}
```

But sometimes not all requests/response will be handled by Spring MVC, but by a
security filter. In this case you need to config CORS in the security filter.

### CORS Expose Headers

You can add the following to `CorsConfig` class as in [#CORS](#cors).

```java
import jakarta.servlet.Filter;
import jakarta.servlet.FilterChain;
import jakarta.servlet.FilterConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;
import jakarta.servlet.http.HttpServletResponse;

@Bean
public Filter corsFilter() {
    return new Filter() {
        @Override
        public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
                throws IOException, ServletException {
            HttpServletResponse response = (HttpServletResponse) res;
            response.setHeader("Access-Control-Expose-Headers", "Location");
            chain.doFilter(req, res);
        }

        @Override
        public void init(FilterConfig filterConfig) {
        }

        @Override
        public void destroy() {
        }
    };
}
```

## 🧭 Navigation

- [🔼 Back to top](#spring-web)
- [◀️ Back](spring.md)
- [🔖 Parent index](../java.md)
- [📑 Notes Index](../../../index.md)
