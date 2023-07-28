---
title: NeoVim
---

# Notes to Self

- I use [lazy-vim-starter](https://github.com/frans-johansson/lazy-nvim-starter)
  script to initialize the NVim configuration structure:
  - Three main components:
    - `core`: Most basic editor set-up, e.g. NVim options, keymaps, and
      bootstrapping for lazy.nvim.
    - `helpers`: Helper functions `require()`-ed elsewhere in the config.
    - `plugins`: Each `.lua` files in this directory should serve as a lazy.nvim
      plugin spec.
  - Mappings specific for each plugin is located in its own `.lua` spec file.

# Lua in NeoVim

## Reference and Help

Use `:help lua-concepts`.

`:help lua-guide` for API.

## Lua API in Vim

### Overview: 3 layers

1.  The **Vim API** inherited from Vim:

    - Ex-commands: via `vim.cmd()`
    - Vim builtin functions, and user functions: via `vim.fn()`

    See `:help lua-guide-vimscript`.

2.  The **NVim API** written in C for use in remote plugins and GUIs. Accessed
    through `vim.api`. See `:help api`.

3.  The **Lua API** written in and specifically for Lua. Accessible by any other
    `vim.*` functions not mentioned above. See `:help lua-stdlib`.

This distinction is important because different layers work in different source
with different syntax. E.g. starting index (0 or 1) and argument omission.

### Using Vim commands

```lua
vim.cmd("colorscheme gruvbox")
vim.cmd("%s/\\Vfoo/bar/g")
vim.cmd([[
    hightlight Error guibg=red
    highlight link WarningError
]])
```

Alternative method:

```lua
vim.cmd.colorscheme("habamax")
vim.cmd.highlight({ "Error", "guibg=red" })
vim.cmd.highlight({ "link", "Warning", "Error" })
```

### Sourcing Vim scripts

`vim.cmd.source("path/to/vim/script.vim")`

### Calling Vim functions

Use `vim.fn`, with the two different notations for Lua table:

1.  `vim.fn.abs(-1)`
2.  `vim.fn['abs'](-1)`

To call an autoload function, since `#` is not a valid character for Lua
identifier, so you have to use the 2nd notation.

### Vim variables

- `vim.g`: global variables (`g:`)
- `vim.b`: variables for the current buffer (`b:`)
- `vim.w`: variables for the current window (`w:`)
- `vim.t`: variables for the current tab page (`t:`)
- `vim.v`: predefined Vim variables (`v:`)
- `vim.env`: environment variables defined in the editor session. Key doesn't
  include the `$` sign.

Data type are converted automatically.

### Vim options

#### Setting options

- `vim.opt` behaves like `:set`
- `vim.opt_global` behaves like `:setglobal`
- `vim.opt_local` behaves like `:setlocal`

For boolean options (e.g. `expandtab`), they take true or false:

- `vim.opt.expandtab = false` is equivalent to `:set noexpandtab`.

For _list-like_, _map-like_ and _set-like_ options:

```vim
set wildignore=*.o,*.a,__pycache__
set listchars=space:_,tab:>~
set formatoptions=njt
```

is equivalent to

```lua
vim.opt.wildignore = { '*.o', '*.a', '__pycache__' }
vim.opt.listchars = { space = '_', tab = '>~' }
vim.opt.formatoptions = { n = true, j = true, t = true }
```

#### Append, prepend and remove

For `:set^=`, `:set+=` and `:set-=`:

```lua
vim.opt.shortmess:append({ I = true })
vim.opt.wildignore:prepend('*.o')
vim.opt.whichwrap:remove({ 'b', 's' })
```

#### Query option value

But getting the value of an options is indirect, and need to use `:get()`:

```lua
print(vim.opt.smarttab)
--output a reference to a table
print(vim.opt.smarttab:get())
--output false
```

#### Vim options shortcuts

- `vim.o`: behaves like `:set`
- `vim.go`: behaves like `:setglobal`
- `vim.bo`: for buffer-scoped options
- `vim.wo`: for window-scoped options

Note that these shortcuts behaves like Vim's `:let &listchars='space:_,tab:>~'`.
So the syntax of the expression on the RHS is different from using the above
`vim.opt` and `vim.opt_global`:

```lua
vim.o.listchars = 'space:_,tab:>~'
```

You can specify window-id or buffer-id:

```lua
vim.bo[4].expandtab = true
vim.wo[2].number = true
```

I.e. `&expandtab` is equivalent to `vim.bo.expandtab`. And a vim excmd

```vim
set expandtab?
```

is equivalent to a Lua code

```lua
print(vim.bo.expandtab)
```

### Vim autocmds

Use `vim.api.nvim_create_autocmd({event}, {*opts})`

- `events` can be a single string for a single event, or can be a table list
  containing multiple events.
- `{*opts}` is a table defining specs for the handler:

  - `pattern` is a table defining the filename pattern.

    E.g. `pattern = {"*.c", "*.h"}`

  - `callback` is a callback function

    E.g. `callback = function(ev) print('ev') end`

  - `command` is a string that is a Vim command.

    E.g. `command = "echo 'Entering a C or C++ file'"`

Example:

```lua
vim.api.nvim_create_autocmd({"BufEnter", "BufWinEnter"}, {
    pattern = {"*.c", "*.h"},
    callback = function(ev)
        print(string.format('event fired: s', vim.inspect(ev)))
    end
})
```

### Vim user commands

Use `vim.api.nvim_create_autocmd({name}, {command}, {opts})`:

One opt is `desc`.

## Using Lua files on startup

From the root config dir of NVim (default: `$XDG_CONFIG_HOME/nvim/`), there can
be either `init.vim` or `init.lua` but not both. Then NVim will source all vim
scripts and lua files in `plugin/` in `'runetimepath'`.

## Vim patterns

In NeoVim, you can use `vim.regex()` to use Vim regex.

## Import Lua modules in Vim

Place the Lua modules in a `lua/` directory in `'runtimepath'`. Then use
`require()` in a Lua script to load it:

```lua
require('other_modules/anothermodule')
-- or
require('other_modules.anothermodule')
```

Submodules is just subdirectories. This is like Vim's autoload mechanism.

What NVim does when using `require()`:

1.  Modules are searched in a `lua/` directory in `'runtimepath'`, in the order
    they appear.
2.  Any `.` in the module name is treated as a directory separator.
3.  So for a module `foo.bar`, the first directory in `'runtimepath'` is search
    for `lua/foo/bar.lua` then for `lua/foo/bar/init.lua`, and then repeat in
    the next directory.
4.  If still fails, then other search mechanics are used. See NeoVim lua.txt
5.  The first lua script found is run, and `require()` returns the value
    **returned by the script** if any, else true.
6.  This return value is **cached** after the first call to `require()` for each
    module. I.e. subsequent calls don't search or execute lua script.

### Catching errors

`pcall()` can be used to catch errors.

```lua
local ok, mymod = pcall(require, 'module_with_error')
if not ok then
  print("Module had an error")
else
  mymod.function()
end
```

### require() vs :source

`require()` caches the module on the module's first use. To clear the cache:

```lua
package.loaded['myluamodule'] = nil
```

## Executing Lua code in Vim

### Overview

`:lua` executes a Lua chunk. `:luado` execute Lua chunk within in a given range
in a Lua file. `:luafile` execute a Lua file as a chunk.

Each chunk has its **own scope**, so only global variables are shared between
command calls.

### :lua

`:lua {chunk}` executes a chunk. If chunk starts with `=`, the rest of the chunk
is evaluated as an expression and printed:

- `:lua =expr` == `:=expr` == `:lua print(vim.inspect(expr))`

### :lua with endmarker

```
:lua << [endmarker]
{script}
{endmarker}
```

Note that the actual endmarker must not preceded by whitespace. An example:

```vim
function! CurrentLineInfo()
lua << EOF
local linenr = vim.api.nvim_win_get_cursor(0)[1]
local curline = vim.api.nvim_buf_get_lines(0, linenr - 1, linenr, false)[1]
print(string.format("Current line [%d] has %d bytes", linenr, #curline))
EOF
endfunction
```

### :luado

`:[range]luado {body}` executes Lua chunk "function(line, linenr) {body} end"
for each buffer line in [range], whereas line is current line text and linenr is
current line number.

### :luafile

`:luafile {file}` executes Lua script in a file. The whole argument is used as
filename (like `:edit`), so don't need to escape whitespace.

You can also `:source` Lua files.

## Print

- To print object in human readable format, use `print(vim.inspect(myTable))`.
- To display a notification, use `vim.notify()`

# Mappings

## Insert mode

- `<C-f>` reindents current line. (Set by 'indk' option by default)

## Lists of useful mappings

- `<leader>sb`: telescope fuzzy search line in current buffer
- `[p`, `]p`, `gp`: paste with current indent; paste and jump cursor to after

## View port

- `z<cr>`: Redraw, cursor line at the top of screen.
- `z.`: Redraw, cursor line at the middle of screen.

## From lazy-nvim-starter

### Source

[GitHub site](https://github.com/frans-johansson/lazy-nvim-starter)

### lazy-nvim-starter mappings

- `<leader>l`: Show Lazy

# Plugins

## Lazy.nvim

See [# Using the Lazy.nvim](#using-the-lazy.vim)

## nvim-cmp

Key bindings:

- `<C-f>` and `<C-b>`: Scroll doc
- `<C-Space>`: Mapping completes.
- `<C-e>`: Abort complete and close menu.
- `<CR>` and `<C-y>`: Confirm selection

## Telescope

### Mappings

| Key       | Effect                       |
| --------- | ---------------------------- |
| `<M-q>`   | Send selected to quickfix    |
| `<Tab>`   | Toogle selection + move up   |
| `<S-Tab>` | Toggle selection + move down |
| `<C-v>`   | Vertical split               |

## nvim-html-css

- I added a bash alias `vh` to set an environment variable before opening nvim,
  to enable this plugin.

## vim-visual-multi

### General rule

- There are two modes, change between each other with `<Tab>`:
  1.  Multi-cursor mode
  2.  Multi-visual mode
- `<Leader>` here is the specific leader for _vim-visual-multi_ only.
- Many keymaps can be repeated by prepending a number, or repeated later with
  `.`.
- Undo/redo records and registers are unique in multi-mode (i.e separate from
  outside Vim's)
  - Seems like only text deleted in multi-select mode (but not in multi-cursor
    mode) are recorded in the unique register.

### Vim normal mode

- `<C-Arrow>` enters multi-cursor mode at given direction.
- `<S-Arrow>` enters multi-select mode at given direction
- `<C-n>` selects the current word and enters multi-select mode.

### Vim visual mode

- `<Leader>c` enters multi-cursor mode
  - If in row visual mode, enters at the column of the visual start
  - If in column visual mode, enters at the column of the right-most column

### Multi-cursor mode

- Most Vim normal mappings are functional
- `s<Selector>` enters multi-visual mode by selecting elements with Vim selector
  mode (like `i'` for inside `'`).
- `<Leader><Lt>` searches for a character to the right, and aligns lines at that
  character
- `X` deletes previous character, same as `<BS>`
- `.` repeats previous operation (entering insertion mode and insert some text
  is one action)
- `<Leader>N` adds a number list followed with a given string.

### Multi-select mode

- Most Vim visual mapping are functional
- `n` and `N` jumps and selects the next matching word/patterns
- `q` and `Q` deselects current match and jumps to next or previous match.
- `<Leader>w` toggle between matching word/pattern
- `<Leader>c` cycles among different case matching setting
- `R` does a `:substitute` command in each selection region

### Multi-insert mode

- `<C-v>` pastes content in visual-multi's register.

## flash

Mappings:

- `r`: Only at the beginning of motion operator pending mode (e.g. after `y`)
  1.  Type pattern
  2.  Select label
  3.  Perform motion / start flash Treesitter with `S`
  4.  (Yank will be performed on the new selection)
  5.  (Cursor back to original window/position)
- `R`: Like above but use flash Treesitter directly

Telescope mapping

- `<C-s>`: Flash to a line?

## Orgmode

## Insert mode (custom)

Leading key is `<A-;>`, then followed by:

- `h` toggles heading

## Mkdnflow

- [Mkdnflow](mkdnflow.md)

## LuaSnip

Guides:

- [Finding the comment-string](https://github.com/L3MON4D3/LuaSnip/wiki/Cool-Snippets#finding-the-comment-string)
- [Writing a comment box snippet](https://github.com/L3MON4D3/LuaSnip/issues/151)

# Plugin Wish List

- [nabla.nvim](https://github.com/jbyuki/nabla.nvim): Render LaTeX equation in
  vim
- [venn.nvim](https://github.com/jbyuki/venn.nvim): Draw diagram in vim
- [lsp_lines.nvim](https://git.sr.ht/~whynothugo/lsp_lines.nvim): Renders
  diagnostics using virtual lines pointing at the location

# Using IDE Tools

## Prettier

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
      "files": ["*.json", "*.html", "*.js", "*.css"],
      "options": {
        "tabWidth": 2
      }
    }
  ]
}
```

For the list of config options, and guidelines for configuration: see
[prettier config](https://prettier.io/docs/en/options.html).

# Using the Lazy.nvim

If use use both `opts` and `config`. Remember to past `opts` to the function set
in the `config`:

```lua
{
    config = function(_, opts)
        require("plugin").setup(opts)
    end,
    opts = {
        ...
    }
}
```

# Treesitter

A capture group is a syntax element (or node) in the tree that is given a
**name**. A capture group's name starts with `@`. You can then change the
highlighting of a capture group with `vim.api.nvim_set_hl()`

To inspect the capture group of the node at the cursor, use

```
lua print(vim.inspect(vim.treesitter.get_captures_at_cursor(0)))
```

# 🧭 Navigation

- [🔼 Back to top](#)
- [📑 Index](../../index.md)
