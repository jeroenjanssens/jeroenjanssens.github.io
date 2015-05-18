---
layout: post
title: "Something old, something new, something borrowed, some Unix glue"
date: 2015-04-01 10:00:00
description: ""
---

The Unix command line is over 40 years old.  The underlying concepts and its interface have stood the test of time.  Even now, in the age of big data, cloud computing, and the internet of things, this technology remains to be highly relevant.  The main reason?  The vast amount of command-line tools that have been--and continue to be--developed.

This post aims to highlight a few of those command-line tools. Indeed, some are old, some are new, and some borrow technology 

I argue, that by combining such tools, that we can face 



 REPL




The concept has 

1. REPL lends itself well for scrubbing and exploring data.
2. It's everywhere from big servers that run in the cloud, to your laptop, to IoT.
3. It plays nicely with other technologies.
4. It's open source.
5. Tools, many open source which adhere to the Unix philosophy, 

(In this post I'd like to focus on the tools.)
In this post, I'll discuss some old tools, some new tools, and some tools that borrow from other 

I'm not saying you should get married with the command line, as the title may imply, 

Four ingredients


Unix command line. 

intro


, but I believe that having some basic skills will make you more

After all these year, Still relevant

The command line is nothing without tools (binaries, interpreted scripts, shell scripts).

## Something old

But let's start with the classics.
sed, awk, grep, cut, sort, uniq, 
These, and many other tools, are described in no less than 1000 pages, in Unix Power Tools (Don't make the same mistake I did and order the e-book version).


## Something new

Thanks to many voluntueers, the command line is able to keep up.
jq, to process JSON data, highly relevant in the age of APIs.
aws, gcloud, azure-cli, manage your entire cloud AWS infrastructure.
parallel, to harness the computing power of multiple cores and even machines.



## Something borrowed

sql (csvsql --query)
littler and Rio allow you to leverage R, plotting ggplot
vagrant (virtual machines)



## Some Unix glue

But all these tools are 

The real power of the command line is that you can glue these tools together.
Pipes
The tools treat text as the universal interface (an important aspect of the Unix philosophy <link>)






