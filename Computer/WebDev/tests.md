---
date: 2023-09-26 (Tue)
---

# Tests

## Testings

### Different levels of testing

#### Unit tests

Unit tests \[Jest\] (automated): These are low-level tests that sit close to the
application code. Generally, they are concerned with testing individual methods
and classes.

#### Integration tests

Integration tests (automated): These are slightly higher level and ensure
different parts of our application interact correctly. For example, a server and
a database.

#### Functional tests

Functional testing (automated): These confirm the business logic is correct.
They are different from integration tests in that they don’t look at the
internal state, they only verify behavior.

#### End-to-end tests

End-to-end testing \[Cypress\] (automated): These replicate complex user flows
across the whole application. They confirm that end users can interact with our
program as expected. They are the most complex and costly.

#### Acceptance tests

Acceptance testing \[Storybook\] (automated/ generally manual): This is where we
check to make sure that what we’ve built satisfies our business requirements.
This could include putting it in front of your product manager and making sure
they’re happy.

#### UI tests

UI testing \[Storybook\] (automated): Ensures everything appears on screen and
functions correctly. Concerned with things like ‘When I press this button, does
it do what I expect?’.

#### Smoke tests

Smoke testing (automated/ manual): When we create a new version of an
application we run these initial tests to make sure that nothing catches fire
when we turn it on. They are a minimal suite of tests to confirm we’ve not
broken anything.

#### Canary tests

Canary testing: This reduces risk by initially releasing new versions to a small
number of users, then expanding the group as we verify there are no issues.

#### Performance tests

Performance testing: A brilliant article on the subject is
[here](https://jc1175.medium.com/a-developers-guide-to-performance-testing-5c69d02a8841).
Performance testing ensures the scalability, stability and reliability of your
web services.

### Test pyramid

This dictates how many of each test you should use. See this
[online book](https://martinfowler.com/articles/practical-test-pyramid.html).

### Testing public behaviour not implementation

See
[Testing implementation Details](https://kentcdodds.com/blog/testing-implementation-details).

- Testing implementation details will lead to false positive (unreported error)
  and false negative (reported non-error) after refactoring
- Test from the perspective of the two users (end users and developers)
  - But don't let the tests to be the third user that you need to cater
- Only tests the things that the two users would see or interact with.
- Write down a list of instructions for that user to manually test that code to
  make sure it's not broken.
- Turn this list of instructions into an automated test.

## Conditional export for testing

Often you should only test the public functions. Exposing private functions just
for the sake of testing is not considered a good practice. If the functions are
generic enough, you could expose them publicly with explicit unit tests, but if
they're not, then you're making your tests highly coupled with the
**implementation details**, so they can break (for example) on refactoring
without any real change in the behaviour.

But if the logic of a public functionality is very complicated and calls many
private functions, a testing failure of the public function cannot give you much
info about where the bug is.

In this case, you can do a **conditional** export of a variable that stores the
reference of your private functions.

```javascript
function shouldntBeExportedFn() {
  // Does stuff that needs to be tested but is not for use outside
}

export function exportedFn() {
  // Is for use outside, and uses other functions in this package
}
let exportedForTesting = null;

if (process.env.NODE_ENV === "test") {
  exportedForTesting = {
    shouldntBeExportedFn,
  };
}

export { exportedFn, exportedForTesting };
```

## Tools

Some tools that can integrate well with testing and for CI

- husky
- lint-staged

## With TypeScript

Sometimes you need to pass an argument of invalid type to a function during
testing, and TypeScript linter will complain. To suppress these kinds of error,
use `@ts-expect-error` comment on the line above. The difference of it from
`@ts-ignore` is that `@ts-expect-error` will alert us if there is in fact no
type error on the next line.

## Mock

### Pros and Cons

Pros:

- Lift the reliance on dependencies, allow pinpointing the location of bug
- Improve performance by mocking result that originally came from complex logic

Cons:

- Testing the implementation makes refactoring difficult
- More complicated tests and complicated code (to allow for mocking)

### Mocking inner functions

See
[jest mock inner function](https://stackoverflow.com/questions/51269431/jest-mock-inner-function)

However the method of importing self module to allow mocking **no longer works
directly**, because `jest.spyOn` relies on the quirk of old CJS import. But the
industry is migrating to ESM import. E.g. Next.js is using SWC to transpile
code, and SWC follows the specification of ESM import. In ESM, entities imported
through ES modules are read-only live bindings, i.e. they are immutable, and so
`jest.spyOn()` cannot mock them by mutating the bindings. So it seems improbable
that `jest.spyOn()` will be ever be available. References:

- https://github.com/aelbore/esbuild-jest/issues/26#issuecomment-1387675405
- https://github.com/swc-project/swc/issues/3843#issuecomment-1062817988
- https://subscription.packtpub.com/book/programming/9781839214110/2/ch02lvl1sec12/esm-ecmascript-modules

Basically, if we want to mock inner functions for better testing performance and
isolation, the most straight-forward way is to put inner functions into a
**separate module file**. It depends on whether you think the necessity is big
enough to justify separating one logically connected functionality into multiple
files.

I figure out a more _hacky_, _convoluted_ and perhaps _ugly_ way that doesn't
require splitting files. Basically you wrap the inner function `innerFn()` in an
object `_innerFn = {_: innerFn}`, then let other functions that depend on it to
call the inner function through the object `_innerFn._()`. When exporting, you
further wrap all these wrapper objects into one `test` object
(`let test = {_innerFn}`), but changing `test` to be undefined when
`process.env.NODE_ENV !== "test"` before exporting it.

After all these works, you can use `jest.spyOn()` to mock the inner function.
See the following illustration

```ts
function innerFn() {
  // innerFn() logic...
}
const _innerFn = { _: innerFn };

function testFn() {
  const someValue = _innerFn._(); // calls innerFn() through the wrapper
  // rest of the logic...
}

let test = { _innerFn };
if (process.env.NODE_ENV !== "test") {
  test = undefined as any;
}
export { test };
```

Then in the test file:

```ts
describe("testFn", () => {
  it("should return correct value", () => {
    const spy = jest.spyOn(test._innerFn, "_");
    spy.mockReturnValue("some value");

    expect(testFn()).toBe("some checking value");

    spy.mockRestore();
  });
});
```

## 🧭 Navigation

- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
