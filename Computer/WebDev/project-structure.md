---
title: Project Structure
date: 2023-11-08 (Wed)
---

# Project Structure

## Component Tree

### Concept

[Component Tree Structure](https://medium.com/better-programming/a-better-frontend-component-structure-component-trees-5a99ed6d1ece)
is a directory structure that mirrors a component tree, making it very clear
about the relationship and dependency between different component. It solves the
problem of naming and organizing components and their directories.

### General structure

The idea is to use a directory structure to mirror a component tree. In a
component tree directory structure, there are two types of directories:

1. A **main** component directory sharing the same name with the main component
   inside (except there's no name sharing for `+page` or `+layout` component in
   a routing directory).
2. A **subcomponent** directory with the name **`components`** (or `_components`
   inside `routes/` to distinguish from a routing directory).

When a component must exist as a child of another component, i.e. it only make
sense to appear as a child component of it's parent (like _Block_-_Element_
relation in _BEM_ convention), it should be in the subcomponent directory of its
parent's directory:

```
📂Card/
│ 📂components/
│ │ CardHeader.svelte
│ │ CardBody.svelte
│ └ CardImg.svelte
└ Card.svelte
```

But vice versa isn't true. Being inside a subcomponent directory doesn't mean
conceptually it must exists as a child component of its parent. In the
following, `CopyrightIcon.tsx` is inside the subcomponent directory of
`Layout.tsx` only because it is the smallest scope for all other elements that
needs it (`Heading.tsx` and `Footer.tsx` and their descendants can import it).

```
📂components/
│ 📂Layout/
│ │ 📂components/
│ │ │ 📂Heading/
│ │ │ │ 📂components/
│ │ │ │ │ Logo.tsx
│ │ │ │ └ Menu.tsx
│ │ │ └ Heading.tsx
│ │ │ CopyrightIcon.tsx
│ │ └ Footer.tsx
│ └ Layout.tsx
└ BoxContainer.tsx
```

This way, you can be sure that, no other outer scope components up the tree,
except its own direct parent `Layout.tsx`, will use the `Copyright.tsx`
component. This ability to help you figure out the logical and dependency
relationship among different components is what Component Tree structure is
about. And this brings us to the import restriction rule of Component Tree.

### Import restriction

Component import rules:

- Can import upwards, except its own parent
- Can import siblings
- Cannot import sibling’s components
- Cannot import its parent
- Cannot import internal components of a sub-component
- Cannot import internal components of a higher-level component

### eslint enforcement

See the link above for recommendation. Here's an example setup:

1. Install `eslint-plugin-import` as a dev dependency.
2. Configure `.eslintrc.js`

```javascript
module.exports = {
  plugins: [
    /* other plugins */
    "eslint-plugin-import",
  ],
  rules: {
    /* other rules */
    "no-restricted-imports": [
      "error",
      {
        patterns: [
          {
            group: ["../**/components/**/*.svelte"],
            message:
              "Do not import from a higher level component's internal components, move that component further up the directory tree instead.",
          },
          {
            group: ["**/components/**/components/**/*.svelte"],
            message:
              "Do not import internal components used by a sub-component, move that component further up the directory tree instead.",
          },
          {
            group: ["./*/components/**/*.svelte"],
            message:
              "Do not import internal components used by a sibling, move that component further up the directory tree instead.",
          },
        ],
      },
    ],
  },
};
```

### Reference link

See
[Component Trees - Medium](https://medium.com/better-programming/a-better-frontend-component-structure-component-trees-5a99ed6d1ece).

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](../../index.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
- [🗃️ Master Index](../../../index.md)
