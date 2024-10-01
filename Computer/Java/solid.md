---
date: 2024-07-25 (Thu)
---

# SOLID Principles

## OO Design

Good design:

- Code Reuse
  - E.g. loop, functions, library
- Scalability
  - Can handle increase in work or number of users
- Extensibility
  - Easily extended, modified
- Maintainability
  - Holds up to changing user needs
- Security

Bad design:

- Rigid- Difficult to change
- Fragile - Easy to break
- Immobile - Hard to extract reusable components
- Repetitive
- Needlessly Complicated - "Over-designed"

## Four Pillars of OOP

### 4 Pillars of OOP

- Abstraction

  Hiding complex implementation details while exposing only the high level
  essential features of an object.

- Encapsulation

  Binding the data and the code that operates on the data into a single unit.

- Inheritance

  A mechanism in which one object acquires all the properties and behaviours of
  a parent object.

- Polymorphism

  The ability of an object to take on many forms. The most common use of
  polymorphism in OOP occurs when a parent class reference is used to refer to a
  child class object.

### Polymorphism

Two types:

- Overriding - runtime polymorphism
- Overloading - compile time polymorphism

You should add `@Override` (without semicolon) annotation above the overriding.

To access the overridden method, use `super.overriddenMethod()`.

`final` method doesn't allow subclass to override this method - otherwise
there's a compile time error

## Single Responsibility

A class should have only one reason to change. Limit the scope of your change.

Associated concepts:

- Encapsulation
- Cohesion: How closely the class is related to its single purpose. Classes with
  high cohesion are easier to maintain and less frequently changed.

## Open Closed Principle

Classes and functions should be open for extension but closed for modification

Associated concepts:

- Abstraction
- Polymorphism

## Liskov Substitution Principle

Subclasses should be substitutable for their base classes.

Related concepts:

- Inheritance
- Polymorphism

## Interface Segregation Principle

Interfaces should be small, each dealing with one aspect of a problem.

A class should not be forced to implement irrelevant behaviour.

## Dependency Inversion Principle

Classes should depend upon abstract concepts, rather than concrete
implementations.

Abstractions should not depend on details, details should depend on
abstractions.

## 🧭 Navigation

- [🔼 Back to top](#solid)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
