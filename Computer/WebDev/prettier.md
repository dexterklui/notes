# prettier

## Config Files

There's no global configuration. It is configured in the following order of
precedence:

- A `prettier` key in your package.json file.
- A `.prettierrc` file written in JSON or YAML.
- A `.prettierrc.json`, `.prettierrc.yml`, `.prettierrc.yaml`, or
  `.prettierrc.json5` file.
- A `.prettierrc.js`, or `prettier.config.js` file that exports an object using
  export default or module.exports (depends on the `type` value in your
  `package.json`).
- A `.prettierrc.mjs`, or `prettier.config.mjs` file that exports an object
  using export default.
- A `.prettierrc.cjs`, or `prettier.config.cjs` file that exports an object
  using module.exports.
- A `.prettierrc.toml` file.

The configuration file will be resolved starting from the location of the file
being formatted, and searching up the file tree until a config file is (or
isn’t) found.

You can run `npx-prettier-init`, which will ask you questions about preference
and generate an initial config file for you.

## My Config

### JSON Format

```json
{
  "overrides": [
    {
      "files": "*.md",
      "options": {
        "max_line_length": 80,
        "proseWrap": "always"
      }
    },
    {
      "files": ["*.json", "*.html", "*.js", "*.css", "*.mjs", "*.ts", "*.tsx"],
      "options": {
        "tabWidth": 2
      }
    }
  ]
}
```

### YAML Format

```yaml
overrides:
  - files: "*.md"
    options:
      max_line_length: 80
      proseWrap: always
  - files: ["*.json", "*.html", "*.js", "*.css", "*.mjs", "*.ts", "*.tsx"]
    options:
      tabWidth: 2
# vim: ft=yaml
```

## Config Options

For the list of config options, and guidelines for configuration: see
[prettier config](https://prettier.io/docs/en/options.html).

## Ignore

- Use `.prettierignore` like `.gitignore`
- Use `// prettier-ignore` to ignore the next block of code
- Use `/* prettier-ignore-start */` and `/* prettier-ignore-end */` to ignore
  everything in between, but this only works on top level code blocks. It is for
  ignoring auto-generated code.

## 🧭 Navigation

- [📑 Notes Index](../../index.md)
