# Neovim LazyVim Keymap Reference

> Comprehensive keymap documentation for LazyVim configuration with custom plugins
>
> **Leader Key:** `<Space>` (Space bar)
> **Local Leader Key:** `\`

---

## Table of Contents

1. [Core Navigation & Movement](#core-navigation--movement)
2. [Window Management](#window-management)
3. [Buffer Management](#buffer-management)
4. [File Operations](#file-operations)
5. [Search & Navigation](#search--navigation)
6. [Code Editing](#code-editing)
7. [LSP (Language Server Protocol)](#lsp-language-server-protocol)
8. [Diagnostics](#diagnostics)
9. [Git Operations](#git-operations)
10. [Formatting & Toggles](#formatting--toggles)
11. [Terminal](#terminal)
12. [Telescope](#telescope)
13. [Fuzzy Finding (FZF)](#fuzzy-finding-fzf)
14. [Claude Code (AI Assistant)](#claude-code-ai-assistant)
15. [Trouble (Diagnostics)](#trouble-diagnostics)
16. [TODO Comments](#todo-comments)
17. [Neo-tree (File Explorer)](#neo-tree-file-explorer)
18. [DAP (Debugging)](#dap-debugging)
19. [Neotest (Testing)](#neotest-testing)
20. [Mini Plugins](#mini-plugins)
21. [Mason (LSP Installer)](#mason-lsp-installer)
22. [Tabs](#tabs)
23. [Quickfix & Location List](#quickfix--location-list)
24. [LazyVim UI](#lazyvim-ui)

---

## Core Navigation & Movement

### Basic Movement
| Key | Mode | Description |
|-----|------|-------------|
| `j` | n, x | Down (better down with `gj` when no count) |
| `k` | n, x | Up (better up with `gk` when no count) |
| `<Down>` | n, x | Down (same as `j`) |
| `<Up>` | n, x | Up (same as `k`) |
| `<C-h>` | n | Move to left window |
| `<C-j>` | n | Move to lower window |
| `<C-k>` | n | Move to upper window |
| `<C-l>` | n | Move to right window |

### Line Movement
| Key | Mode | Description |
|-----|------|-------------|
| `<A-j>` | n, i, v | Move current line down |
| `<A-k>` | n, i, v | Move current line up |

### Visual Selection Movement (Mini.move)
| Key | Mode | Description |
|-----|------|-------------|
| `<A-h>` | v | Move selection left |
| `<A-j>` | v | Move selection down |
| `<A-k>` | v | Move selection up |
| `<A-l>` | v | Move selection right |

### Flash Navigation
| Key | Mode | Description |
|-----|------|-------------|
| `s` | n, x, o | Flash jump |
| `S` | n, x, o | Flash Treesitter |
| `r` | o | Remote Flash |
| `R` | o, x | Treesitter Search |
| `<C-s>` | c | Toggle Flash Search (in command mode) |

---

## Window Management

### Window Navigation
| Key | Mode | Description |
|-----|------|-------------|
| `<C-h>` | n | Move to left window |
| `<C-j>` | n | Move to lower window |
| `<C-k>` | n | Move to upper window |
| `<C-l>` | n | Move to right window |

### Window Resizing
| Key | Mode | Description |
|-----|------|-------------|
| `<C-Up>` | n | Increase window height by 2 |
| `<C-Down>` | n | Decrease window height by 2 |
| `<C-Left>` | n | Decrease window width by 2 |
| `<C-Right>` | n | Increase window width by 2 |

### Window Operations
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>-` | n | Split window horizontally |
| `<leader>\|` | n | Split window vertically |
| `<leader>wd` | n | Delete window |
| `<leader>wm` | n | Toggle window zoom |
| `<leader>uZ` | n | Toggle window zoom (alternate) |

---

## Buffer Management

| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<S-h>` | n | Previous buffer | `:bprevious` |
| `<S-l>` | n | Next buffer | `:bnext` |
| `[b` | n | Previous buffer (alternate) | `:bprevious` |
| `]b` | n | Next buffer (alternate) | `:bnext` |
| `<leader>bb` | n | Switch to other buffer | `:e #` |
| `<leader>\`` | n | Switch to other buffer (alternate) | `:e #` |
| `<leader>bd` | n | Delete buffer | `:lua Snacks.bufdelete()` |
| `<leader>bo` | n | Delete other buffers | `:lua Snacks.bufdelete.other()` |
| `<leader>bD` | n | Delete buffer and window | `:bd` |
| `<leader>tb` | n | Show buffers (Telescope) | `:Telescope buffers` |

---

## File Operations

| Key | Mode | Description |
|-----|------|-------------|
| `<C-s>` | n, i, x, s | Save file |
| `<leader>fn` | n | New file |

---

## Search & Navigation

| Key | Mode | Description |
|-----|------|-------------|
| `<Esc>` | i, n, s | Clear search and stop snippet |
| `<leader>ur` | n | Redraw, clear search, diff update |
| `n` | n, x, o | Next search result (centered) |
| `N` | n, x, o | Previous search result (centered) |
| `<leader>sr` | n, x | Search and Replace |
| `<leader>st` | n | Search TODOs |

---

## Code Editing

### Basic Editing
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>K` | n | Keywordprg lookup |
| `<` | x | Decrease indent (maintains selection) |
| `>` | x | Increase indent (maintains selection) |
| `gco` | n | Add comment below line |
| `gcO` | n | Add comment above line |

### Undo Break-points
| Key | Mode | Description |
|-----|------|-------------|
| `,` | i | Add undo break-point |
| `.` | i | Add undo break-point |
| `;` | i | Add undo break-point |

### Surround Operations (Mini.surround)
| Key | Mode | Description |
|-----|------|-------------|
| `gsa` | n, x | Add surrounding |
| `gsd` | n | Delete surrounding |
| `gsf` | n | Find surrounding (to the right) |
| `gsF` | n | Find surrounding (to the left) |
| `gsh` | n | Highlight surrounding |
| `gsr` | n | Replace surrounding |
| `gsn` | n | Update n_lines configuration |

---

## LSP (Language Server Protocol)

### Navigation
| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `gd` | n | Goto Definition | `:lua vim.lsp.buf.definition()` |
| `gD` | n | Goto Declaration | `:lua vim.lsp.buf.declaration()` |
| `gr` | n | References | `:lua vim.lsp.buf.references()` |
| `gi` | n | Goto Implementation | `:lua vim.lsp.buf.implementation()` |
| `gt` | n | Goto Type Definition (custom override) | `:lua vim.lsp.buf.type_definition()` |
| `gy` | n | Goto Type Definition (LazyVim default) | `:lua vim.lsp.buf.type_definition()` |
| `]]` | n | Next Reference | - |
| `[[` | n | Prev Reference | - |
| `<A-n>` | n | Next Reference (alternate) | - |
| `<A-p>` | n | Prev Reference (alternate) | - |

### Documentation & Help
| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `K` | n | Hover documentation | `:lua vim.lsp.buf.hover()` |
| `gK` | n | Signature Help | `:lua vim.lsp.buf.signature_help()` |
| `<C-k>` | i | Signature Help (insert mode) | `:lua vim.lsp.buf.signature_help()` |

### Code Actions
| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>ca` | n, x | Code Action (LazyVim default) | `:lua vim.lsp.buf.code_action()` |
| `<leader>Ca` | n, v | Code Action (custom LSP binding) | `:lua vim.lsp.buf.code_action()` |
| `<D-.>` | n, v | Code Action (macOS - Cmd+.) | `:lua vim.lsp.buf.code_action()` |
| `<leader>cA` | n | Source Action | - |
| `<leader>cr` | n | Rename symbol (LazyVim default) | `:lua vim.lsp.buf.rename()` |
| `<space>rn` | n | Rename symbol (custom) | `:lua vim.lsp.buf.rename()` |
| `<leader>cR` | n | Rename File | - |
| `<leader>cf` | n, x | Format code (LazyVim default) | `:lua vim.lsp.buf.format()` |
| `<space>f` | n | Format code (custom) | `:lua vim.lsp.buf.format()` |

### Code Lens
| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>cc` | n, x | Run Codelens | `:lua vim.lsp.codelens.run()` |
| `<leader>cC` | n | Refresh & Display Codelens | `:lua vim.lsp.codelens.refresh()` |

### LSP Management
| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>cl` | n | LSP Info | `:LspInfo` |
| `<leader>cm` | n | Mason (LSP installer) | `:Mason` |
| `<leader>cs` | n | Symbols | - |
| `<space>wa` | n | Add workspace folder | `:lua vim.lsp.buf.add_workspace_folder()` |
| `<space>wr` | n | Remove workspace folder | `:lua vim.lsp.buf.remove_workspace_folder()` |
| `<space>wl` | n | List workspace folders | `:lua print(vim.inspect(vim.lsp.buf.list_workspace_folders()))` |

---

## Diagnostics

| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>cd` | n | Show line diagnostics | `:lua vim.diagnostic.open_float()` |
| `<leader>ud` | n | Toggle diagnostics | - |
| `]d` | n | Next diagnostic | `:lua vim.diagnostic.goto_next()` |
| `[d` | n | Previous diagnostic | `:lua vim.diagnostic.goto_prev()` |
| `]e` | n | Next error | `:lua vim.diagnostic.goto_next({severity=vim.diagnostic.severity.ERROR})` |
| `[e` | n | Previous error | `:lua vim.diagnostic.goto_prev({severity=vim.diagnostic.severity.ERROR})` |
| `]w` | n | Next warning | `:lua vim.diagnostic.goto_next({severity=vim.diagnostic.severity.WARN})` |
| `[w` | n | Previous warning | `:lua vim.diagnostic.goto_prev({severity=vim.diagnostic.severity.WARN})` |

---

## Git Operations

### Git Commands
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>gg` | n | Lazygit (root directory) |
| `<leader>gG` | n | Lazygit (current working directory) |
| `<leader>gL` | n | Git log |
| `<leader>gl` | n | Git log (root) |
| `<leader>gf` | n | Git history for current file |
| `<leader>gb` | n | Git blame for current line |
| `<leader>gB` | n, x | Open in git browser |
| `<leader>gY` | n, x | Copy git browser URL |

### Git Hunks (Gitsigns)
| Key | Mode | Description |
|-----|------|-------------|
| `]h` | n | Next hunk |
| `[h` | n | Previous hunk |
| `]H` | n | Last hunk |
| `[H` | n | First hunk |
| `<leader>ghs` | n, x | Stage hunk |
| `<leader>ghr` | n, x | Reset hunk |
| `<leader>ghS` | n | Stage buffer |
| `<leader>ghu` | n | Undo stage hunk |
| `<leader>ghp` | n | Preview hunk inline |
| `<leader>ghb` | n | Blame line |
| `<leader>ghd` | n | Diff this |

---

## Formatting & Toggles

### Formatting
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>cf` | n, x | Format code |
| `<leader>uf` | n | Toggle auto-formatting |
| `<leader>uF` | n | Force auto-formatting |

### UI Toggles
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>us` | n | Toggle spelling |
| `<leader>uw` | n | Toggle line wrap |
| `<leader>uL` | n | Toggle relative line numbers |
| `<leader>ul` | n | Toggle line numbers |
| `<leader>ud` | n | Toggle diagnostics |
| `<leader>uc` | n | Toggle conceal level |
| `<leader>uA` | n | Toggle tabline |
| `<leader>uT` | n | Toggle treesitter |
| `<leader>ub` | n | Toggle dark background |
| `<leader>uD` | n | Toggle dim |
| `<leader>ua` | n | Toggle animations |
| `<leader>ug` | n | Toggle indent guides |
| `<leader>uS` | n | Toggle smooth scrolling |
| `<leader>uh` | n | Toggle inlay hints |
| `<leader>uz` | n | Toggle zen mode |

---

## Terminal

| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<C-t>` | n, t | Toggle terminal (ToggleTerm) | `:ToggleTerm` |
| `<C-\`>` | n, t | Toggle terminal (ToggleTerm alternate) | `:ToggleTerm` |
| `<C-/>` | n, t | Toggle terminal (root directory) | `:lua Snacks.terminal()` |
| `<C-_>` | n, t | Toggle terminal (alternate binding) | `:lua Snacks.terminal()` |
| `<leader>fT` | n | Terminal at current working directory | `:lua Snacks.terminal(nil, {cwd=vim.fn.getcwd()})` |
| `<leader>ft` | n | Terminal at root | `:lua Snacks.terminal()` |

---

## Telescope

### Custom Keymaps
| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>tf` | n | Find Files (Telescope) | `:Telescope find_files` |
| `<leader>tg` | n | Live Grep (Telescope) | `:Telescope live_grep` |
| `<leader>tb` | n | Show Buffers (Telescope) | `:Telescope buffers` |
| `<leader>th` | n | Show Help Tags (Telescope) | `:Telescope help_tags` |
| `<leader>td` | n | TODO List (TodoTelescope) | `:TodoTelescope` |

---

## Fuzzy Finding (FZF)

FZF-lua is configured with `fd` to respect gitignore and exclude hidden files by default.

---

## Claude Code (AI Assistant)

### Main Commands
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>cc` | n, v | Toggle Claude |
| `<leader>cf` | n, v | Focus Claude |
| `<leader>cr` | n, v | Resume Claude |
| `<leader>cC` | n, v | Continue Claude |

### File Management
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>cb` | n, v | Add current buffer |
| `<leader>cs` | v | Send selection to Claude |
| `<leader>cs` | neo-tree | Add file from tree |

### Diff Management
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>ca` | n, v | Accept diff |
| `<leader>cd` | n, v | Deny diff |

---

## Trouble (Diagnostics)

| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>tt` | n | Toggle Diagnostics (Trouble) | `:Trouble diagnostics toggle` |
| `<leader>xx` | n | Toggle Diagnostics (alternate) | `:Trouble diagnostics toggle` |
| `<leader>xX` | n | Buffer Diagnostics | `:Trouble diagnostics toggle filter.buf=0` |
| `<leader>cs` | n | Symbols | `:Trouble symbols toggle` |
| `<leader>xL` | n | Location List | `:Trouble loclist toggle` |
| `[q` | n | Previous item | - |
| `]q` | n | Next item | - |

---

## TODO Comments

| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>td` | n | Toggle TODO list | `:TodoTelescope` |
| `<leader>xt` | n | TODO List (alternate) | `:Trouble todo toggle` |
| `<leader>st` | n | Search TODOs | `:TodoTelescope` |
| `]t` | n | Next TODO | - |
| `[t` | n | Previous TODO | - |

---

## Neo-tree (File Explorer)

### Main Commands
| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>fe` | n | Explorer NeoTree (Root Dir) | `:Neotree` |
| `<leader>fE` | n | Explorer NeoTree (cwd) | `:Neotree dir=%:p:h` |
| `<leader>e` | n | Explorer NeoTree (Root Dir) | `:Neotree` |
| `<leader>E` | n | Explorer NeoTree (cwd) | `:Neotree dir=%:p:h` |
| `<leader>ge` | n | Git Explorer | `:Neotree git_status` |
| `<leader>be` | n | Buffer Explorer | `:Neotree buffers` |
| `:E` | command | Open Neotree (NetRW replacement) | `:Neotree` |

### Window Mappings (when Neo-tree is open)
| Key | Description |
|-----|-------------|
| `%` | Add new file |
| `d` | Add directory |
| `D` | Delete file/directory |
| `R` | Rename file/directory |
| `r` | Refresh |

**Note:** Neo-tree follows the current file and shows filtered items (including hidden files).

---

## DAP (Debugging)

### Main Debugging Controls
| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<S-CR>` | n | Continue/Start debugging (custom - Shift+Enter) | `:lua require('dap').continue()` |
| `<leader>db` | n | Toggle Breakpoint | `:DapToggleBreakpoint` |
| `<leader>dB` | n | Breakpoint Condition | `:lua require('dap').set_breakpoint(vim.fn.input('Condition: '))` |
| `<leader>dc` | n | Run/Continue (disabled - use `<S-CR>`) | `:DapContinue` |
| `<leader>dC` | n | Run to Cursor | `:lua require('dap').run_to_cursor()` |
| `<leader>da` | n | Run with Args | - |

### Stepping
| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>di` | n | Step Into | `:DapStepInto` |
| `<leader>do` | n | Step Out | `:DapStepOut` |
| `<leader>dO` | n | Step Over | `:DapStepOver` |
| `<leader>dg` | n | Go to Line (No Execute) | `:lua require('dap').goto_()` |
| `<leader>dj` | n | Down | `:lua require('dap').down()` |
| `<leader>dk` | n | Up | `:lua require('dap').up()` |

### Session Management
| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>dr` | n | Toggle REPL | `:lua require('dap').repl.toggle()` |
| `<leader>ds` | n | Session | `:lua require('dap').session()` |
| `<leader>dt` | n | Terminate | `:DapTerminate` |
| `<leader>dP` | n | Pause | `:lua require('dap').pause()` |
| `<leader>dl` | n | Run Last | `:lua require('dap').run_last()` |

### UI & Information
| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>du` | n | Toggle DAP UI | `:lua require('dapui').toggle()` |
| `<leader>de` | n, v | Eval | `:lua require('dapui').eval()` |
| `<leader>dw` | n | Widgets | `:lua require('dap.ui.widgets').hover()` |

---

## Neotest (Testing)

**Note:** All test keybindings use **capital T** (`<leader>T*`) instead of lowercase.

| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>TT` | n | Run File | `:lua require('neotest').run.run(vim.fn.expand('%'))` |
| `<leader>Ta` | n | Attach to Test | `:lua require('neotest').run.attach()` |
| `<leader>Tr` | n | Run Nearest | `:lua require('neotest').run.run()` |
| `<leader>Tl` | n | Run Last | `:lua require('neotest').run.run_last()` |
| `<leader>Ts` | n | Toggle Summary | `:lua require('neotest').summary.toggle()` |
| `<leader>To` | n | Show Output | `:lua require('neotest').output.open({enter=true, auto_close=true})` |
| `<leader>TO` | n | Toggle Output Panel | `:lua require('neotest').output_panel.toggle()` |
| `<leader>TS` | n | Stop | `:lua require('neotest').run.stop()` |
| `<leader>Tw` | n | Toggle Watch | `:lua require('neotest').watch.toggle(vim.fn.expand('%'))` |
| `<leader>Td` | n | Debug Nearest | `:lua require('neotest').run.run({strategy='dap', suite=false})` |

**Disabled:** All lowercase `<leader>t*` test bindings are disabled to avoid conflicts with Telescope/TODO/Trouble.

---

## Mini Plugins

### Mini.surround
See [Surround Operations](#surround-operations-minisurround) section above.

### Mini.move
See [Visual Selection Movement](#visual-selection-movement-minimove) section above.

---

## Mason (LSP Installer)

| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>ma` | n | Toggle Mason menu | `:Mason` |
| `<leader>cm` | n | Mason (alternate) | `:Mason` |

---

## Tabs

| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader><tab><tab>` | n | New tab | `:tabnew` |
| `<leader><tab>l` | n | Go to last tab | `:tablast` |
| `<leader><tab>f` | n | Go to first tab | `:tabfirst` |
| `<leader><tab>]` | n | Next tab | `:tabnext` |
| `<leader><tab>[` | n | Previous tab | `:tabprevious` |
| `<leader><tab>d` | n | Close tab | `:tabclose` |
| `<leader><tab>o` | n | Close other tabs | `:tabonly` |

---

## Quickfix & Location List

| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>xq` | n | Toggle quickfix list | `:copen` / `:cclose` |
| `<leader>xl` | n | Toggle location list | `:lopen` / `:lclose` |
| `<leader>xL` | n | Location List (Trouble) | `:Trouble loclist toggle` |
| `[q` | n | Previous quickfix item | `:cprevious` |
| `]q` | n | Next quickfix item | `:cnext` |

---

## LazyVim UI

| Key | Mode | Description | Command |
|-----|------|-------------|---------|
| `<leader>l` | n | Open Lazy plugin manager | `:Lazy` |
| `<leader>L` | n | Show LazyVim changelog | `:LazyExtras` |
| `<leader>qq` | n | Quit all | `:qa` |
| `<leader>ui` | n | Inspect highlight at cursor | `:Inspect` |
| `<leader>uI` | n | Inspect treesitter tree | `:InspectTree` |
| `<leader>?` | n | Buffer keymaps help | - |

---

## Which-Key Groups

These are prefix groups for organizing keymaps:

| Prefix | Group Name |
|--------|-----------|
| `<leader><tab>` | Tabs |
| `<leader>b` | Buffer |
| `<leader>c` | Code / Claude (lowercase) |
| `<leader>C` | Code Actions (capital C - custom LSP) |
| `<leader>d` | Debug (DAP) |
| `<leader>dp` | Profiler |
| `<leader>f` | File/Find |
| `<leader>g` | Git |
| `<leader>gh` | Git Hunks |
| `<leader>q` | Quit/Session |
| `<leader>s` | Search |
| `<leader>t` | Telescope/Todo/Trouble |
| `<leader>T` | Neotest (Testing) - uses CAPITAL T |
| `<leader>u` | UI Toggles |
| `<leader>w` | Windows |
| `<leader>x` | Diagnostics/Quickfix |

---

## Lua Development (for .lua files)

| Key | Mode | Description |
|-----|------|-------------|
| `<localleader>r` | n, x | Execute Lua code |

---

## Additional Plugins

### Vim Fugitive
Git integration plugin with standard Fugitive commands (`:Git`, `:Gread`, `:Gwrite`, etc.)

### Git Blame
Shows git blame information inline

### Colorizer
Automatically highlights color codes

### Tailwind Tools
Provides tailwind CSS utilities (keymaps defined by plugin)

### Blink.cmp (Completion)
| Key | Mode | Description |
|-----|------|-------------|
| `<Tab>` | i | Accept completion |
| `<CR>` | i | Disabled (configured to do nothing) |

**Note:** Auto-show for completion menu is disabled; completions appear on demand.

---

## Configuration Notes

- **Leader Key:** Space (`<Space>`)
- **Local Leader Key:** Backslash (`\`)
- **Tab Width:** 4 spaces
- **Line Numbers:** Relative line numbers enabled
- **Scrolloff:** 10 lines
- **Colorscheme:** rose-pine
- **Terminal:** Uses ToggleTerm with floating direction

### LSP Servers Configured
- bashls (Bash)
- bufls (Protocol Buffers)
- cssls (CSS)
- dotls (Graphviz DOT)
- lua_ls (Lua)
- sourcekit (Swift)
- vimls (VimScript)
- Plus all language servers from enabled LazyVim language extras

### Disabled Features
- Noice LSP hover and signature (using native LSP floating windows)
- Snacks.nvim explorer (using Neo-tree instead)
- Virtual text diagnostics (using virtual lines for current line only)
- Blink.cmp auto-show menu (manual trigger only)

---

## Resources

- [LazyVim Documentation](https://lazyvim.github.io/)
- [Neovim Documentation](https://neovim.io/doc/)
- [Mini.nvim Documentation](https://github.com/echasnovski/mini.nvim)
- [Telescope Documentation](https://github.com/nvim-telescope/telescope.nvim)

---

*Last Updated: 2025-01-22*
