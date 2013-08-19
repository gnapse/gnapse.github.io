---
layout: post
title: "Easier use of the vim-surround plugin"
date: 2013-08-12 14:53
tags: [vim, plugin]
categories: programming
---

Tim Pope's [vim-surround][] plugin is awesome, but I sometimes find myself in a
situation where I want to make it easier and more intuitive to use, perhaps
inspired on how the equivalent functionality works in TextMate and Sublime
out-of-the-box.

It turns out it was not difficult at all.  The plugin does all the heavy
lifting, and a few simple custom key bindings do the rest.

My aim when designing these key mappings was to achieve surrounding the
selected text in visual mode simply by typing the corresponding surround
character.  The only gotcha is that some of these characters, specially the
double quotes, have a special and very useful meaning in visual mode, so I
decided to prepend the `<leader>` key to the mappings.

So enough chit-chat, and on to the actual code:

``` vim
" Surround text currently selected in visual mode
" (The surrounded text is kept selected after being surround)
vmap <leader>" S"lvi"
vmap <leader>' S'lvi'
vmap <leader>` S`lvi`
vmap <leader>( S)lvi(
vmap <leader>{ S}lvi{
vmap <leader>[ S]lvi[
vmap <leader>< S>lvi<
```

Include the above lines in your `~/.vimrc` file, and remember to install the
[vim-surround][] plugin if you haven't already.

Now, whenever you're in visual mode, and you type the `<leader>` key followed
by any of the valid surrounding characters, the selected text is surround and
kept selected.  The valid surround characters are `(`, `{`, `[`, `<`, `'`, `"`,
`` ` ``.  Also, the fact that the selection is kept allows for typing more than
one of these key combinations in a row, and it keeps surrounding on and on!

[vim-surround]: https://github.com/tpope/vim-surround
