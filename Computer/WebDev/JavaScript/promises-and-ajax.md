# Promises and AJAX

## Overview of promise

A `Promise` object represents the eventual completion (or failure) of an
asynchronous operation and its resulting value.

## States and fates

### States of promise

- _pending_: initial state
- _settled_: to either of the two _eventual states_
  - _fulfilled_: operation completed successfully
  - _rejected_: operation failed

After a promise is settled, its associated handlers queued up by `then()` or
`catch()` are called asap, even if the handlers are attached afterwards.

### Fates of promise

A promise is always in one of the two mutually exclusive fates: resolved or
unresolved.

Also a promise is said to be **_resolved_**, when it is settled or "locked-in"
to match the eventual state of another promise, thus further attempt to resolve
the promise will have **no effect**.

So a resolved promise can be in one of three states, not just fulfilled:

- _fulfilled_ - When resolved to:
  - Non-promise value
  - Thenable which will call any passed fulfilment handler asap
  - Another fulfilled promise
- _Rejected_ - When:
  - Rejected directly
  - Resolved to a thenable which will call any passed rejection handler asap
  - Resolved to another promise that is rejected
- _Pending_ - When resolved to:
  - Thenable which will call neither handler asap
  - Another promise that is pending

## Handling resolved promise

We use `then()` method to attach callback functions. It takes two arguments:

1. Callback function to handle the fulfilment value
2. **Optional**: Callback function to handle rejection value

`then()` returns a newly generated promise, whose fulfilment value is the
**return value** of the executed callback. So remember to add return statement
if you want to **chain** further `then()`.

In case of a rejected promise and there is no rejection callback passed,
`then()` returns an equivalent rejected promise it takes for further handling
down the chain.

The callback functions should take the resolved result and handle it:

```javascript
prompise1.then(
  (result) => {
    console.log(result);
  },
  (error) => {
    console.error(error);
  },
);
```

## Handling rejected promise

Besides passing a rejection callback as a second argument to `then()`, we can
also use `catch()`. It takes a callback for **rejected promise** or **thrown
error**.

If there is no rejection callback passed to `then()`, then `then()` will return
the same promise if the promise is rejected. So you can attach a rejection
callback at the end of the chain with a `catch()`:

You can continue chaining from a `catch`'s returned promise like `then()`'s.

```javascript
promise2
  .then((result) => {
    // do something
  })
  .then((result) => {
    // do something more
  })
  .catch((err) => {
    // handle rejection
  });
```

## Cleaning up settled promise

`finally()` takes a callback and calls it when the promise is _settled_. The
callback function does **not** receive any argument. So it is usually used for
doing processing or clean up regardless of the outcome.

It immediately returns an **equivalent promise** for further chaining. I.e. the
return value of the callback won't affect the promise returned by `finally()`.
But a `throw` in the callback still rejects the returned promise.

## Creating Promise

### Creating unresolved promise

```javascript
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("foo");
  }, 300);
});
```

The constructor `Promise()` accepts a function, which can accepts two arguments.

1. A _resolve_ callback - The function calls it to fulfil the promise and passes
   the result to it, if any.
2. A _reject_ callback - The function calls it to reject the promise and passes
   the reject reason to it (usually an error).
   - Any uncaught error thrown by the function will also reject the promise,
     unless the promise has already been resolved.

Once a promise is created, it will execute the function it accepts, and wait for
the function to resolve it. If the function returns without resolving the
promise, the promise will remains in pending forever.

Note that if you pass another promise to the resolve or reject callback, the
original promise's state will be _locked in_ to the promise passed. It can be
said to be resolved but still not settled. See
[Relating states and fates](#relating-states-and-fates).

### Wrapping callback-based API

```javascript
const countDown = new Promise((resolve) =>
  setTimeout(() => resolve("done"), 2000),
);
countDown.then((res) => console.log(res));
```

Promise can be created to wrap asynchronous functions that use a callback-based
API, i.e. accepts a callback function to handle the result.

The function passed to the `Promise()` constructor acts as a proxy by providing
a function closure for the asynchronous functions to use the promise's resolve
and reject callbacks.

### Creating resolved promise

`Promise.reject(reason)` returns a new rejected promise with a reason.

`Promise.resolve(value)` returns a new resolved promise with a value.

- If the value is thenable then the promise state is locked-in to it.
- Otherwise the promise fulfils with the value.

Generally if don't know if a value is a promise or not, `Promise.resolve(value)`
it instead.[^1]

## Handling multiple promises

All the following static methods takes an iterable of promises and returns a
single promise.

- `Promise.all()`

  - Fulfils when all input promises **fulfil**, with an array of fulfilment
    values (with an empty if input is empty).
  - Rejects when any promise reject, with this first rejection reason.

  Typically used for multiple tasks that depend on the success of others.

- `Promise.allSettled()`

  Fulfils when all input promises **settle**, with an array of objects that
  describe each outcome. Each object has properties:

  - `status`: Either `"fulfilled"` or `"rejected"`.
  - `value`: Only present for fulfilled promises.
  - `reason`: Only present for rejected promises.

  Typically used for multiple independent asynchronous tasks.

- `Promise.any()`

  - Fulfils when any of the input promises fulfil, with the first fulfilment
    value.
  - Rejects when all input promises reject, with an `AggregateError` containing
    an array of rejection reasons.
    - Also rejects if the input contains no pending promise.

  The errors in AggregateError are in the order of the promises passed.

- `Promise.race()`

  Settles with the eventual state of the first promise that settles. I.e.

  - Fulfils when the first settled promise is fulfilled.
  - Rejects when the first settled promise is rejected.
  - Remains pending forever if the input is empty.
  - Still settles asynchronously if input is non-empty but contains no pending
    promises.

## AJAX

See [AJAX](../ajax.md).

## References

- [Example with diverse situations](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise#example_with_diverse_situations)

[^1]:
    [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise#static_properties](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise#static_properties)

<!-- vim: set fdl=1: -->
