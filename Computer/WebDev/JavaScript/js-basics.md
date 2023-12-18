---
title: Basics
date: 2023-07-26 (Wed)
---

# Data Types

## Dynamically and weakly typed

- **Dynamically typed**: type of variables are determined at runtime
- **Weakly typed**: type of variables can be implicitly inferred for operations
  between different data types.

## String

### Instance methods

The argument `s` means a string.

| Method             | Description                               |
| ------------------ | ----------------------------------------- |
| `padStart(len)`    | Prepend spaces until target length        |
| `padStart(len, s)` | Use s as pad string; truncate s if needed |
| `padEnd(len)`      | Append spaces until target length         |
| `padEnd(len, s)`   | Use s as pad string; truncate s if needed |

### Template literals

#### [Normal use of templates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

```javascript
`I am ${age} years old.`;
```

#### [Nesting templates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#nesting_templates)

```javascript
`Sie ${isWorking ? "arbeits" : `ist Student${gender === "F" ? "in" : ""}`}.`;
```

#### [Tagged templates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates)

It is a new way of calling a function with arguments. When calling a function
this way, we call the function a **_tag_**.

1.  The template literals are broken into segments of string values and
    expression values (i.e. evaluated `${}`)
2.  All string values are passed to the function in an array as the first
    argument
3.  Each expression value is passed one by one as a single argument
4.  Function body executes normally, and it can return anything like a normal
    function.

```javascript
const person = "Mike";
const age = 28;

function myTag(str, person, age) {
  const ageStr = ageExp > 99 ? "centenarian" : "youngster";
  return `${str[0]}${person}${str[1]}${age}${str[2]}`;
}

const output = myTag`That ${person} is a ${age}.`;
console.log(output); // That Mike is a youngster.
```

```javascript
console.log`Hello`; // [ 'Hello' ]
console.log.bind(1, 2)`Hello`; // 2 [ 'Hello' ]
new Function("console.log(arguments)")`Hello`;
// [Arguments] { '0': [ 'Hello' ] }

function recursive(strings, ...values) {
  console.log(strings, values);
  return recursive;
}
recursive`Hello``World`;
// [ 'Hello' ] []
// [ 'World' ] []
```

- Untagged template literals are strings and will throw a `TypeError` when
  chained.

- Optional chaining will throw a syntax error:

  ```javascript
  console.log?.`Hello`
  console?.`Hello`
  ```

## Void

The keyword `void` represents and can be tested against both `null` and
`undefined`.

# Data Structure

- `Map`
  - any data type can be used as key
  - Iterable
- `WeakMap`:
  - Only allow objects as key
  - Keys are stored weakly, i.e. even if an object is used as a key for a
    WeakMap, when there's no more reference to it, it is still eligible for
    garbage collection. So it is useful for associating data with an object that
    may be destroyed, like a **DOM node**.
  - Non-iterable
  - Doesn't have a `size` property that returns the number of key-value pairs,
    because it can change on garbage collection
  - Have additional garbage collection overhead performance-wise
  - `new WeakMap<key, value>([iterable])`;

# Hoisting

**_Hoisting_** is JavaScript's default behaviour of moving all declarations to
the **top** of the current scope. Note that hoisted variables are only declared
but **not initialized**.

But as a good practice, you should declare all variables at the top of the
scope. That can also prevent beginners from misunderstanding.

# Modules and Constructed Object

When creating an instance using constructor function in a module, JS will only
call the constructor once. That means if you import the module twice, you will
get the same instance of the object.

# Concepts

## Accessor function

An **_accessor_** is a function that takes an object and returns the desired
value of a particular field.

It is useful to refactor the code and passed around to do _dependency
injection_.

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
- [🗃️ Master Index](../../../index.md)
