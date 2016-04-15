# vim-alias

This plugin lets you define command-line abbreviations by `:Alias` which only expand at the beginning of the command prompt.

You can pass the optional parameters

  `-buffer`  or  `-range`

to create a buffer local alias or one that accepts a range preceding the
alias.

These command line abbreviations work like the bash aliases, except that
the alias is substituted in-place.

## Usage

```vim
    :Alias [-range] [-buffer] <lhs> <rhs>
    :UnAlias <lhs> ...
    :Aliases [<lhs> ...]
```

### Examples:

```vim
    :Alias   -range   dg   <c-r>=&l:diff?"diffget":"dg"<cr>
    :Alias   -buffer  spl  setlocal\ spell<bar>setlocal\ spelllang=en
    :Alias            w!!  write\ !sudo\ tee\ >\ /dev/null\ %
    :Alias            F    find\ *<c-r>=EatChar("\ ")<cr>
    :Alias            th   tab\ help
    :Alias            sft  setfiletype
    :Alias            g    !git
    :UnAlias          g
    :Aliases
```

  See :help abbreviations for Eatchar(c).

## Configuration

The variable `g:cmdaliasCmdPrefixes` lists the patterns of all commands by
which an alias command may be preceded and yet expand. It defaults to

```vim
  let g:cmdaliasCmdPrefixes = [
      \ '\d*verb\%[ose]', 'debug', 'sil\%[ent]!\?', '\%uns[ilent]' 'redir\?!\?',
      \ 'ld!\?', '[cl]fd!\?', '[cl]f\?do!\?',
      \ '\%(\%([.$]\|\d\+\)\%([,;]\%([.$]\|\d\+\)\)*\)\?\s*' .
      \ '\%(argdo\?!\?\|bufdo\?!\?\|windo\?\|tabdo\?\)' ]
```

## Installation

If you use [vim-plug](https://github.com/junegunn/vim-plug), then add the
following line to your `vimrc` file:

```vim
Plug 'Konfekt/vim-alias
```

Credits
-------

This plugin builds and improves on cmdalias.vim 3. 0 by Hari Krishna Dara by

- allowing for aliases of commands preceded by a range (like :Alias -range dg
  diffget),
- allowing for alias names ending in non-word chars (like :Alias w!! ...),
- having a Vim documentation, and
- more checks for proper usage, more consistent parameter parsing and a finer
  check for blanks or commands (such as :  silent! ) preceding the alias.
