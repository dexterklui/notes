---
title: prettier
---

# Config File

For prettier, you can set the format by going to your project root directory,
and run `npx-prettier-init`. It will ask you questions about formatting
preference and then generate a `.prettierrc.json` (or yaml) file. The most basic
config I use for some filetypes is:

```json
{
  "tabWidth": 4,
  "overrides": [
    {
      "files": "*.md",
      "options": {
        "max_line_length": 80,
        "proseWrap": "always"
      }
    },
    {
      "files": ["*.json", "*.html", "*.js", "*.css", "*.mjs"],
      "options": {
        "tabWidth": 2
      }
    }
  ]
}
```

For the list of config options, and guidelines for configuration: see
[prettier config](https://prettier.io/docs/en/options.html).

# 🧭 Navigation

- [🔼 Back to top](#)
- [📑 Notes Index](../../index.md)
- [🗃️ Index](/media/mikeX/Nextcloud/index.md)
