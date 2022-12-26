This repository contains the Vim thesaurus files (see `:help thesaurus`)

- `de.ths` (for German)
- `en.ths` (for English)
- `es.ths` (for Spanish)
- `fr.ths` (for French)
- `it.ths` (for Italian)
- `pt.ths` (for Portuguese)

for completing the word before the cursor to a synonym in insert mode.

# Set-up

1. Copy your selected `ths` files to `~/.vim/thesauri` (respectively `%USERPROFILE%\vimfiles\thesauri` on Microsoft Windows), and
2. add to your `~/.vimrc` (respectively `%USERPROFILE%\_vimrc` on Microsoft Windows) an auto command such as

```vim
if exists('##OptionSet')
    autocmd OptionSet spelllang call SetThesaurus(&l:spelllang)
else
  autocmd BufWinEnter spelllang call SetThesaurus(&l:spelllang)
endif
  
let s:slash = exists('+shellslash') && !&g:shellslash ? '\' : '/'
let g:vimfiles_dir = split(&runtimepath, ',')[0]

function! SetThesaurus(lang) abort
  let ths = fnamemodify(resolve(g:vimfiles_dir . s:slash . 'thesauri' . s:slash . lang . '.ths'), ':p')
  if filereadable(ths)
    execute 'setlocal thesaurus=' . ths
  endif
endfunction
```

to automatically enable the thesaurus (whenever available) corresponding to your `&spelllang` (see `:help spelllang`).

3. Then setting `&spelllang` to one of the file names (without extension) of the available `ths` files and hitting `Ctrl-X, Ctrl-T` (see `:help i_CTRL-X_CTRL-T`) in Insert Mode shows a list of synonym of the word before the cursor, which will be replaced by the selected synonym.

# Caveat

Two words are considered synonym by Vim if there is a line that contains both of them (whereas, say in Libreoffice, only if one of them is at the beginning of the line).
See, in particular on Unix platforms, the recent `&thesaurusfunc` option (`:help thesaurusfunc`) to achieve instead such a behavior.

# Related

The plug-in [complete-common-words.vim](https://github.com/Konfekt/complete-common-words.vim) ensures that common words are shown first on completing prose.
