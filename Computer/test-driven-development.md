---
date: 2024-06-03 (Mon)
---

# Test Driven Development

## Three Laws of TDD

1. You must write a failing test before you write any production code.
2. You must not write more of a test than is sufficient to fail, or fail to
   compile.
3. You must not write more production code than is sufficient to make the
   currently failing test pass.

This can be seen as _red-green-refactor_ cycle.

1. Create a unit tests that fails
2. Write production code that makes that test pass.
3. Clean up the mess you just made.

The philosophy is based on the idea that our limited minds are not capable of
pursuing the two simultaneous goals of all software systems: 1. Correct
behaviour. 2. Correct structure. So the RGR cycle tells us to first focus on
making the software work correctly; and then, and only then, to focus on giving
that working software a long-term survivable structure.

1. Make it work
2. Make it right
3. Make it fast

## Reference

- [The Cycle of TDD](https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD)

## 🧭 Navigation

- [🔼 Back to top](#test-driven-development)
- [◀️ Back](../index.md)
- [🔖 Parent index](../index.md)
- [📑 Notes Index](../index.md)
