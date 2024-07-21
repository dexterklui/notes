---
date: 2024-06-03 (Mon)
---

# Test Driven Development

## TDD

### History

- Burn out of HP (extreme programming) movement in the late 90s
  - Push to the extreme - what if we test everything? - This is the beginning of
    TDD.

### Agile

TDD is Agile

- Design occurs at the same time as testing
- All aspects are done concurrently
- Tests are well written
- Focuses and directs effort to produce value
- Encourages emergent design

### Three Laws of TDD

1. You must write a failing test before you write any production code.
2. You must not write more of a test than is sufficient to fail, or fail to
   compile.
3. You must not write more production code than is sufficient to make the
   currently failing test pass.

This can be seen as **_red-green-refactor_** cycle.

1. Create a unit tests that fails
2. Write production code that makes that test pass.
3. Clean up the mess you just made.

The philosophy is based on the idea that our limited minds are not capable of
pursuing the two simultaneous goals of all software systems: 1. Correct
behaviour. 2. Correct structure. So the RGR cycle tells us to first focus on
making the software work correctly; and then, and only then, to focus on giving
that working software a long-term survivable structure.

1. Make it work
2. Make it right
3. Make it fast

### What tests to start?

1. Select the user story with highest priority in sprint planning
2. Identify the acceptance criteria
3. Look at the external dependencies in the acceptance criteria
4. Start with unit test cases for the acceptance criteria relying on the
   external dependencies

### Refactoring

#### How often to refactor?

In the red-green-refactor cycle, the refactor step is optional in each cycle.
There's no hard rule on how often you should refactor, but it depends on the
complexity of the project and how confident you are. For a general guideline,
refactor when you sense that the code stinks, or when you are taking longer to
understand your code and add functionality.

For each cycle, the test writing and code writing should usually take a few
minutes only. This makes sure you are making small incremental steps in each
cycle.

Refactoring may takes longer, but if you are taking refactoring frequently
enough, it shouldn't take too long either.

#### What to refactor?

_Extract function_ refactoring is to move a piece of code into a separate
function. You do this when you find that the intention of a function does not
align well with the implementation, e.g. when implementation do more than the
intention, you extract the extra part into a separate function. This conforms
the single responsibility principle.

See [Refactoring.Com](https://refactoring.com/) for over 60 techniques for
refactoring.

#### Focus on design

During refactoring stage, you focus on the design, and should not add any
functionality.

### Two Approaches

The two approaches differ in there focus on which functionality our failing test
will cover. Usually we combine both approaches.

#### Inside-Out

Or called Bottom-Up. We start with the smallest components and work our way up.

- Focus on specific components (e.g. classes, methods)
- Collaborations between components evolve over time during refactorings
- TDD fully steers the design of the application
- An algorithmic style of development

#### Outside-In

- Start with a **collaborating** components and mock components that are not yet
  created.
- Once a component has been completed, start writing tests for a mock
  collaborator and begin TDD process again there
- Lends well to creating **integration** tests and unit tests
- Pair well with **_YAGNI_** (You ain't gonna need it) principle. It means that
  we should not implement functionality/feature until it is needed/required.

### Changing Tests

Except for external-facing API, changing tests are normal. For external-facing
API, tests should be agnostic to the implementation detail of the API. So
changing tests for external-facing API is dangerous.

### Structuring Test Cases

- Typically, one test class per one code class
- But when you split a code class, you may keep using one test class for
  multiple code classes
- Similarly, you may split a test class into multiple test classes for testing
  one single code class

## xUnit framework

### History

The most popular testing frameworks for various programming languages are
modelled on **_xUnit_** framework. It is designed by Ken Beck and Eric Gamma for
SmallTalk as sUnit, which is later ported to Java as jUnit, which became very
popular and inspired many other frameworks for other languages using the same
xUnit architecture. So knowledge on one xUnit framework is easily transferable
to other xUnit frameworks on other languages.

### xUnit Components

- Test case
- Test suite
- Test runner
- Test fixture
- Test result

#### Test fixture

A test **_fixture_** is a fixed state of the system required at the start of a
test. It ensures that the test is repeatable and consistent.

After each test, you may want to tear down the fixture to clean up the state of
the system, by destroying the objects created and reinitialising variables set
up during the fixture.

You set up the fixture in the **_setup()_** method and tear down the fixture in
the **_tearDown()_** method.

You can share the fixture and tearing down to multiple tests by using
annotations in jUnit5.

```java
public class TestMyClass {

  static private MyClass myClass;
  static private Random random;
  private int randomValue;

  @BeforeAll
  public static void setupClass() {
    myClass = new MyClass();
    random = new Random();
  }

  @BeforeEach
  public void setupTest() {
    randomValue = random.nextInt(100) + 1;
  }

  // Not really needed in this example
  @AfterEach
  public void tearDownTest() {
    randomValue = 0;
  }

  // Not really needed in this example
  @AfterAll
  public static void teatDownClass() {
    myClass = null;
    random = null;
  }
}
```

### jUnit Modules

- Platform - foundation for launching testing frameworks on JVM
- Vintage - for backward compatibility for jUnit 3 and 4
- Jupiter - offers new programming model and extension model

## Unit Testing

### Property of a Good Unit Test

Test the unit works correctly in ideal situation and also **fails correctly** in
error condition.

- Simple
- Focused
- Independent
- Flexible
- Easy to read

### What to test?

- Test for the requirements, not the methods.
- Test the behaviour, not the implementation.
- Test the public API, not the private methods.

### Arrange-Act-Assert Pattern

The 3 A's are a pattern of writing unit tests

- **_Arrange_**: Set up preconditions (e.g. create object, set up states,
  prepare input arguments)
- **_Act_**: Call the code under test
- **_Assert_**: Check the result

### Code Coverage

A test framework like Junit5 shows which line of codes have been executed when
test cases are run.

Code coverage is the proportion of code in each class which is executed.

Can be used to identify and remove **obsolete code**.

## Mocking

### Types of test doubles

**_Test doubles_** are objects that replace the external dependencies when
testing. There are many types of test doubles:

- **_Dummy_**: object of class that has no logic inside, but just to make the
  test compile
- **_Stub_**: object of class whose methods return _canned_ values (i.e. hard
  coded) to mimic restricted behaviour
- **_Spy_**: object of class that records the method calls and parameters passed
  to the method
- **_Fake_**: object of class that has a simplified version of the real
  implementation (i.e. not production ready)
- **_Mock_**: object of class that can have its behaviour contingently defined
  by each test as required. And can verify that the method calls are made with
  correct arguments. Usually created by a mocking framework.

### When to Mock?

- The class / method doesn't exist yet
- If it uses a lot of resources to create / interact with a class / database
- The test requires a specific scenario that is hard to reproduce when using an
  instance of the class such as an IOException.
- A class of non-deteministic behaviour

When **NOT** to mock:

- Don't mock entities - mock services
- Don't mock the class under test - mock the interface (don't mock the whole
  thing, mock the external-facing interface/contract)

### Stubbing

It means to return a fixed value for a method call. It's to mimic a restricted
behaviour of a class.

### Verification

To test a method that doesn't return any value (void return value), we can
verify that it calls a method with a specific argument for a specific number of
times.

## Reference

- [The Cycle of TDD](https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD)

## 🧭 Navigation

- [🔼 Back to top](#test-driven-development)
- [◀️ Back](../index.md)
- [🔖 Parent index](../index.md)
- [📑 Notes Index](../index.md)
