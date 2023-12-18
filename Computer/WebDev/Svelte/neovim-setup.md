---
title: NeoVim Setup
date: 2023-10-24 (Tue)
---

# Automatic Types

Here's a [blog post](https://svelte.dev/blog/zero-config-type-safety) about
automatic types.

To enable it in NeoVim, make sure you install the latest **Svelte LSP server**
and also the
[Svelte TypeScript plugin](https://www.npmjs.com/package/typescript-svelte-plugin).

# PostCSS

You will need to install this
[PostCSS parser plugin](https://github.com/postcss/postcss-scss#2-inline-comments-for-postcss)
to enable `//` inline comments within `<style lang="postcss">` in a svelte
component file. This is because for the time being, nvim-treesitter takes uses
SCSS parser within this tag, and comment keymap will use `//` inline comments.

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../../index.md)
- [🗃️ Master Index](../../../../index.md)
