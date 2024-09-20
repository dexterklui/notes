---
date: 2024-09-19 (Thu)
---

# Date and Time

## Classes Representing Temporal Data

- `java.time.Instant`
- `java.util.Date`
- `java.util.Calendar`

`java.time` package was introduced in Java 8 to address the shortcomings of the
`java.util.Date` and `java.util.Calendar` classes.

## Concepts

### The difference between instant and date time

Imagine two persons, Alice and Bob. Both of them born at the same instant, but
they born in different time zones. Alice would reach 1 year-old at a different
instant than Bob, because we celebrate birthday for a date, not counting the
time elapsed since the instant of birth.

If we were to use an instant to represent the birthday, we would have to
celebrate a person's birthday on a different date for the local time zone, which
doesn't make sense.

See
[The difference between instant and date time - Medium](https://medium.com/@willchinelato/the-difference-between-instant-and-date-time-52fac96c64ab)
for more explanation.

## 🧭 Navigation

- [🔼 Back to top](#date-and-time)
- [◀️ Back](java.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
