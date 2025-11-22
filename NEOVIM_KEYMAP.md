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
18. [Mini Plugins](#mini-plugins)
19. [Mason (LSP Installer)](#mason-lsp-installer)
20. [Tabs](#tabs)
21. [Quickfix & Location List](#quickfix--location-list)
22. [LazyVim UI](#lazyvim-ui)

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

| Key | Mode | Description |
|-----|------|-------------|
| `<S-h>` | n | Previous buffer |
| `<S-l>` | n | Next buffer |
| `[b` | n | Previous buffer (alternate) |
| `]b` | n | Next buffer (alternate) |
| `<leader>bb` | n | Switch to other buffer |
| `<leader>\`` | n | Switch to other buffer (alternate) |
| `<leader>bd` | n | Delete buffer |
| `<leader>bo` | n | Delete other buffers |
| `<leader>bD` | n | Delete buffer and window |
| `<leader>tb` | n | Show buffers (Telescope) |

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
| Key | Mode | Description |
|-----|------|-------------|
| `gd` | n | Goto Definition |
| `gD` | n | Goto Declaration |
| `gr` | n | References |
| `gI` | n | Goto Implementation |
| `gy` | n | Goto Type Definition |
| `]]` | n | Next Reference |
| `[[` | n | Prev Reference |
| `<A-n>` | n | Next Reference (alternate) |
| `<A-p>` | n | Prev Reference (alternate) |

### Documentation & Help
| Key | Mode | Description |
|-----|------|-------------|
| `K` | n | Hover documentation |
| `gK` | n | Signature Help |
| `<C-k>` | i | Signature Help (insert mode) |

### Code Actions
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>ca` | n, x | Code Action |
| `<leader>cA` | n | Source Action |
| `<leader>cr` | n | Rename symbol |
| `<leader>cR` | n | Rename File |
| `<leader>cf` | n, x | Format code |

### Code Lens
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>cc` | n, x | Run Codelens |
| `<leader>cC` | n | Refresh & Display Codelens |

### LSP Management
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>cl` | n | LSP Info |
| `<leader>cm` | n | Mason (LSP installer) |
| `<leader>cs` | n | Symbols |

---

## Diagnostics

| Key | Mode | Description |
|-----|------|-------------|
| `<leader>cd` | n | Show line diagnostics |
| `<leader>ud` | n | Toggle diagnostics |
| `]d` | n | Next diagnostic |
| `[d` | n | Previous diagnostic |
| `]e` | n | Next error |
| `[e` | n | Previous error |
| `]w` | n | Next warning |
| `[w` | n | Previous warning |

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

| Key | Mode | Description |
|-----|------|-------------|
| `<C-t>` | n, t | Toggle terminal (ToggleTerm) |
| `<C-\`>` | n, t | Toggle terminal (ToggleTerm alternate) |
| `<C-/>` | n, t | Toggle terminal (root directory) |
| `<C-_>` | n, t | Toggle terminal (alternate binding) |
| `<leader>fT` | n | Terminal at current working directory |
| `<leader>ft` | n | Terminal at root |

---

## Telescope

### Custom Keymaps
| Key | Mode | Description |
|-----|------|-------------|
| `<leader>tf` | n | Find Files (Telescope) |
| `<leader>tg` | n | Live Grep (Telescope) |
| `<leader>tb` | n | Show Buffers (Telescope) |
| `<leader>th` | n | Show Help Tags (Telescope) |
| `<leader>td` | n | TODO List (TodoTelescope) |

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

| Key | Mode | Description |
|-----|------|-------------|
| `<leader>tt` | n | Toggle Diagnostics (Trouble) |
| `<leader>xx` | n | Toggle Diagnostics (alternate) |
| `<leader>xX` | n | Buffer Diagnostics |
| `<leader>cs` | n | Symbols |
| `<leader>xL` | n | Location List |
| `[q` | n | Previous item |
| `]q` | n | Next item |

---

## TODO Comments

| Key | Mode | Description |
|-----|------|-------------|
| `<leader>td` | n | Toggle TODO list |
| `<leader>xt` | n | TODO List (alternate) |
| `<leader>st` | n | Search TODOs |
| `]t` | n | Next TODO |
| `[t` | n | Previous TODO |

---

## Neo-tree (File Explorer)

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

## Mini Plugins

### Mini.surround
See [Surround Operations](#surround-operations-minisurround) section above.

### Mini.move
See [Visual Selection Movement](#visual-selection-movement-minimove) section above.

---

## Mason (LSP Installer)

| Key | Mode | Description |
|-----|------|-------------|
| `<leader>ma` | n | Toggle Mason menu |
| `<leader>cm` | n | Mason (alternate) |

---

## Tabs

| Key | Mode | Description |
|-----|------|-------------|
| `<leader><tab><tab>` | n | New tab |
| `<leader><tab>l` | n | Go to last tab |
| `<leader><tab>f` | n | Go to first tab |
| `<leader><tab>]` | n | Next tab |
| `<leader><tab>[` | n | Previous tab |
| `<leader><tab>d` | n | Close tab |
| `<leader><tab>o` | n | Close other tabs |

---

## Quickfix & Location List

| Key | Mode | Description |
|-----|------|-------------|
| `<leader>xq` | n | Toggle quickfix list |
| `<leader>xl` | n | Toggle location list |
| `<leader>xL` | n | Location List (Trouble) |
| `[q` | n | Previous quickfix item |
| `]q` | n | Next quickfix item |

---

## LazyVim UI

| Key | Mode | Description |
|-----|------|-------------|
| `<leader>l` | n | Open Lazy plugin manager |
| `<leader>L` | n | Show LazyVim changelog |
| `<leader>qq` | n | Quit all |
| `<leader>ui` | n | Inspect highlight at cursor |
| `<leader>uI` | n | Inspect treesitter tree |
| `<leader>?` | n | Buffer keymaps help |

---

## Which-Key Groups

These are prefix groups for organizing keymaps:

| Prefix | Group Name |
|--------|-----------|
| `<leader><tab>` | Tabs |
| `<leader>b` | Buffer |
| `<leader>c` | Code / Claude |
| `<leader>d` | Debug |
| `<leader>dp` | Profiler |
| `<leader>f` | File/Find |
| `<leader>g` | Git |
| `<leader>gh` | Git Hunks |
| `<leader>q` | Quit/Session |
| `<leader>s` | Search |
| `<leader>t` | Telescope/Todo/Trouble |
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

*Last Updated: 2025-11-22*
