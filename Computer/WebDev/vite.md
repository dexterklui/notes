---
date: 2024-09-20 (Fri)
---

# Vite

## Quick Start

- Set up import [path resolution](#resolve-alias) with `tsconfig.json` and
  `vite-tsconfig-paths` plugin.
- Quick start with [prettier](prettier.md)

## ⚙️ Config

### Config File

Config file is `vite.config.js` (or `.ts`) in project root.

The most basic content is:

```javascript
export default {
  // config options
};
```

To enable intellisense in the config file, add either jsdoc annotation:

```javascript
/** @type {import('vite').UserConfig} */
```

Or import:

```javascript
import { defineConfig } from "vite";
```

### Conditional Config

You can export a function instead to conditionally determine options based on
the command (`serve` or `build`), the mode, if it's an SSR build, or is
previewing.

```javascript
export default defineConfig(({ command, mode, isSsrBuild, isPreview }) => {
  if (command === "serve") {
    return {
      // dev specific config
    };
  } else {
    // command === 'build'
    return {
      // build specific config
    };
  }
});
```

It is important to note that in Vite's API the `command` value is `serve` during
dev (in the cli `vite`, `vite dev`, and `vite serve` are aliases), and `build`
when building for production (`vite build`).

### Shared Options

#### resolve

##### resolve alias

`resolve.alias` is used to define aliases for paths.

- Type:
  `Record<string, string> | Array<{ find: string | RegExp, replacement: string}>`
  - For record, the key is the alias and the value is the path.
  - When using array, `find` is the alias and can accept a string or a regexp.

It will be passed to `@rollup/plugin-alias` as its
[entries option](https://github.com/rollup/plugins/tree/master/packages/alias#entries).

But better yet, you can use vite plugin
[vite-tsconfig-paths](https://github.com/aleclarson/vite-tsconfig-paths), so you
don't need to duplicate your
[`paths`](https://www.typescriptlang.org/tsconfig/#paths) resolution setting
from `tsconfig.json` to this shared option of vite.

## 🔗 Resources and Links

## 🧭 Navigation

- [🔼 Back to top](#vite)
- [◀️ Back](web_dev.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
