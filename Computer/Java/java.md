---
date: 2024-07-23 (Tue)
---

# Java

## Link to Notes

### Java Basics

- [Four Pillars and SOLID Principles](solid.md)
- [OOP Dependency](../design-diagrams.md#oop)
- [Threading](threading.md)
- [Collections](collections.md)
- [Java Memory Management](java-memory-management.md)
- [Date and Time](date-and-time.md)
- [JavaDoc](javadoc.md)

### Functional Programming

- [Functional Interfaces](FuntionalProgramming/functional-interfaces.md)
- [Lambda Expression](FuntionalProgramming/lambda-expression.md)
- [Streams and Optional](FuntionalProgramming/streams-and-optional.md)

### Java Libraries

- [Log4J2](log4j2.md)
- [Maven](maven.md)
- [Spring](spring/spring.md)

## Jackson

```java
public class Book implements Comparable<Book> {

 private String title;
 private String author;
 private String isbn;

 @JsonCreator
 public Book(@JsonProperty("title") String title, @JsonProperty("author") String author,
   @JsonProperty("isbn-10") String isbn) {
  this.title = title;
  this.author = author;
  this.isbn = isbn;
 }

 @JsonGetter("title")
 public String getTitle() {
  return title;
 }

 @JsonGetter("author")
 public String getAuthor() {
  return author;
 }

 @JsonGetter("isbn-10")
 public String getIsbn() {
  return isbn;
 }

 public int compareTo(Book book) {
  return this.getTitle().compareTo(book.getTitle());
 }
}
```

## Exceptions

- Errors and Exceptions
  - You can and should catch checked Exceptions in your program
  - You should not catch error or unchecked exceptions

### Errors

**_Errors_** indicate a serious problem **external** to the program, e.g. a
hardware malfunction, and `java.io.OutOfMemoryError` might be thrown

You should not catch errors and let the program terminate and then investigate
the problem

### Try-With-Resources

A resource is an object that must be closed after the program is finished with
it. The try-with-resources statement ensures that each resource is closed at the
end of the statement. Any object that implements `java.lang.AutoCloseable`,
which includes all objects which implement `java.io.Closeable`, can be used as a
resource.

- Updates try-catch handling to automatically close resources
- Objects that implement AutoCloseable interface can be passed almost as
  argument to the try block
- Eliminates the need for many finally blocks and exception handling within
  finally blocks

```java
static String readFirstLineFromFile(String path) throws IOException {
  try (FileReader fr = new FileReader(path);
    BufferedReader br = new BufferedReader(fr)) {
    return br.readLine();
  }
}
```

### Custom Exceptions

```java
public class MyCustomException extends Exception {
  public MyCustomException(String message) {
    super(message);
  }
}
```

## External Links and Resources

### Logging

- [Learning Java By Example, Chp 2 – Use Java Logging To Report Errors](https://www.linkedin.com/learning/learning-java-by-example/use-java-logging-to-report-errors)
- [The Complete Log4J Manual](https://www.amazon.com/Complete-Log4j-Manual-Ceki-Gulcu/dp/2970036908)
  (There might be a free version out there)
- [Log4J2 Website](https://logging.apache.org/log4j/2.x/index.html)
- [Log4J2 vulnerability video](https://youtu.be/uyq8yxWO1ls) (interesting)
- [Hands-on introduction: Java](https://www.linkedin.com/learning/hands-on-introduction-java)

### Debugging

- [Debugging the Eclipse IDE for Java Development](https://www.eclipse.org/community/eclipse_newsletter/2017/june/article1.php)
- [Eclipse Debugger Tutorial](https://help.eclipse.org/latest/index.jsp?topic=%2Forg.eclipse.ocl.doc%2Fhelp%2FDebuggerTutorial.html)

### Misc Links

- [Competitive Programming](https://cses.fi/book/book.pdf)

## Misc

### SerialVersionUID

Used to indicate the version of a serializable class, based on the properties of
the class. It makes sure that the correct serialized class is being used on
deserialization. Otherwise, when changes are made to the class member variables,
and the class is deserilalized with the old serialized version, there will be
data lost.

See
[What Is serialVersionUID?](https://dzone.com/articles/what-is-serialversionuid).

### Some libraries

- JavaFX for GUI animation

## Links and Resources

- [Java Lambdas and Streams - LinkedIn Learning](https://www.linkedin.com/learning/java-lambdas-and-streams/java-lambdas-and-streams)
- [Learning Java Lambda Expressions - LinkedIn Learning](https://www.linkedin.com/learning/learning-java-lambda-expressions)
- [Functional Programming with Java - LinkedIn Learning](https://www.linkedin.com/learning/functional-programming-with-java/functional-programming-a-new-way-to-organize-code)

## 🧭 Navigation

- [🔼 Back to top](#index)
- [◀️ Back](../../index.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
