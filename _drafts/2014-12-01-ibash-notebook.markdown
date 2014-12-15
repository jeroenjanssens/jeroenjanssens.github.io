---
layout: post
title: "IBash Notebook"
date: 2014-12-10 10:00:00
description: "The Bash kernel for IPython Notebook now displays inline images. Could it ever be a convenient environment for doing data science? Would it be worthwhile to distribute my book Data Science at the Command Line as a series of notebooks?"
image: /img/twitter-ibash_notebook.png
twitter_card: summary_large_image
---

The Bash kernel for IPython Notebook can now display inline images. Could it ever be a convenient environment for doing data science? Would it be worthwhile to distribute my book [Data Science at the Command Line][book] as a series of notebooks?

![](/img/ibash-notebook.png)


### Notebooks for everyone

Doing research is hard. Remembering which steps you've taken, and why, is even harder. In order to be an effective researcher, it's recommended you [keep a laboratory notebook](http://colinpurrington.com/tips/academic/labnotebooks). Besides having a record of your steps and results, this also allows you to improve reproducibility, share your research with others, and, yes, think more clearly. So, why wouldn't you keep a notebook?

Well, if you perform your research or analysis on a computer, where most steps boil down to running code, invoking commands, and clicking icons and buttons, keeping an analogue notebook is rather cumbersome. Fortunately, since recently, digital counterparts are quickly gaining popularity. For the R community, for example, there's [R Markdown](http://rmarkdown.rstudio.com). And for those who use the Python scientific stack, there's [IPython Notebook](http://ipython.org/notebook.html). Both solutions allow you to combine code, text, equations, and visualizations into a single document, all for free.

The people behind the IPython project saw the potential of a language-agnostic architecture. By creating a flexible messaging protocol, writing good [documentation][kerneldoc] for it, and rebranding the project as the [Jupyter project][jupyter], they opened the door to other languages. And now, languages like Julia, Ruby, and Haskell have their own so-called kernel.


### What about Bash?

To demonstrate how easy it is to create a kernel, [Thomas Kluyver][takluyver] created a Python package called [bash_kernel][repo]. This Bash kernel basically works by employing [pexpect](https://pexpect.readthedocs.org/en/latest/) that wraps around a Bash command line.
When I stumbled upon this package I immediately got excited. I believed that it could be much more than just a demonstration. With some effort, we could have an "IBash Notebook" that would have some important advantages over a terminal, which is the classical environment for the command line.

First, the command line is ad-hoc in nature, which makes it difficult to reproduce your steps or share them with your peers. You can put those steps in a shell script, [Makefile](http://www.gnu.org/software/make/), or [Drakefile](https://github.com/Factual/drake) to improve reproducibility, but when you're working in a notebook, they would be stored automatically.

Second, if you're running a server or virtual machine, there would be no need to *ssh* into it. As a result, Microsoft Windows users wouldn't need to install [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/) anymore. I'm particularly interesting in this advantage, together with the next two, because ever since I started writing [Data Science at the Command Line][book], I've been looking for ways to make the command line as accessible as possible.

Third, for users who are new to the command line, a notebook with code cells could be less intimidating than a terminal with a prompt. Because the browser (and perhaps already the notebook) is a familiar environment, the threshold to try out the command line will be lower.

Fourth, in order to view an image located on a server or virtual machine, you normally have to go trough an extra hoop. Approaches that I know are: (1) copy this image to localhost, (2) forward X11, or (3) serve it using, say, <code>python -m SimpleHTTPServer</code> and then view it with a browser. In a notebook, images can be shown inline (I'll discuss this in the next section). With a notebook, images can be shown inline. Speaking of which...


### Adding inline images

The Bash kernel didn't feel complete yet. 

Having inline Markdown is already possible because that's part of the front end. 


Being inspired by that one (which I used in my tutorial at Strata, 100 EC2 instances) and Pigshell

I figured that the biggest win would the ability to show inline images.




To add the ability to show inline images, I first tried to detect the MIME type of the (binary) output of a command, so that you could simply run `cat file.png`. This didn't work because I learned that `pexpect` isn't meant to transfer binary data. With some suggestions from Thomas Kluyver, I came up with the following solution instead. (You may decide whether it's a hack or not.)

The solution involves registering a Bash function called `display` when the kernel starts. Now, images can be displayed by running something as simple as:

```bash
display < file.png
```

or even something as ugly as:

```bash
cat iris.csv |                        # Read our beloved Iris data set
cols -C species body tapkee -m pca |  # Apply PCA using tapkee
header -r x,y,species |               # Replace header of CSV
Rio-scatter x y species | display     # Create scatter plot using ggplot2
```

which produces:

![](/img/iris-pca.png)

Note that `cols` and `body` are used to only pass numerical values to [tapkee][tapkee], which is a fantastic library for dimensionality reduction by Sergey Lisitsyn. In case you're interested, these two Bash scripts, together with `header` and `Rio-scatter`, can be found in [this repository](https://github.com/jeroenjanssens/data-science-at-the-command-line/tree/master/tools). To see what the `display` function looks like, we can run `type display` in a notebook (when the Bash kernel is active):

```bash
display is a function
display ()
{ 
    TMPFILE=$(mktemp ${TMPDIR-/tmp}/bash_kernel.XXXXXXXXXX);
    cat > $TMPFILE;
    echo "bash_kernel: saved image data to: $TMPFILE" 1>&2
}
```

In words, `display` saves the standard input to a temporary file and prints the filename to standard error. After a code cell has been evaluated, the Bash kernel simply extracts the filename (if any) from the output, detects its MIME type using the [imghdr](https://docs.python.org/3.4/library/imghdr.html) library, and sends the image data (encoded with base64) to the front end. 

I chose the name "display" because there's also a [command-line tool in ImageMagick][display] called "display" that accepts image data from standard input and shows it in a popup. Because that tool doesn't work without X, I figured that a function called "display" could serve as a drop-in replacement when using IPython notebook.


### A book as a series of notebooks


Matthew Russell has done it with his book Mining the Social Web. Oreilly.

Many other examples.



The main advantage of a notebook as opposed to an ebook is that you can execute the code yourself.


I wonder if I could now do the same.

I wonder whether IBash Notebook would be a suitable environment for doing data science at the command line. 
As a first test, I manually converted part of the first chapter of my book [Data Science at the Command Line][book] to a notebook, which you can [view on nbviewer][notebook].



disadvantages to Bash kernel, currently:

- no real time output, long running processes
- scrolling  (less, man)
- no interactivity


A challenge will be 
Package as docker container.

### What's next?


If you want to try this out yourself, you should first install [IPython 3](https://github.com/ipython/ipython). Then, clone the [GitHub repository][repo] and install the package.
What do you think?
Any news regarding this will be posted to [Twitter][twitter].




[display]: http://www.imagemagick.org/script/display.php
[otherbooks]: https://github.com/ipython/ipython/wiki/A-gallery-of-interesting-IPython-Notebooks#entire-books-or-other-large-collections-of-notebooks-on-a-topic
[jupyter]: http://jupyter.org/
[twitter]: https://twitter.com/jeroenhjanssens/
[takluyver]: https://twitter.com/takluyver/
[oreilly]: http://shop.oreilly.com/product/0636920032823.do?cmp=af-strata-books-videos-product_cj_9781491947852_%25zp
[book]: http://datascienceatthecommandline.com
[repo]: https://github.com/takluyver/bash_kernel
[notebook]: http://nbviewer.ipython.org/github/jeroenjanssens/jeroenjanssens.github.io/blob/master/Data%20Science%20at%20the%20Command%20Line%20-%20When%20is%20Fashion%20Week%20in%20New%20York%3F.ipynb
[kerneldoc]: http://ipython.org/ipython-doc/dev/development/kernels.html
[tapkee]: http://tapkee.lisitsyn.me/
