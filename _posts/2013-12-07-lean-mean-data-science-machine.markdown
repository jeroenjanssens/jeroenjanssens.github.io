---
layout: post
title:  "Lean, mean data science machine"
date: 2013-12-07 9:00:00
description: "I am sharing a virtual environment that enables you to follow along with all the commands and tools discussed in my upcoming book Data Science at the Command Line."
image: /img/vagrant.png
---

*Update (1-4-2014) Be sure to check out the brand new [Data Science Toolbox](http://www.datasciencetoolbox.org)!*

*Update (9-12-2013) I have compared my Vagrant environment with three other virtual environments for data science ([see below](#comparison-of-virtual-environments-for-data-science)).*

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

### Your own Data Science Toolbox environment with Vagrant

The environment is created and configured using [Vagrant](http://www.vagrantup.com/), which is basically a wrapper around VirtualBox and other virtualization software such AWS EC2. With a few commands, a fresh virtual machine is spun up and configured according to a simple script. It was [Matthew Russell's Ignite talk](http://miningthesocialweb.com/2013/11/23/confessions-of-a-prolific-moonlighter-with-a-chronic-writing-disorder) that inspired me to use Vagrant; he provides one for his book [Mining the Social Web](http://miningthesocialweb.com) that is focused more on Python.
If my Vagrant environment would be provided with Data Science at the Command Line, then the reader would be able to follow along with the commands and source code. But since my mission is to enable everybody to do data science at the command-line as soon as possible, I have decided to make it available right now.

Currently, the environment includes the [seven command-line tools I discussed](http://jeroenjanssens.com/2013/09/19/seven-command-line-tools-for-data-science.html) a while ago and [GNU parallel](http://www.gnu.org/software/parallel/), which will be discussed in Chapter 8. Just like the book itself, the environment is a work in progress. In order to be able to run [Rio](https://github.com/jeroenjanssens/data-science-toolbox/blob/master/tools/Rio) (one of the seven tools), I had to include the latest version of `R`, together with the packages `ggplot2`, `sqldf`, and `plyr`. <del>I am aware that many of you would prefer the Python scientific stack to be included as well.</del> The Python scientific stack (`ipython`, `numpy`, `scipy`, `matplotlib`, `pandas`, and `scikit-learn`) is also included. However, because of disk-space and provision-time constraints, I doubt whether it is desirable (or even possible) to create an environment that includes everything. Perhaps that we can devise a solution where you select which tools, packages, and languages you would like to have installed. As mentioned, it is a work in progress and my main goal is to get you up and running on the command-line.


### Installing the Data Science Toolbox environment

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

### <a name="comparison-of-virtual-environments-for-data-science"></a>Comparison of virtual environments for data science

Of course the Data Science Toolbox environment is not the only one available for doing data science! 
So far, I have been able to perform a rudimentary comparison with three other solutions.
(Please let me know if you know any others.)

**1. Data Science Toolbox (DST)** <br />
Created by: [Jeroen Janssens][twitter] <br />
Github: [jeroenjanssens/data-science-toolbox](https://github.com/jeroenjanssens/data-science-toolbox) <br />
Installs R, the Python scientific stack, and of course many command-line tools for processing data. Uses Vagrant and for now it can be deployed on VirtualBox, only.


**2. Mining the Social Web (MTSW)** <br />
Created by: [Matthew Russel](https://twitter.com/ptwobrussell) <br />
Website: [miningthesocialweb.com/](http://miningthesocialweb.com/) <br />
Github: [ptwobrussell/Mining-the-Social-Web-2nd-Edition](https://github.com/ptwobrussell/Mining-the-Social-Web-2nd-Edition) <br />
Uses Vagrant (with Chef as the provisioner, which is really nice) and can be deployed on both VirtualBox and AWS.
Installs IPython Notebook, numpy, mongo, and NLTK, which allows you to follow along with the examples provided in the book. An AWS AMI is available as well.


**3. Data Science Toolkit (DSTK)** <br />
Created by: [Pete Warden](https://twitter.com/petewarden) <br />
Website: [www.datasciencetoolkit.org](http://www.datasciencetoolkit.org) <br />
Github: [petewarden/dstk](https://github.com/petewarden/dstk)  <br />
The website provides a sandbox from which you can try out many interesting APIs. These APIs can also be accessed from the command line. An AWS AMI is available.


**4. Data Science Box (DSB)** <br />
Created by: [Drew Conway](https://twitter.com/drewconway) <br />
Github: [drewconway/data\_science\_box](https://github.com/drewconway/data_science_box) <br />
This is a bash script for which you need have an AWS EC2 instance running.
It installs R, Shiny, IPython Notebook, and the Python scientific stack.

For your convenience I have summarized this information in the following table.

<table>
	<thead>
		<tr>
			<th />
			<th>Configuration</th>
			<th>VirtualBox</th>
			<th>AWS</th>
			<th>AMI</th>
			<th>Python</th>
			<th>R</th>
			<th>Shiny</th>
			<th>Comments</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th>1. DST</th>
			<td>Vagrant</td>
			<td class="yes">Yes</td>
			<td class="no">No</td>
			<td class="no">No</td>
			<td class="yes">Yes</td>
			<td class="yes">Yes</td>
			<td class="no">No</td>
			<td>Includes the Data Science Toolbox</td>
		</tr>
		<tr>
			<th>2. MTSW</th>
			<td>Vagrant</td>
			<td class="yes">Yes</td>
			<td class="yes">Yes</td>
			<td class="yes">Yes</td>
			<td class="yes">Yes</td>
			<td class="no">No</td>
			<td class="no">No</td>
			<td></td>
		</tr>
		<tr>
			<th>3. DSTK</th>
			<td>Vagrant</td>
			<td class="yes">Yes</td>
			<td class="yes">Yes</td>
			<td class="yes">Yes</td>
			<td class="no">No</td>
			<td class="no">No</td>
			<td class="no">No</td>
			<td>Includes various command-line tools</td>
		</tr>
		<tr>
			<th>4. DSB</th>
			<td>Bash</td>
			<td class="no">No</td>
			<td class="yes">Yes</td>
			<td class="no">No</td>
			<td class="yes">Yes</td>
			<td class="yes">Yes</td>
			<td class="yes">Yes</td>
			<td></td>
		</tr>
	</tbody>
</table>

In short, I think that they all have some strong aspects. Some of these may be improved over time (I am currently looking into using Chef as the provisioner), new environments may arise; that is the way open source works. In the end, it is up to you to decide which one works best for you. And if you want to make some tweaks, you can always fork the appropriate Github repository.

It is in general just amazing to be able to spin up a new virtual machine with your own or somebody else's environment, whether by running `vagrant up` or by clicking a few buttons on AWS.

I realize that three out of four names look really alike, which can be confusing, but it could also indicate that there is a need for having an automated (and isolated) setup to start doing data science without any additional hassle.

### Conclusion

While the command-line is a very powerful environment to process data, manually installing the latest command-line tools is not straightforward. Vagrant allows you to spin up a virtual machine and to install all the tools automatically.
In this post I have shared with you the exact same Vagrant environment as that I am using for my upcoming book, in the hope that it will be useful to get you started with doing data science at the command line. I have also compared my environment with three other virtual environments for data science.
Please let me know if you have any questions, suggestions, or contributions.

Best wishes,

Jeroen <br>
[@jeroenhjanssens][twitter]

[twitter]: https://twitter.com/jeroenhjanssens/
