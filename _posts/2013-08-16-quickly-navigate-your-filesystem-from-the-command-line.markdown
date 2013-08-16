---
layout: post
title:  "Quickly navigate your filesystem from the command-line"
date:   2013-08-16 19:06:19
---

Like many others, I spend most of my day behind a computer.
In order make the most of it (and to keep my body from complaining too much), I try to maintain an optimized setup.
For example, I code in [Vim][vim], browse with [Vimperator][vimperator], and move windows around in [i3][i3].
Another common task is filesystem navigation. 
I prefer to use the command-line for this, but typing `cd ~/some/very/deep/often-used/directory` does become cumbersome.

Automated tools like [autojump][autojump] and [fasd][fasd] address this problem by offering shortcuts to the directories you often go.
Personally, I prefer a more manual solution, which I would like to share with you.
I have noticed quite an increase in efficiency with this, and perhaps you will too.

Under the hood this manual solution comes down to storing symbolic links in a hidden directory (e.g., `~/.marks`).
There are four shell functions `jump`, `mark`, `unmark`, and `marks`, and they look like this:

```bash
export MARKPATH=$HOME/.marks
function jump { 
	cd -P $MARKPATH/$1 2>/dev/null || echo "No such mark: $1"
}
function mark { 
	mkdir -p $MARKPATH; ln -s $(pwd) $MARKPATH/$1
}
function unmark { 
	rm -i $MARKPATH/$1 
}
function marks {
	ls -l $MARKPATH | sed 's/  / /g' | cut -d' ' -f9- | sed 's/ -/\t-/g' && echo
}
```

Put this in your `.zshrc` or `.bashrc` and you're ready to jump. (I have also turned this into a plugin for [oh-my-zsh][ohmyzsh] called `jump`, but that's currently in a [pull-request][pullrequest].) To add a new bookmark, `cd` into the directory and `mark` it with a name to your liking:

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
