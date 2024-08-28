---
date: 2024-08-22 (Thu)
---

# Spring Validation

## Annotations from Validation

- `@Max(value)`
- `@Min(value)`
  - `BigDecimal` and `BigInteger`
- `@NotNull` (not NonNull)
- `@Email`
- `@NotBlank`
- `@Pattern(regexp = "...")`
- `@Positive`
- `@Negative`
- `@Size(min = 8, max = 15)`

Hibernate validation annotations don't work with Validation module's `@Valid`
annotation used in `@Controller` methods

## Validation

```java
@PostMapping
public void addContact(@Valid @RequestBody Contact trade) {
    // ...
}
```

## 🧭 Navigation

- [🔼 Back to top](#spring-validation)
- [◀️ Back](spring.md)
- [🔖 Parent index](../index.md)
- [📑 Notes Index](../../../index.md)
