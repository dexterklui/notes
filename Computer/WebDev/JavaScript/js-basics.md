---
date: 2023-07-26 (Wed)
---

# JS Basics

## Data Types

### Dynamically and weakly typed

- **Dynamically typed**: type of variables are determined at runtime
- **Weakly typed**: type of variables can be implicitly inferred for operations
  between different data types.

### String

#### Instance methods

The argument `s` means a string.

| Method             | Description                               |
| ------------------ | ----------------------------------------- |
| `padStart(len)`    | Prepend spaces until target length        |
| `padStart(len, s)` | Use s as pad string; truncate s if needed |
| `padEnd(len)`      | Append spaces until target length         |
| `padEnd(len, s)`   | Use s as pad string; truncate s if needed |

#### Template literals

##### [Normal use of templates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

```javascript
`I am ${age} years old.`;
```

##### [Nesting templates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#nesting_templates)

```javascript
`Sie ${isWorking ? "arbeits" : `ist Student${gender === "F" ? "in" : ""}`}.`;
```

##### [Tagged templates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates)

###### Calling with tagged templates

Instead of using parenthesized arguments to call a function, we can call a
function with a template literal, as in `` console.log`Hello, ${name}!` ``. The
template literal is a **_tagged template_** and the function a **_tag_**.

In a call `` myTag`Hello, ${p1} and ${p2}!` ``, the tag function `myTag` will
receive three arguments:

- The first argument is a list of strings, which contains string fragments from
  the tagged template literal separated by expressions (given within `${}`).
  It's like applying the `split` method of a string, where the delimiters are
  the expressions in the template literal. So here the first argument will be
  `["Hello, ", " and ", "!"]`.
- The second argument is the evaluated value of the first expression `${p1}`,
  the third argument is `${p2}`, and so on if there're more expressions. In the
  tag function, you could use **rest operator** to collect all the expression
  values into a list: `myTag(strings, ...expressions)`.

###### Raw interpretation

Actually, the list in the first argument has a hidden property `raw` (like
`length` is a hidden property to any array), which is a list storing the same
sequence of strings but in **raw interpretation**. Hence, special escape
sequences like `\n` will be interpreted literally. I.e. its content is identical
to mapping each string fragment `s` to `` String.raw`s` ``. As in:

```javascript
console.log("a\tb"); // a	b
console.log(String.raw`a\tb`); // a\tb

function print(strings) {
  console.log(strings[0]);
  console.log(strings.raw[0]);
}
print`a\tb`; // output is the following:
// a	b
// a\tb
```

###### Passing arrow functions to tag function

When an expression in a tagged template is an arrow function, the arrow function
is passed to the tag function, unlike the case in untagged template literal
where the arrow function is stringified.

```javascript
console.log(`Hi, ${() => "Ben"}!`); // Hi, () => "Ben"!

function print(strings, fn) {
  console.log(strings[0] + fn() + strings[1]);
}
print`Hi, ${() => "Ben"}!`; // Hi, Ben!
```

###### Examples

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
console.log.bind(this, 2)`Hello${":"}`; // 2 [ 'Hello', '' ] :
new Function("console.log(arguments)")`Hello`;
// [Arguments] { '0': [ 'Hello' ] }

// recursive chaining
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

### Void

The keyword `void` represents and can be tested against both `null` and
`undefined`.

## Data Structure

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

## Hoisting

**_Hoisting_** is JavaScript's default behaviour of moving all declarations to
the **top** of the current scope. Note that hoisted variables are only declared
but **not initialized**.

But as a good practice, you should declare all variables at the top of the
scope. That can also prevent beginners from misunderstanding.

## Modules and Constructed Object

When creating an instance using constructor function in a module, JS will only
call the constructor once. That means if you import the module twice, you will
get the same instance of the object.

## Concepts

### Accessor function

An **_accessor_** is a function that takes an object and returns the desired
value of a particular field.

It is useful to refactor the code and passed around to do _dependency
injection_.

## 🧭 Navigation

- [🔼 Back to top](#js-basics)
- [🔖 Parent index](./index.md)
- [📑 Notes Index](../../../index.md)
