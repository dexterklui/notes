---
date: 2023-09-18 (Mon)
---

# programming paradigm

## Composition Better Than Inheritance

- A YouTube video,
  [The Flaws of Inheritance](https://youtu.be/hxGOiiR9ZKg?si=sO-EC10vrYYH-US0),
  with examples.

Inheritance forces all the common elements into a parent class. Hence **the more
perfect an inheritance structure is to the existing code base, the more hostile
your code is to new changes**. That is, it is more easy for a change to
introduce exceptions that break the existing inheritance structure that require
an enormous amount of change to many different classes in the inheritance
structure.

Instead of inheritance, use interfaces. It combines into a minimal set of
contracts, unlike superclasses that combines into a maximal set of contracts.
Then we use **_dependency injection_**[^1] and use interface to describe the
kind of argument a function is expecting to receive, where interface is for
establishing a contract of what the argument can do.

## 🧭 Navigation

- [🔖 Parent index](../index.md)
- [📑 Notes Index](../index.md)

## Footnote

[^1]:
    YouTube video,
    [Dependency Injection, The Best Pattern](https://youtu.be/J1f5b4vcxCQ?si=eV_pZnYbXp-daLpv)
