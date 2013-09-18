# puritan4.vim

My vim configuration. It works for me.

## introduction

This is my ***personal*** vim configuration that I forked from bling's original.  It's basically the same thing he does, except much lighter. I've removed everything I don't use in my normal work.  Well, not really everything. I left a few things that I think are interesting or that I'm trying out, but I've removed a bunch.

If you want the full effect, with more plugins than you can count, get bling's original:
*  [bling](https://github.com/bling/dotvim)

## installation

1.  clone this repository into your `~/.vim` directory
1.  `git submodule init && git submodule update`
1.  `mv ~/.vimrc ~/.vimrc.backup`
1.  create the following shim and save it as `~/.vimrc`:

```
let g:dotvim_settings = {}
let g:dotvim_settings.version = 1
source ~/.vim/vimrc
```

1.  startup vim and neobundle will detect and ask you install any missing plugins.  you can also manually initiate this with `:NeoBundleInstall`
1.  done!

### versioning

the `g:dotvim_settings.version` is a simple version number which is manually edited.  it is used to detect whether significant breaking changes have been introduced so that users of the distribution can be notified accordingly.

## customization

*  since the distribution is just one file, customization is straightforward.  any customizations can be added to the `g:dotvim_settings` variable, which will be used whilst sourcing the distribution's `vimrc` file.  here is an example:

```
" this is the bare minimum
let g:dotvim_settings = {}
let g:dotvim_settings.version = 1

" here are some basic customizations, please refer to the top of the vimrc file for all possible options
let g:dotvim_settings.default_indent = 3
let g:dotvim_settings.max_column = 80
let g:dotvim_settings.colorscheme = 'my_awesome_colorscheme'

" by default, language specific plugins are not loaded.  this can be changed with the following:
let g:dotvim_settings.plugin_groups_exclude = ['ruby','python']

" if there are groups you want always loaded, you can use this:
let g:dotvim_settings.plugin_groups_include = ['go']

" alternatively, you can set this variable to load exactly what you want
let g:dotvim_settings.plugin_groups = ['core','web']

" if there is a particular plugin you don't like, you can define this variable to disable them entirely
let g:dotvim_settings.disabled_plugins=['vim-foo','vim-bar']

" finally, load the distribution
source ~/.vim/vimrc

" anything defined here are simply overrides
set wildignore+=\*/node_modules/\*
set guifont=Wingdings:h10
```

## autocomplete

this distribution will pick one of two combinations, in the following priority:

1.  [neocomplete][nc] + [neosnippet][ns] if you have `lua` enabled.
2.  [neocomplcache][ncl] + [neosnippet][ns] if you only have vimscript available

this can be overridden with `g:dotvim_settings.autocomplete_method`

## standard modifications

*  if you have either [ack](http://betterthangrep.com/) or [ag](https://github.com/ggreer/the_silver_searcher) installed, they will be used for `grepprg`
*  all temporary files are stored in `~/.vim/.cache`, such as backup files and persistent undo

## mappings

### insert mode
*  `<C-h>` move the cursor left
*  `<C-l>` move the cursor right
*  `jk`, `kj` remapped for "smash escape"

### normal mode
*  `<leader>fef` format entire file
*  `<leader>f$` strip current line of trailing white space
*  window shortcuts
  *  `<leader>v` vertical split
  *  `<leader>s` horizontal split
  *  `<leader>vsa` vertically split all buffers
  *  `<C-h>` `<C-j>` `<C-k>` `<C-l>` move to window in the direction of hkjl
*  window killer
  *  `Q` remapped to close windows and delete the buffer (if it is the last buffer window)
* searching
  *  `<leader>fw` find the word under cursor into the quickfix list
  *  `<leader>ff` find the last search into the quickfix list
  *  `/` replaced with `/\v` for sane regex searching
  *  `<cr>` toggles hlsearch
*  `<Down>` `<Up>` maps to `:bprev` and `:bnext` respectively
*  `<Left>` `<Right>` maps to `:tabprev` and `:tabnext` respectively
*  `gp` remapped to visually reselect the last paste
*  `gb` for quick going to buffer
*  `<leader>l` toggles `list` and `nolist`
*  profiling shortcuts
   * `<leader>DD` starts profiling all functions and files into a file `profile.log`
   * `<leader>DP` pauses profiling
   * `<leader>DC` continues profiling
   * `<leader>DQ` finishes profiling and exits vim

### visual mode
*  `<leader>s` sort selection
*  `>` and `<` automatically reselects the visual selection

## plugins

### [unite.vim](https://github.com/Shougo/unite.vim)
*  this is an extremely powerful plugin that lets you build up lists from arbitrary sources
*  mappings
  *  `<space><space>` go to anything (files, buffers, MRU, bookmarks)
  *  `<space>y` select from previous yanks
  *  `<space>l` select line from current buffer
  *  `<space>b` select from current buffers
  *  `<space>o` select from outline of current file
  *  `<space>s` quick switch buffer
  *  `<space>/` recursively search all files for matching text (uses `ag` or `ack` if found)

### [fugitive](https://github.com/tpope/vim-fugitive)
*  git wrapper
*  `<leader>gs` status
*  `<leader>gd` diff
*  `<leader>gc` commit
*  `<leader>gb` blame
*  `<leader>gl` log
*  `<leader>gp` push
*  `<leader>gw` stage
*  `<leader>gr` rm
*  in addition to all the standard bindings when in the git status window, you can also use `U` to perform a `git checkout --` on the current file

### [ctrlp](https://github.com/kien/ctrlp.vim)
*  fuzzy file searching
*  `<C-p>` to bring up the search
*  `\t` search the current buffer tags
*  `\T` search global tags
*  `\l` search all lines of all buffers
*  `\b` search open buffers
*  `\o` parses the current file for functions with [funky](https://github.com/tacahiroy/ctrlp-funky)

### [undotree](https://github.com/mbbill/undotree)
*  visualize the undo tree
*  `<F5>` to toggle

### [neocomplcache][ncl]/[neosnippet][ns]
*  autocomplete/snippet support as a fallback choice when YCM and/or python is unavailable
*  `<Tab>` to select the next match, or expand if the keyword is a snippet
*  if you have lua installed, it will use [neocomplete][nc] instead

### [vimshell](https://github.com/Shougo/vimshell)
*  `<leader>c` splits a new window with an embedded shell

# and some more plugins
*  [surround](https://github.com/tpope/vim-surround) makes for quick work of surrounds
*  [repeat](https://github.com/tpope/vim-repeat) repeat plugin commands
*  [signature](https://github.com/kshenoy/vim-signature) shows marks beside line numbers
*  [matchit](https://github.com/vim-scripts/matchit.zip) makes your `%` more awesome
*  [syntastic](https://github.com/scrooloose/syntastic) awesome syntax checking for a variety of languages
*  [indent-guides](https://github.com/nathanaelkane/vim-indent-guides) vertical lines
*  [signify](https://github.com/mhinz/vim-signify) adds + and - to the signs column when changes are detected to source control files (supports git/hg/svn)
*  [delimitmate](https://github.com/Raimondi/delimitMate) automagically adds closing quotes and braces
*  [startify](https://github.com/mhinz/vim-startify) gives you a better start screen

# and even more plugins...
*  i think i've listed about half of the plugins contained in this distribution, so please have a look at the vimrc directly to see all plugins in use

## credits

i wanted to give special thanks to all of the people who worked on the following projects, or people simply posted their vim distributions, because i learned a lot and took many ideas and incorporated them into my configuration.

*  [bling](https://github.com/bling/dotvim)
*  [janus](https://github.com/carlhuda/janus)
*  [spf13](https://github.com/spf13/spf13-vim)
*  [yadr](http://skwp.github.com/dotfiles/)
*  [astrails](https://github.com/astrails/dotvim)
*  [tpope](https://github.com/tpope)
*  [scrooloose](https://github.com/scrooloose)
*  [shougo](https://github.com/Shougo)
*  [lokaltog](https://github.com/Lokaltog)
*  [sjl](https://github.com/sjl)
*  [terryma](https://github.com/terryma)

## license
[WTFPL](http://sam.zoy.org/wtfpl/)

## changelog

*  v1
	* removed all the plugins I don't use.


