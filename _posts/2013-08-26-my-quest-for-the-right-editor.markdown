---
layout: post
date: 2013-08-26 12:17
title: "My quest for the right $EDITOR"
---

Five years ago I was already into Ruby on Rails, and I was envy of Mac users
because of [TextMate][].  I watched every new [Railscasts][] episode each week,
religiously, and I envied Ryan Bates using that awesome editor that made things
so easy.  I mostly used Linux back then, and there were alternatives like
gedit, but they were not the same.

[TextMate]: http://www.macromates.com/
[Railscasts]: http://railscasts.com/

I've been always aware of the Emacs and Vim hegemony among Unix dwellers, but I
crashed into a wall every time I tried to crack a dent into any of those two.
I had a slightly longer attempt on Emacs back in '08, but it did not end well.

In 2009 I finally managed to get my hands on a MacBook, and Mac OS X was
everything I thought it to be, and more.  And among all its goodies, I finally
had TextMate to soothe that envy.  I finally had a reach for those nifty little
features, such as snippets, language bundles, and the wonderful ⌘T for quickly
fuzzy-finding any file in the project, and opening it.  It was paradise.

## Breaking the ice

Last year though, I got the opportunity to work with [a great fellow
programmer][], and fellow countryman too, and his use of Vim made me envious.
I actually never saw him doing much on Vim.  It was just the fact that he had
mastered it to a point where it was his preferred editor, that triggered back
on me the craving for being able to use one of Unix two main editors.

This desire is not just a mere _"being a more unix kind of guy"_, or
_"appearing more like a true hacker"_.  There are advantages for going either
down the Emacs or Vim roads when you work on Unix environments.  The main one
being that **your editor can live in your terminal**, meaning you can use it
every time, even within an ssh session into another server.  You can also use
it on virtually any platform there is, something you can't do with TextMate, or
even [Sublime][].

So, to make a long story short.  I ended up really motivated to break that
wall, run fiercely up that learning curve, and really get to know Vim as much
as possible.  After a couple of months I knew there was no going back.  A month
later, I was so much into it, that I finally decided to roll up [my own
dotfiles][] so that I can persist my customizations, and share them among all
the command line terminals that I use.

[my own dotfiles]: https://github.com/gnapse/dotfiles
[a great fellow programmer]: http://adrianperez.org
[Sublime]: http://www.sublimetext.com

In the process, as a by-product, I also ended up learning a great deal of stuff
about other complementary tools commonly found in a Unix ecosystem, such as
tmux, git, shell scripting, zsh, bash, etc.  This made the journey even more
worthwhile than just learning a new code editor.

In the end... well, the end hasn't arrived yet. And it probably never will.
Vim provides an endless experience of learning.  I've heard from seasoned users
that are still learning new things about it after 15+ years of using it.  But
at least today, after six months into it, I do have the following line of code
in my shell initialization files:

```sh
export EDITOR="vim"
```

And I do not expect it to change anytime soon ;)

## Give it a try

So if...

- you are in the software development business...
- and you work most of the time in a unix-like environment...
- and you're not yet using either Vim or Emacs...

then I encourage you to really give it a try to both, decide which one fits you
best, and get to learn it enough.  In the end, you might not decide to switch
completely, you might prefer to stay in the comfort of Sublime or TextMate or
whatever editor you use on a daily basis.  But you might end up knowing a
little bit more about your unix environment, and you'll end up gaining an
editor to use in those remote ssh sessions where those other editors are not to
be found.

In case you go for Emacs, that's perfectly fine.  I think **there's enough room
in this world for both editors**.  It's perhaps even more customizable than
Vim, in the sense that it goes beyond being a mere editor, and people have even
made mail and chat clients out of it, and surely a lot more.  You'll be amazed.

In case you go for Vim, it's more focused than Emacs in being just a text
editor, and nothing else.  **But it rocks at it!**  It's less intuitive than
anything you've used, in the sense that it uses the revolutionary concept of
different editing modes.  Don't let this feature scare you.  It's much more
useful than you might think at first, and will change the way you look at code
and work with it.

In case you go for Vim, here's a minimal set of instructions to get started:

- Make sure you have a recent version of Vim installed on your system.
- Open your terminal and run the `vimtutor` command.
- Make your way through the tutorial.

Here are some online resources I think you'll find useful too:

- [Vimcasts](http://vimcasts.org).  These are an absolute must, so don't miss it.
- [Vim: revisited](http://mislav.uniqpath.com/2011/12/vim-revisited/). Vim intro by @mislav
- [Coming home to Vim](http://stevelosh.com/blog/2010/09/coming-home-to-vim/). Vim intro by @stevelosh.
- [Vim intro by @wycats](http://yehudakatz.com/2010/07/29/everyone-who-tried-to-convince-me-to-use-vim-was-wrong/).
- [Walking with crutches](http://walking-without-crutches.heroku.com/). A slideshow by @nelstrom.
- [Tim Pope at github](https://github.com/tpope). Great Vim plugins by @tpope.
