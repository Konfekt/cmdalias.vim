# vim-alias

This plugin lets you define command-line abbreviations by `:Alias` which only expand at the beginning of the command prompt.
These command-line abbreviations work like the bash aliases, except that the alias is substituted in-place.

Check out [why](https://konfekt.github.io/blog/2016/10/03/get-the-leader-right) an alias is most often preferable to a `<leader>` mapping!

## Usage

You can pass the optional parameters `-buffer` or `-range` to create a buffer-local alias or one that accepts a range preceding the alias.

```vim
    :Alias [-range] [-buffer] <lhs> <rhs>
    :UnAlias [-buffer]  <lhs> ...
    :Aliases [-buffer] [<lhs> ...]
```

To define the Aliases after Vim has started up, define them in
`~/.vim/after/plugin/alias.vim`, or add to `~/.vimrc`

```vim
if exists('s:loaded_vimafter')
  silent doautocmd VimAfter VimEnter *
else
  let s:loaded_vimafter = 1
  augroup VimAfter
    autocmd!
    autocmd VimEnter * source ~/.vim/after/vimrc.vim
  augroup END
endif
```

and define them in `~/.vim/after/vimrc.vim`.

### Examples:

```vim
    :Alias   -range   dg   <c-r>=&l:diff?"diffget":"dg"<cr>
    :Alias   -buffer  spl  setlocal\ spell<bar>setlocal\ spelllang=en
    :Alias            w!!  write\ !sudo\ tee\ >\ /dev/null\ %
    :Alias            F    find\ *<c-r>=Eatchar("\ ")<cr>
    :Alias            th   tab\ help
    :Alias            sft  setfiletype
    :Alias -range     il   ilist\ /\v/<left><c-r>=EatChar("\ ")<cr>
    :Alias -range     dl   dlist\ //<left><c-r>=EatChar("\ ")<cr>
    :Alias            g    Silent git
    :Alias            gbl  Silent\ tig\ blame\ +<c-r>=line('.')<cr>\ --\ %<c-left><c-left><left>
    :Alias -range     tl   !translate\ -no-warn\ -no-ansi\ -brief\ -to
    :Alias            g!   g!
    :UnAlias          g
    :Aliases
```

See :help abbreviations for `Eatchar(c)`.
The command `:Silent` is defined as
```vim
    if has('unix') || has('win32unix')
        command! -complete=shellcmd -nargs=1 -bang Silent execute ':silent<bang> !' . <q-args> | execute ':redraw!'
    elseif has('win32')
        command! -complete=shellcmd -nargs=1 -bang Silent execute ':silent<bang> !start /b ' . <q-args> | execute ':redraw!'
    endif
```
The executable [tig](https://github.com/jonas/tig) is a text-mode interface for `git` and [translate-shell](https://github.com/soimort/translate-shell) a command-line translator.


## Configuration

The variable `g:cmdaliasCmdPrefixes` lists the patterns of all commands by
which an alias command may be preceded and yet expand. It defaults to

```vim
  let g:cmdaliasCmdPrefixes = [
    \ '\d*verb\%[ose]', 'debug', 'sil\%[ent]!\?', 'uns\%[ilent]', 'redir\?!\?',
    \ '.*[^|]|',
    \ 'ld!\?', '[cl]fd!\?', '[cl]f\?do!\?',
    \ '\%(\%([.$]\|\d\+\)\%([,;]\%([.$]\|\d\+\)\)*\)\?\s*' .
    \ '\%(argdo\?!\?\|bufdo\?!\?\|windo\?\|tabdo\?\)' ]
```

## Installation

If you use [vim-plug](https://github.com/junegunn/vim-plug), then add the
following line to your `vimrc` file:

```vim
Plug 'Konfekt/vim-alias'
```

Credits
-------

This plugin builds and improves on [cmdalias.vim 3.0](http://www.vim.org/scripts/script.php?script_id=746) by Hari Krishna Dara by

- allowing for aliases of commands preceded by a range (like :Alias -range dg
  diffget),
- allowing for alias names ending in non-word chars (like :Alias w!! ...),
- having a Vim documentation, and
- more checks for proper usage, more consistent parameter parsing and a finer
  check for blanks or commands (such as :  silent! ) preceding the alias.
