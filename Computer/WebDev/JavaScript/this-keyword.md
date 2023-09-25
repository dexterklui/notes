---
title: The keyword "this"
---

# Example 1

## Code snippet

```javascript
class TestThis {
    constructor() {
        this.data = "Some data";
    }

    transform(text) {
        return `${text} ${this.data}`;
    }

    echo(texts) {
        const array = texts.map((text) => this.transform(text)); // No problem
        const array = texts.map(this.transform); // Can't find `this.data` in `transform()`
        const array = texts.map(this.transform.bind(this)); // No problem
        console.log(array);
    }
}
const myTest = new TestThis();
myTest.echo(["1:", "2:"]);
```

## Explanation

What actually happens in the last line of code:

### Passing arrow function `(test) => this.transform(text)`

1.  `myTest` calls the method `echo()`

2.  In its body, `echo()` passes (the reference to) the array function

    ```javascript
    (text) => this.transform(text);
    ```

    to `map()`. The array function, now being declared, fixes the `this` in its
    body to be the current lexical context, which is the object `myTest`.

3.  `map()` calls the arrow function once for each element in `texts`

4.  In each iteration, the arrow function calls `myTest.transform()` with the
    element it takes. So in the body of the method `transform()`, the `this`
    refers to `myTest`, and hence it can find the field `data`.

### Passing method without binding `this.transform`

1.  `myTest` calls the method `echo()`

2.  In its body, `echo()` tries to pass `this.transform` to `map()`.

    1.  First, The `this` here is evaluated to `myTest`.
    2.  Then, `transform()`'s **reference** (i.e. the value stored in
        `myTest.transform`) is passed to `map()`.

3.  `map()` directly calls the method `transform()` once for each element in
    `texts`

4.  In each iteration, `transform()` tries to evaluate `this.data`, but here
    `this` refers to the caller which is `map()` or some other functions `map()`
    uses to call `transform()`. So the field `data` is not found.

### Passing method with binding `this.transform.bind(this)`

1.  `myTest` calls the method `echo()`

2.  In its body, `echo()` tries to pass `this.transform.bind(this)` to `map()`.

    1.  First, both `this`'s are evaluated to `myTest`.
    2.  Then `myTest.transform` calls the method `bind(myTest)`, which returns a
        new function whose body is the same as `myTest.transform` except that
        the `this` in the body is fixed to `myTest`.
    3.  The **reference** to this new function (let's us call it
        `bindedTransform()` just for for later use) is passed to `map()`

3.  `map()` directly calls the method `bindedTransform()` once for each element
    in `texts`

4.  In each iteration, `bindedTransform()` is able to access `myTest.data` (the
    original `this` was already replaced by `bind()`).
