---
layout: post
title:  "Lean, mean data science machine"
date: 2013-12-07 9:00:00
description: "I am sharing a virtual environment that enables you to follow along with all the commands and tools discussed in my upcoming book Data Science at the Command Line."
image: /img/vagrant.png
---

Data scientists love to create interesting models and exciting data visualizations. However, before they get to that point, usually much effort goes into obtaining, scrubbing, and exploring the required data. I argue that the \*nix command-line, although invented decades ago, remains a powerful environment for processing data. It provides a read-eval-print loop (REPL) that is often much more convenient for exploratory data analysis than the edit-compile-run-debug cycle associated with large programs and even scripts.

Unfortunately, setting up a workable environment and installing the latest command-line tools can be quite a pain. This post describes how to alleviate that pain and how to get you started doing data science on the command-line in a matter minutes.


### Data Science at the Command Line

I am currently authoring a book titled "Data Science at the Command Line", which will be published by O'Reilly in summer 2014.
The main goal of the book is to teach why, how, and when the command-line could be employed for data science. The tentative outline is as follows:

1. Introduction
2. Getting Started
3. Step 1: Obtaining Data
4. Creating Reusable Command-line Tools
5. Step 2: Scrubbing Data
6. Managing Your Data Workflow
7. Step 3: Exploring Data
8. Speeding Up Data-Intensive Commands
9. Step 4: Modeling Data
10. Poor Man's MapReduce
11. Step 5: Interpreting Data
12. Conclusion

Naturally, the book will be drenched with commands and source code. It is important that the text, the code, and the output of the code are consistent with each other. Manually running the code and copy-pasting the output is a cumbersome and error-prone process. 
To automate this process, I have created a script (a [dexy](http://www.dexy.it/) filter to be precise) that will (1) extract all the source code from the text, (2) run these in an isolated environment, and (3) paste the output back into the text. From here the O'Reilly toolchain takes over and converts the text to a variety of digital formats. Very smooth.


### Vagrant environment

The isolated environment is created and configured using [Vagrant](http://www.vagrantup.com/), which is basically a wrapper around VirtualBox and other virtualization software such AWS EC2. With a few commands, a fresh virtual machine is spun up and configured according to a simple script. It was [Matthew Russell's Ignite talk](http://miningthesocialweb.com/2013/11/23/confessions-of-a-prolific-moonlighter-with-a-chronic-writing-disorder) that inspired me to use Vagrant; he provides one for his book [Mining the Social Web](http://miningthesocialweb.com) that is focused more on Python.
If my Vagrant environment would be provided with Data Science at the Command Line, then the reader would be able to follow along with the commands and source code. But since my mission is to enable everybody to do data science at the command-line as soon as possible, I have decided to make it available right now.

Currently, the environment includes the [seven command-line tools I discussed](http://jeroenjanssens.com/2013/09/19/seven-command-line-tools-for-data-science.html) a while ago and [GNU parallel](http://www.gnu.org/software/parallel/), which will be discussed in Chapter 8. Just like the book itself, the environment is a work in progress. In order to be able to run [Rio](https://github.com/jeroenjanssens/data-science-toolbox/blob/master/tools/Rio) (one of the seven tools), I had to include the latest version of `R`, together with the packages `ggplot2`, `sqldf`, and `plyr`. I am aware that many of you would prefer the Python scientific stack to be included as well. However, because of disk-space and provision-time constraints, I doubt whether it is desirable (or even possible) to create an environment that includes everything. Perhaps that we can devise a solution where you select which tools, packages, and languages you would like to have installed. As mentioned, it is a work in progress and my main goal is to get you up and running on the command-line.


### Installation

The environment is currently configured to run on top of [VirtualBox](https://www.virtualbox.org). (I am looking into the option to deploy it on an AWS EC2 instance.)
So, first you will need to install [VirtualBox](https://www.virtualbox.org). 
Second you need to install [Vagrant](http://www.vagrantup.com/). 
Third, you need to download the environment by cloning the data science toolbox. (If you do not want to use `git` you can also [download the zip file](https://github.com/jeroenjanssens/data-science-toolbox/archive/master.zip).)

```bash
git clone https://github.com/jeroenjanssens/data-science-toolbox.git
cd data-science-toolbox/box
```

Running `vagrant up` in the `box` directory will download the base box (Ubuntu 12.04 LTS 64-bit), spin up a virtual machine, and provision it. (Now would be the perfect time to think about any command-line scripts you may have lying around and donate them to the [data science toolbox](http://datasciencetoolbox.org).) Once the provisioning is complete, you will be able to log into your own lean, mean data science machine: 

```
vagrant ssh
```

Run the following command to test whether everything has been installed correctly:

```bash
curl -s 'http://en.wikipedia.org/wiki/List_of_countries_and_territories_by_border/area_ratio' | scrape -be 'table.wikitable > tr:not(:first-child)' | xml2json | jq -c '.html.body.tr[] | {country: .td[1][], border: .td[2][], surface: .td[3][], ratio: .td[4][]}' | json2csv -p -k=country,ratio | Rio -se'sqldf("select * from df where ratio > 0.3 order by ratio desc")' | csvlook
```
```
|----------------+------------|
|  country       | ratio      |
|----------------+------------|
|  Vatican City  | 7.2727273  |
|  Monaco        | 2.2        |
|  San Marino    | 0.6393443  |
|  Liechtenstein | 0.475      |
|----------------+------------|
```

The virtual machine is not entirely isolated. Files that you put in the `box` directory will be accessible from the `/vagrant` directory in the virtual machine. This allows you to use both the tools you already have installed and the command-line tools provided by the environment. If you want to install any of these tools on your own machine, then you can run the relevant commands from the [provisioning script](https://github.com/jeroenjanssens/data-science-toolbox/blob/master/box/bootstrap.sh). 


### Conclusion

While the command-line is a very powerful environment to process data, manually installing the latest command-line tools is not straightforward. Vagrant allows you to spin up a virtual machine and to install all the tools automatically.
In this post I have shared with you the exact same Vagrant environment as that I am using for my upcoming book, in the hope that it will be useful to get you started with doing data science at the command line.
Please let me know if you have any questions, suggestions, or contributions.

Best wishes,

Jeroen <br>
[@jeroenhjanssens][twitter]

[twitter]: https://twitter.com/jeroenhjanssens/
