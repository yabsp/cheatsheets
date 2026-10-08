# Vim Cheat Sheet

Most Vim commands are built from an operator and a motion or text object:
`d` (delete) + `iw` (inner word) = `diw`. A count in front repeats it (`3dd`)
and `.` repeats the last change. Commands starting with `:` are typed in normal
mode and confirmed with `Enter`.

!!! note "Version"
    Vim 9.2 and Neovim 0.12. Everything without a version mark also works
    in Vim 9.0. Check yours with `vim --version` or `nvim --version`.
    Differences are listed in [Vim vs Neovim](#vim-vs-neovim).

## Coming From an IDE

| IntelliJ (Linux keymap) | Vim |
| --- | --- |
| Copy / cut / paste | `y` / `d` / `p` |
| Undo / redo | `u` / `Ctrl+r` |
| Duplicate line (`Ctrl+d`) | `yyp` |
| Delete line (`Ctrl+y`) | `dd` |
| Move line up / down (`Alt+Shift+Up/Down`) | `:m -2` / `:m +1` |
| Comment line (`Ctrl+/`) | `gcc` |
| Find (`Ctrl+f`) | `/text` |
| Replace (`Ctrl+r`) | `:%s/old/new/gc` |
| Find in files (`Ctrl+Shift+f`) | `:vimgrep /text/ **/*` |
| Extend selection (`Ctrl+w`) | `viw`, `vi(`, `vip`, repeat `i(` to grow |
| Select next occurrence (`Alt+j`) | `*`, then `cgn`, then `.` |
| Go to line (`Ctrl+g`) | `:42` or `42G` |
| Go to declaration (`Ctrl+b`) | `gd` |
| Back / forward (`Ctrl+Alt+Left/Right`) | `Ctrl+o` / `Ctrl+i` |
| Go to file (`Ctrl+Shift+n`) | `:find name` |
| Recent files (`Ctrl+e`) | `:browse oldfiles` |
| Reformat file (`Ctrl+Alt+l`) | `gg=G` |
| Rename (`Shift+F6`) | `:%s/\<old\>/new/gc` |
| Clipboard history (`Ctrl+Shift+v`) | `:reg` |

## Basics

| Command | Description |
| --- | --- |
| `i` / `a` | Insert before / after the cursor |
| `I` / `A` | Insert at the start / end of the line |
| `o` / `O` | Open a new line below / above |
| `Esc` | Back to normal mode |
| `:w` | Save |
| `:q` / `:q!` | Quit / quit and discard changes |
| `:wq` or `ZZ` | Save and quit |
| `:wa` / `:qa` | Save all / quit all |
| `u` / `Ctrl+r` | Undo / redo |
| `.` | Repeat the last change |
| `:earlier 5m` / `:later 5m` | Go back / forward in time |
| `:h ciw` | Help for any command |

## Moving

| Command | Description |
| --- | --- |
| `h` `j` `k` `l` | Left, down, up, right |
| `w` / `b` / `e` | Next word / previous word / end of word |
| `W` / `B` / `E` | Same, but words are only split by spaces |
| `0` / `^` / `$` | Line start / first character / line end |
| `gg` / `G` | First / last line |
| `42G` or `:42` | Go to line 42 |
| `fx` / `Fx` | Jump to the next / previous `x` in the line |
| `tx` / `Tx` | Jump to just before the next / after the previous `x` |
| `;` / `,` | Repeat the last `f` or `t` forward / backward |
| `%` | Jump to the matching bracket |
| `{` / `}` | Previous / next empty line |
| `Ctrl+d` / `Ctrl+u` | Half a page down / up |
| `zz` | Center the current line on screen |
| `gd` | Go to the local definition |
| `gf` | Open the file under the cursor |
| `Ctrl+o` / `Ctrl+i` | Jump back / forward |
| `` `. `` | Go to the last change |
| `gi` | Continue inserting where you last stopped |
| `ma` / `` `a `` | Set mark `a` / jump back to it |

## Text Objects

Use them after an operator (`d`, `c`, `y`, `v`, `>`, `gU` ...). `i` means
inside, `a` includes the delimiters or surrounding space.

| Object | Description |
| --- | --- |
| `iw` / `aw` | Word |
| `i"` / `a"` | Double quotes, also `'` and `` ` `` |
| `i(` / `a(` | Parentheses, also `ib` |
| `i{` / `a{` | Braces, also `iB` |
| `i[` / `a[` | Brackets |
| `i<` / `a<` | Angle brackets |
| `it` / `at` | HTML or XML tag |
| `ip` / `ap` | Paragraph |

| Example | Description |
| --- | --- |
| `ciw` | Change the word |
| `ci"` | Change the text in quotes, works from anywhere before them on the line |
| `da(` | Delete the parentheses and their content |
| `yi{` | Copy the content of a block |
| `dit` | Delete the content of a tag |
| `vip` | Select the paragraph |

## Editing

| Command | Description |
| --- | --- |
| `x` | Delete the character |
| `rx` | Replace the character with `x` |
| `R` | Overwrite mode |
| `dd` / `D` | Delete the line / to the end of the line |
| `cc` / `C` | Change the line / to the end of the line |
| `dw` / `cw` | Delete / change to the end of the word |
| `J` | Join the line below |
| `>>` / `<<` | Indent / unindent |
| `==` / `gg=G` | Reindent the line / the whole file |
| `~` | Toggle case of the character |
| `gUiw` / `guiw` | Uppercase / lowercase the word |
| `Ctrl+a` / `Ctrl+x` | Increment / decrement the number under the cursor |
| `yyp` | Duplicate the line |
| `:m +1` / `:m -2` | Move the line down / up |
| `gcc` / `gcip` | Toggle comment on the line / paragraph (Vim 9.1.0375+) |

`gc` is built into Neovim. In Vim 9.1.0375 and later run `:packadd comment`
first or add it to the [config file](#config-file).

## Visual Mode

| Command | Description |
| --- | --- |
| `v` / `V` / `Ctrl+v` | Select characters / lines / a block |
| `o` | Jump to the other end of the selection |
| `gv` | Reselect the last selection |
| `viw` / `vi(` / `vip` | Select a text object, repeat `i(` or `a(` to grow it |
| `y` / `d` / `c` | Copy / delete / change the selection |
| `p` | Replace the selection, the replaced text goes to the register |
| `P` | Replace the selection, keep the register for pasting again |
| `>` / `<` | Indent / unindent, `.` repeats |
| `=` | Reindent |
| `J` | Join the lines |
| `U` / `u` / `~` | Uppercase / lowercase / toggle case |
| `rx` | Replace every selected character with `x` |
| `gc` | Toggle comment (Vim 9.1.0375+) |
| `:` | Run a command on the selected lines, shows `:'<,'>` |
| `:m '>+1` / `:m '<-2` | Move the selected lines down / up |
| `:sort` | Sort the selected lines |
| `:norm A;` | Append `;` to every selected line |
| `g Ctrl+a` | Turn equal numbers into a sequence 1, 2, 3 ... |

Block selection (`Ctrl+v`) works like multiple cursors:

| Command | Description |
| --- | --- |
| `I`, type, `Esc` | Insert text at the start of the block on every line |
| `$A`, type, `Esc` | Append text at the end of every line |
| `c`, type, `Esc` | Change the block on every line |
| `d` | Delete the block |

## Copy and Paste

| Command | Description |
| --- | --- |
| `yy` / `Y` | Copy the line |
| `yiw` / `y$` / `yip` | Copy the word / to the end of the line / the paragraph |
| `p` / `P` | Paste after / before the cursor |
| `gp` / `gP` | Same, but move the cursor after the pasted text |
| `]p` | Paste and match the current indentation |
| `"0p` | Paste the last copied text, even after deleting something |
| `"_dd` | Delete without overwriting the register |
| `"ayy` / `"ap` | Copy into / paste from register `a` |
| `"Ayy` | Append to register `a` |
| `"+y` / `"+p` | Copy to / paste from the system clipboard |
| `:reg` | Show all registers |
| `Ctrl+r 0` | Paste the last copied text in insert mode |
| `Ctrl+r +` | Paste the clipboard in insert mode |

Vim needs a build with clipboard support for `"+`, check with
`vim --version | grep clipboard`. On Debian and Ubuntu install `vim-gtk3`
instead of `vim`. Neovim needs `wl-clipboard` (Wayland) or `xclip` (X11).

## Search

| Command | Description |
| --- | --- |
| `/text` / `?text` | Search forward / backward |
| `n` / `N` | Next / previous match |
| `*` / `#` | Search the word under the cursor forward / backward |
| `g*` | Same as `*`, but also matches inside other words |
| `/\<word\>` | Whole word only |
| `/text\c` | Ignore case for this search |
| `/\v(foo)+` | Very magic: `( ) + ? { }` work without backslashes |
| `/` then `Up` | Search history |
| `:noh` | Clear the highlight |

## Replace

| Command | Description |
| --- | --- |
| `:s/old/new/` | Replace the first match in the line |
| `:s/old/new/g` | Replace all matches in the line |
| `:%s/old/new/g` | Replace all matches in the file |
| `:%s/old/new/gc` | Ask for each match: `y` yes, `n` no, `a` all, `q` quit |
| `:%s/old/new/gi` | Ignore case |
| `:%s/\<old\>/new/g` | Whole words only |
| `:'<,'>s/old/new/g` | Replace in the selected lines |
| `:'<,'>s/\%Vold/new/g` | Replace only inside the exact selection |
| `:%s//new/g` | Reuse the last search, e.g. after `*` |
| `:%s/old/&s/g` | `&` inserts the whole match |
| `:%s/\v(\w+)-(\w+)/\2-\1/g` | Groups: turns `foo-bar` into `bar-foo` |
| `:%s/, /,\r/g` | `\r` inserts a line break |
| `:%s/\w\+/\u&/g` | `\u` uppercases the next character, `\U` / `\L` all |
| `&` / `g&` | Repeat the last replace on this line / on all lines |
| `:g/text/d` | Delete every line containing `text` |
| `:v/text/d` | Delete every line not containing `text` |
| `:g/text/norm A;` | Run normal mode keys on every matching line |

Replace matches one by one: `*` (or `/text`), then `cgn`, type the new text,
`Esc`, then press `.` for each next match and `n` to skip one.

## Files, Splits and Project Search

| Command | Description |
| --- | --- |
| `:e file` | Open a file |
| `:find name` | Find and open a file in subfolders, `Tab` completes |
| `:Ex` | File explorer |
| `:ls` | List open files (buffers) |
| `:b name` | Switch to a buffer by part of its name |
| `:bn` / `:bp` / `:bd` | Next / previous / close buffer |
| `Ctrl+^` | Switch between the last two files |
| `:sp` / `:vs` | Split horizontal / vertical |
| `Ctrl+w h/j/k/l` | Move to the split in that direction |
| `Ctrl+w q` / `Ctrl+w o` | Close the split / close all other splits |
| `Ctrl+w =` | Make all splits equal size |
| `:tabnew` / `gt` / `gT` | New tab / next tab / previous tab |
| `:vimgrep /text/ **/*.py` | Search in files, results go to the quickfix list |
| `:copen` / `:cclose` | Open / close the result list |
| `:cn` / `:cp` | Next / previous result |
| `:cdo s/old/new/gc` | Replace in every result line |
| `:cfdo %s/old/new/g` | Replace in every result file |
| `:wa` | Save all files after a project wide replace |

## Macros

| Command | Description |
| --- | --- |
| `qa` | Start recording into register `a` |
| `q` | Stop recording |
| `@a` / `10@a` | Play once / 10 times |
| `@@` | Play the last macro again |
| `:'<,'>norm @a` | Play on every selected line |
| `@:` | Repeat the last `:` command |

## Insert Mode

| Command | Description |
| --- | --- |
| `Ctrl+n` / `Ctrl+p` | Complete a word from open files |
| `Ctrl+x Ctrl+f` | Complete a file path |
| `Ctrl+x Ctrl+l` | Complete a whole line |
| `Ctrl+w` | Delete the word before the cursor |
| `Ctrl+r a` | Insert register `a` |
| `Ctrl+o` | Run one normal mode command, then continue inserting |
| `Ctrl+t` / `Ctrl+d` | Indent / unindent the line |

## Config File

A minimal starting point for `~/.vimrc`. Vim 9.2 also reads
`~/.config/vim/vimrc`. Neovim already has most of these defaults and uses
`~/.config/nvim/init.vim` (or `init.lua`) instead.

```vim
" A vimrc disables Vim's defaults (syntax, incsearch, mouse ...), load them
unlet! skip_defaults_vim
source $VIMRUNTIME/defaults.vim

set number relativenumber
set ignorecase smartcase           " Case sensitive only with capitals
set hlsearch                       " Highlight all matches
set expandtab shiftwidth=4 tabstop=4
set path+=**                       " :find searches subfolders
set clipboard=unnamedplus          " y and p use the system clipboard

" gc / gcc to toggle comments (Vim 9.1.0375 and later)
silent! packadd! comment

" Ctrl+l also clears the search highlight
nnoremap <silent> <C-l> :nohlsearch<CR><C-l>
```

## Vim vs Neovim

| Topic | Vim 9.2 | Neovim 0.12 |
| --- | --- | --- |
| `Y` | Copies the line | Copies to the end of the line |
| `gc` / `gcc` | Needs `:packadd comment` | Built in |
| Clear highlight | `:noh` | `:noh` or `Ctrl+l` |
| System clipboard | Needs a `+clipboard` build | Needs `wl-clipboard` or `xclip` |
| Config | `~/.vimrc` | `~/.config/nvim/init.lua` |

With a language server set up, Neovim 0.11 and later adds IDE features with
default keys:

| Command | Description |
| --- | --- |
| `Ctrl+]` | Go to definition |
| `K` | Show documentation |
| `grn` | Rename the symbol |
| `grr` | List references |
| `gri` | Go to implementation |
| `gra` | Code actions |
| `gO` | List symbols in the file |
| `[d` / `]d` | Previous / next diagnostic |
| `Ctrl+s` | Signature help in insert mode |
