---
title: Extended BNA Notation
---

# Basics of BNA

**_Backus-Naur form_** or **_Backus normal form (BNF)_** is a meta is a
**_metasyntax_** notations, i.e. to express the syntax of aother notation.

A BNF specification is a set of **derivation rules**:

```
<symbol> ::= __expression__
```

-   `<symbol>` is a nonterminal (variable).
-   `__expression__` consists of one or more sequences of terminal or
    nonterminal symbols.
-   `::=` means that the symbol on the left must be replaced with the expression
    on the right.
-   A `|` in the expression means an alternative choice.

Symbols that never appear on the left side are **_terminals_**. Otherwise they
are non-terminals and are always **enclosed between** `<>`.

# Basics of EBNA

**_Extended BNA Notation (EBNF)_** extends BNA.

A valid EBNF consists of terminal symbols and non-terminal **_production
rules_**.

-   Terminal symbols are tokens that can't be changed by the language grammar.
-   Production rules stipulate how legal sequence can be formed.

```
digit excluding zero = "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;
digit                = "0" | digit excluding zero ;
```

-   `"` surrounds terminal symbols.
-   `|` represents alternatives
-   `;` is a terminating character

A sequence of terminals are separated by a comma `,`:

```
two hundred one = "2", "0", "1" ;
three hundred twelve = "3", twelve ;
```

```
natural number = digit excluding zero, { digit } ;
integer        = "0" | [ "-" ], natural number ;
```

-   `{ }` represents that the content can appear 0 or more times.
-   `[ ]` represents that the content is optional, i.e. 0 or 1 time.

# References

[Wikipedia](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form)

# 🧭 Navigation

-   [🔼 Back to top](#)
-   [📑 Notes Index](../index.md)
-   [🗃️ Index](/media/mikeX/Nextcloud/index.md)
