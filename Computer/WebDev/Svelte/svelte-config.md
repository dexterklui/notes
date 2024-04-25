---
date: 2023-12-21 (Thu)
---

# Svelte Config

It should have one default export, which is an config option object.

- `extensions`: Array of file extensions to be treated as Svelte components.
  Actually, it selects the files to be processed by preprocessors.
- `preprocess`: Array of preprocessors to be applied to components
- `kit`: Options for SvelteKit

```javascript
const config = {
  extensions: [".svelte", ".md"],
  preprocess: vitePreprocess(),
  kit: {
    adapter: adapter(),
  },
};
```

## 🧭 Navigation

- [🔖 Parent index](index.md)
- [📑 Notes Index](../../../index.md)
