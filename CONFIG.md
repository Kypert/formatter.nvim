# Sample configuration

Below is a non-extensive list of tools you can use with Formatter.nvim. Since the plugin does not define formatters to run, you must specify them in your neovim config.

## Prettier

Prettier can be configured for multiple filetypes, but the below config will use JavaScript as the example.

```lua
require('formatter').setup({
  filetype = {
    javascript = {
      -- prettier
      function()
        return {
          exe = "prettier",
          args = {"--stdin-filepath", vim.fn.fnameescape(vim.api.nvim_buf_get_name(0)), '--single-quote'},
          stdin = true
        }
      end
    },
  }
})
```

## Rustfmt

```lua
require('formatter').setup({
  filetype = {
    rust = {
      -- Rustfmt
      function()
        return {
          exe = "rustfmt",
          args = {"--emit=stdout"},
          stdin = true
        }
      end
    },
  }
})
```

## shfmt

```lua
require('formatter').setup({
  filetype = {
    sh = {
        -- Shell Script Formatter
       function()
         return {
           exe = "shfmt",
           args = { "-i", 2 },
           stdin = true,
         }
       end,
   }
  }
})
```

## luafmt

```lua
require('formatter').setup({
  filetype = {
    lua = {
        -- luafmt
        function()
          return {
            exe = "luafmt",
            args = {"--indent-count", 2, "--stdin"},
            stdin = true
          }
        end
    },
  }
})
```

## clang-format

Run clang-format with inline (`-i`) and hint about the lines to operate on by setting `$start_line` and `$end_line`.
This makes range formatting work, in the sense that the correct indentation will be kept.

```lua
function formatter_clang_format()
  return {
    exe = clang_format_exe,
    args = {"--assume-filename", vim.api.nvim_buf_get_name(0), "--lines", "$start_line:$end_line", "-i"},
    stdin = false,
    cwd = vim.fn.getcwd(),
    tempfile_inline = true,
    range_lines_one_based = true,
  }
end

require('formatter').setup({
  filetype = {
    c = { formatter_clang_format },
    cpp = { formatter_clang_format },
  }
})
```
## rubocop

```lua
require('formatter').setup({
  filetype = {
    ruby = {
       -- rubocop
       function()
         return {
           exe = "rubocop", -- might prepend `bundle exec `
           args = { '--auto-correct', '--stdin', '%:p', '2>/dev/null', '|', "awk 'f; /^====================$/{f=1}'"},
           stdin = true,
         }
       end
     }
  }
})
```
