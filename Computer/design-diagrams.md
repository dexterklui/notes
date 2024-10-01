---
date: 2024-10-01 (Tue)
---

# Design Diagrams

## OOP

### Dependency

The manner and strength of dependency between two objects.

#### Four levels of dependencies

In ascending order of dependency strength:

1. **_Dependency_**

   Use an object of another class as method **argument** or **return type**. Use
   **broken line arrow**, pointing to the class that is depended on. Use-a
   relationship.

2. **_Association_**

   Has-a relationship. Use **_solid line arrow_**, pointing to the class that is
   depended on. Should use dependency injection, when having an association
   relationship.

3. **_Aggregation_**

   One class has multiple objects of another class as attributes, e.g.
   `ArrayList<Trainee>`. Use **solid line with an diamond** representing an
   aggregation (diamond on the side of the class using the other class).

   The class being depended on can exists stand alone, without being inside
   another class.

4. **_Composition_**

   An object of one class **only** exists within another class. Composition is
   coded using an **_inner class_**. The inner class cannot be used outside, and
   only exists in the outer class. Use a **solid diamond** to represents a
   composition. The direction is same as aggregation.

### UML

#### Dependencies

For edges representing dependencies, refer to the
[Four Levels of Dependencies](#four-levels-of-dependencies).

#### Inheritance

Use a solid line with a solid triangle pointing to the super class.

For implementing an interface, use a **dashed line** with a **hollow triangle**
pointing to the interface.

#### Static

To represent _static_ members, **underline** it.

#### Access Modifier

- `+`: public
- `-`: private
- `#`: protected

#### Abstract Class and Methods

Use italic

#### Final

Use Capitalized name

## 🧭 Navigation

- [🔼 Back to top](#design-diagrams)
- [◀️ Back](../index.md)
- [🔖 Parent index](../index.md)
- [📑 Notes Index](../index.md)
