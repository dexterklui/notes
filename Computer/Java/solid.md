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
- Bad design:

- Rigid- Difficult to change
- Fragile - Easy to break
- Immobile - Hard to extract reusable components
- Repetitive
- Needlessly Complicated - "Over-designed"

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
