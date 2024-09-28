---
date: 2024-05-08 (Wed)
---

# markdown

## Nvim Render Markdown Extra Syntax

### Code Blocks with filename

```python {filename="demo.py"}
def main() -> None:
  sum = 0
  for i in range(10):
    sum += i
  print(sum)

if __name__ == "__main__":
  main()
```

### Quote blocks with header

You can add a header to a quote block using the syntax. They are case
insensitive.

- `[!NOTE]`
- `[!TIP]`
- `[!IMPORTANT]`
- `[!WARNING]`
- `[!CAUTION]`
- `[!DANGER]`
- `[!INFO]`
- `[!WARNING] Custom Title in Warning Style`

> [!NOTE]
>
> A regular note
>
> With a second paragraph

> [!TIP]
>
> ```lua
> print('Standard tip')
> ```

> [!WARNING]
>
> Warning

> [!INFO] Trivia
>
> Some trivia facts

### TODO List

- [ ] Unchecked
- [x] Checked
- [-] Todo
- Regular list item

## Tricks

- To type a backtick within inline code, use double backtick and spaces `` ` ``.

## 🧭 Navigation

- [🔼 Back to top](#markdown)
- [◀️ Back](../../index.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
