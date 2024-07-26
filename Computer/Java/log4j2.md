---
date: 2024-07-25 (Thu)
---

# Log4J2

## Libraries

```xml
<dependency>
  <groupId>org.apache.logging.log4j</groupId>
  <artifactId>log4j-core</artifactId>
  <version>${2.23.1}</version>
</dependency>
<dependency>
  <groupId>org.apache.logging.log4j</groupId>
  <artifactId>log4j-api</artifactId>
  <version>${2.23.1}</version>
</dependency>
```

## Benefits

Hierarchical logging framework

- Different level of severity
- Inheritable configuration

Send log messages to multiple destinations

Configuration is external to the code

## Key Components

- Loggers

- Appenders

  - Output destinations for log messages
  - A single logger can have multiple appenders

- Layouts

  - Output formats for log message (do you need time, date)
  - A layout is set for each appender

### Loggers

#### What is a logger?

- Named objects that allow us to log messages according to level
- Loggers are in hierarchy, with each logger (except root) having a parent
  logger
- After a logger logs a message, it passes the message to its parent logger for
  further logging
- One "root logger" object always exists
- The root logger has a default level, if the message is of a level higher than
  or equal to the default level, the message will be logged.
- Child logger objects are created from configuration

#### Creating Loggers

A factory design pattern. We use the **static** method `getLogger` of
`LogManager` class. The method is overloaded with three signatures:

- `getLogger("MyLoggerName")`

  Set the logger name to the string passed as an argument.

- `getLogger(ClassName.class)`

  Set the logger name to the _fully qualified class name_ (e.g.
  `com.myorg.package.Class`) of the class. This is the **standard practice**.

- `getLogger()`

  Set the logger name to the _fully qualified class name_ of the class that
  called this method.

You can also get the root logger by `getRootLogger()`

#### Logger Inheritance

Loggers follow a parent-child relationship (dot-separated).

A logger named `com.mycompany.trading` implicitly inherits configuration from
logger `com.mycompany`. If no configuration is found for `com.mycompany`, then
`com.mycompany.trading` will inherit from root logger.

#### Methods

Other than the logging methods:

- `is{Level}Enabled()` such as `isWarnEnabled()` can be used to check if a level
  is enabled for this logger. This can be useful when some message construction
  is expensive, and you only want to run the construction when it will be
  logged.
  - But since Java 8, you can use lambda expressions to avoid this problem:
    `log.debug(() -> "Some expensive message: " + expensiveMethod())` The lambda
    function will only be executed if the level is enabled.

Some tracing methods:

- `entry()` accepts 0 to 4 arguments, typically what is passed to the method
- `traceEntry()` accepts a format string and then a variable number of
  arguments, or a Message.
- `exit()` accept 0 or 1 argument, and return the argument
- `traceExit(object, new SomeMessage(object))`
- `throwing()` when throwing exceptions that is unlikely to be handled
- `catching()` when catching exceptions that the method is not going to rethrow

### Appenders

log4j2 API provides a variety of appenders to log messages to different
destination:

- Files (FileAppender, RollingFileAppender)
- Console (ConsoleAppender)
- Databases (JdbcAppender, NoSqlAppender)
- Remote servers (SocketAppender, HTTPAppender)
- Many more

Also set up in the configuration file.

## Logging

### Severity Levels

Six logging levels, from least severe to most severe:

1. trace (think of stack trace)
2. debug (think of debugging, e.g. value of variables)
3. info (for users, e.g. payment successful)
4. warn (indicate potential problem, e.g. unexpected condition)
5. error (an impact on the system that is recoverable)
6. fatal (an impact that can crash a system)

- Each is a method with the same overloaded argument signatures
- Logging requests are made by invoking one of these methods on a Logger
  instance

```java
log.error(Object message);
log.error(String message);
log.error(Object message, Throwable t);
```

### Log Messages

You can inject variables into log messages using the brace syntax:

```java
logger.info("If you are logging messages without parameters, just use plain text");
logger.info("However, all logging methods are heavily overloaded "
  + "to take in a multitude of additional parameters");
logger.info("");

logger.info("Use braces for parameters, as such: {}", "VALUE");
logger.info("Messages can handle {} number of {}", "any", "parameters");
logger.info("");

logger.info("They can handle all types as well, using toString() for objects");
logger.info("They box primitives and call their toString(), and use Arrays.toString() for arrays");
logger.info("Primitives: int {}, double {}, boolean {}", 32, 76.54321, true);
logger.info("Arrays: {}", new int[] { 5, 4, 3, 2, 1 }); // Arrays: [5, 4, 3, 2, 1]
logger.info("Other objects: {}", Arrays.asList("Hello", "World"));
logger.info("");

logger.info("There are additional ways of formatting loggers, such as using format Strings like printf");
logger.info("Log4j2 also provides a Message interface for more complex messages");
logger.info("See the documentation for more ways, "
  + "but strings and braces should be sufficient for most logging");
```

## Configuration

### Configuration File

Automatically loaded (at startup) if the file name is `log4j2.{fileType}` (e.g.
`log4j2.xml`) and placed in `src/main/resources`. Available file types are:

- .properties
- .xml
- .yaml
- .json

You can set how often log4j2 will check for changes in the configuration file.

If no configuration is found, log4j2 will use the default logger, i.e. printing
all messages to the console.

If configuration needs to be **different** in tests, it is common practice to
have a file `log4j2-test.{fileType}` in `src/test/resources`.

### Setting Up a Logger

Two pieces of information are required:

- A threshold level (minimum level to log)
- At least one appender

If you don't provide these info explicitly, a logger will inherit from their
parent.

You can define a logger in the configuration file even if you never use it, this
can be still useful because of the inheritance nature of loggers.

```xml
<!-- root logger -->
<Root level="error">
  <AppenderRef ref="appenderName" />
</Root>

<Logger name="com.mycompany" level="info">
  <AppenderRef ref="appenderName" />
</Logger>
```

### Configuring Appenders

Info required:

- Destination
- Layout
- Any other appender-specific configuration

## 🧭 Navigation

- [🔼 Back to top](#log4j2)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
