---
date: 2023-11-03 (Fri)
---

# PostCSS

## Plugins

### PostCSS Global Data

- [Socket page](https://socket.dev/npm/package/@csstools/postcss-global-data)

It lets you inject CSS that is removed again before the final output. This is
useful for plugins that use global CSS as data.

E.g. for PostCSS Custom Media, other files that use the custom media cannot
access the custom media defined (even in the global CSS) during compilation,
because the global CSS is a separate file and only active after compilation, but
is compartmentalized during compilation.

So in order for other files to access the custom media, you have to use this
plugin to inject the custom media data into each file compilation.

NOTE that this plugin will remove the injected code in the CSS file after the
compilation. What this does is that it will do a static injection during
compilation. So if you have rules that you want to be present not only to each
CSS file during compilation for static injection, but also present in the final
output, then you still have to import that CSS file besides using this plugin to
inject it. This is because static injection is only done by PostCSS plugins.

E.g. PostCSSPresetEnv allows you to use the experimental feature _Relative Color
Syntax_, as in `rgb(from var(--my-color) r g b / 20%)`. Normally, this won't
work if it is used in a CSS module but `--my-color` is defined in a separate
global CSS, because then the module won't have access to `--my-color` during
compilation for static injection. So you have to use this plugin to inject that
global CSS. Yet, injecting the global CSS alone is not enough. If there are
other normal CSS rules that rely on `--my-color`, such as
`p {color: var(--my-color)}`, this CSS rule won't have any static injection done
because it's just a standardized CSS rule. So if you only inject the global CSS
but not also import it, during run time `--my-color` will be undefined and all
other standardized CSS rules that depend on it will break.

## 🧭 Navigation

- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
