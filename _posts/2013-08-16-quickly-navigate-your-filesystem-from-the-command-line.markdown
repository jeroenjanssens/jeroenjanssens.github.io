---
layout: post
title:  "Quickly navigate your filesystem from the command-line"
date:   2013-08-16 19:06:19
---
*Update (6-9-2013) This code is now also available as the "jump" plugin in [oh-my-zsh][ohmyzsh].*

*Update (18-8-2013): Thanks to the many useful suggestions in [the discussion on Hacker News][hn], I have added (1) quotes to the code, (2) a section about tab completion, and (3) a note for Mac OS X users.*

Like many others, I spend most of my day behind a computer.
In order make the most of it (and to keep my body from complaining too much), I try to maintain an optimized setup.
For example, I code in [Vim][vim], browse with [Vimperator][vimperator], and move windows around in [i3][i3].
Another common task is filesystem navigation. 
I prefer to use the command-line for this, but typing `cd ~/some/very/deep/often-used/directory` over and over again does become cumbersome.

Automated tools like [autojump][autojump], [z][z], and [fasd][fasd] address this problem by offering shortcuts to the directories you often go to.
Personally, I prefer a more manual solution, which I would like to share with you.
I have noticed quite an increase in efficiency with this, and perhaps you will too.

###  Jumping with symbolic links

Under the hood this manual solution comes down to storing symbolic links in a hidden directory (e.g., `~/.marks`).
There are four shell functions `jump`, `mark`, `unmark`, and `marks`, and they look like this:

```bash
export MARKPATH=$HOME/.marks
function jump { 
	cd -P "$MARKPATH/$1" 2>/dev/null || echo "No such mark: $1"
}
function mark { 
	mkdir -p "$MARKPATH"; ln -s "$(pwd)" "$MARKPATH/$1"
}
function unmark { 
	rm -i "$MARKPATH/$1"
}
function marks {
	ls -l "$MARKPATH" | sed 's/  / /g' | cut -d' ' -f9- | sed 's/ -/\t-/g' && echo
}
```

Put this in your `.zshrc` or `.bashrc` and you're ready to jump (Mac OS X users need a slightly different version of the `marks` function; see below). I have also turned this into a plugin for [oh-my-zsh][ohmyzsh] called `jump`. To add a new bookmark, `cd` into the directory and `mark` it with a name to your liking:

```bash
$ cd ~/some/very/deep/often-used/directory
$ mark deep
```

This adds a symbolic link named `deep` to the directory `~/.marks`. To `jump` to this directory, type the following from any place in the filesystem:

```bash
$ jump deep
```

To remove the bookmark (i.e., the symbolic link), type:

```bash
$ unmark deep
```

You can view all marks by typing:

```bash
$ marks

deep	-> /home/johndoe/some/very/deep/often-used/directory
foo		-> /usr/bin/foo/bar
```

That's all there is to it! 

### Adding tab completion

In order to add tab completion for the `jump` and `unmark` functions, add the following code to your .zshrc (thanks to [tiziano88][hn1]):

```bash
function _completemarks {
  reply=($(ls $MARKPATH))
}

compctl -K _completemarks jump
compctl -K _completemarks unmark
```

or the following to your .bashrc (thanks to [microcolonel][hn2]):

```bash
_completemarks() {
  local curw=${COMP_WORDS[COMP_CWORD]}
  local wordlist=$(find $MARKPATH -type l -printf "%f\n")
  COMPREPLY=($(compgen -W '${wordlist[@]}' -- "$curw"))
  return 0
}

complete -F _completemarks jump unmark
```

If you now type `jump` or `unmark` and then press TAB, you see a list of available marks. Neat!

### Note for Mac OS X users

As pointed out by [guygurari][hn3], Mac OS X users need a slightly different version of the `marks` function:

```bash
function marks {
	\ls -l "$MARKPATH" | tail -n +2 | sed 's/  / /g' | cut -d' ' -f9- | awk -F ' -> ' '{printf "%-10s -> %s\n", $1, $2}'
}
```

If you like what I had to say then you may want to [follow me on Twitter][twitter].

[autojump]: https://github.com/joelthelion/autojump
[fasd]: https://github.com/clvv/fasd

[vim]: http://en.wikipedia.org/wiki/Vim_(text_editor)
[vimperator]: http://www.vimperator.org/vimperator
[i3]: http://i3wm.org
[zsh]: http://en.wikipedia.org/wiki/Z_shell
[zshrc]: https://github.com/jeroenjanssens/dotfiles/blob/master/home/.zshrc

[pullrequest]: https://github.com/robbyrussell/oh-my-zsh/pull/2045
[ohmyzsh]: https://github.com/robbyrussell/oh-my-zsh
[twitter]: https://twitter.com/jeroenhjanssens/
[hn]: https://news.ycombinator.com/item?id=6229001
[z]: https://github.com/rupa/z

[hn1]: https://news.ycombinator.com/item?id=6229468
[hn2]: https://news.ycombinator.com/item?id=6229768
[hn3]: https://news.ycombinator.com/item?id=6229428

