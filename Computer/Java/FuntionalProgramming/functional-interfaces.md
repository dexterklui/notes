---
date: 2024-10-07 (Mon)
---

# Functional Interfaces

- Provides target **types** for [lambda expressions](lambda-expression.md) and
  method **references**.
- Each functional interface has a **single abstract** method, called _functional
  method_.

## Defining and Using Functional Interfaces

Use `@FunctionalInterface` annotation to define a functional interface.

> [!INFO]
>
> The annotation is not strictly required for the compiler to recognize a
> functional interface. But it communicates the design intent to the compiler so
> it can flag errors when the interface does not meet the requirements.

````java
```java
@FunctionalInterface
public interface MyFunction {
  int apply(T t);
}
````

```java
MyFunction func = (t) -> t.value + 1;
```

## Builtin Functional Interfaces

### Naming Conventions

- `Function`: unary function from T to R
  - `R apply(T t)`
- `Consumer`: unary function from T to void
  - `void accept(T t)`
- `Predicate`: unary function from T to boolean
  - `boolean test(T t)`, `Predicate negate()`, `Predicate or(Preciate)`
- `Supplier`: nullary function to R

Arity (number of arguments) prefixes:

- `Bi`: two arguments, e.g. `BiFunction` - binary function from T, U to R

Additional derived function shapes:

- `UnaryOperator`: unary function from T to T; extends `Function`
- `BinaryOperator`: binary function from T, T to T; extends `BiFunction`

With primitive types:

- `To<Type>` prefixes - e.g. `ToIntFunction`: unary function from T to int.
- Type arguments are specialized left to right.
- If there are specialization prefixes for all parameters, the arity prefix is
  left out.
- E.g. `DoubleToIntFunction`, and `ObjIntConsumer`

### Some Builtin Functional Interfaces

- `IntBinaryOperator`
- `ToLongBiFunction<T, U>`

### Using builtin functional interfaces

```java
BiFunction<Person, Integer, Integer> yearsToRetirement =
    (person, retirementAge) -> retirementAge - person.getAge();
int yearsLeft = yearsToRetirement.apply(person1, 65);
```

You can reuse an functional interface with [streams](streams-and-optional.md).

## 🧭 Navigation

- [🔼 Back to top](#functional-interfaces)
- [◀️ Back](../java.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)

```

```
