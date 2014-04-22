--- 
layout: post
title: Using Gitbook with R Markdown
tags: R R-Bloggers
type: post
published: true
status: publish
---
 
<b>
UPDATE: This R Script has been deprecated and all the functionality has been included in the Rgitbook Package. More information is at http://jason.bryer.org/Rgitbook
</b>

[Gitbook](http://www.gitbook.io/) has been getting some (deserved) attention. For those who haven't seen it, Gitbook is a system to create really beautiful interactive web (or PDF and ebook) books. For me, the timing of discovering this framework could not be better as I am preparing documentation for propensity score analysis for an [upcoming workshop I am giving](http://www.meetup.com/Albany-R-Users-Group/events/176617442/). Of course, I want to use it to write about R and would prefer to use R markdown instead of just plain markdown. I have written a number of R functions to combine the functionality of `knitr` and Gitbook. The source code is on [Gist](https://gist.github.com/jbryer/11049319) or can be sourced directly using the `devtools` package.

	> devtools::source_gist(11049319)

The first step is to create a new Gitbook. The `newGitbook` will create a new directory and create four files.

* `.bookignore` - This file is formatted like any `.gitignore` file, but is specifically for Gitbook. That is, list files here that you want Gitbook to ignore, but want Git to manage. This defaults to include `.Rmd`, `.md`, `.R`, and `.Rproj` files as well as the `log` directory (this will be discussed below).
* `.gitignore` - This defaults to include the `_book` directory. This is the default location for the built book and it is likely you do not want to include it in your master branch. As we will see below, this directory will be published to the `gh-pages` branch.
* `README.md` - Introduction to your Gitbook.
* `SUMMARY.md` - Defines the structure (i.e. table of contents) of your Gitbook.

You can get more information about how Gitbook is organized at their webiste [gitbook.io](http://gitbook.io).

	> newGitbook('~/Dropbox/Projects/testbook')
	Creating /Users/jbryer/Dropbox/Projects/testbook
	Writing .bookignore...
	Writing .gitignore...
	Writing README.md...
	Writing SUMMARY.md...
	You can now open README.md and SUMMARY.md. Once you are done 
	editting SUMMARY.md, initGitbook() will create the file and folder 
	structure for your new Gitbook.
	
I should note that the `newGitbook` will change the working directory to the location of your new Gitbook.

	> getwd()
	[1] "/Users/jbryer/Dropbox/Projects/testbook"

And we can see the four files it created.

	Jasons-MacBook-Air:testbook jbryer$ ls -la
	total 32
	drwxr-xr-x    6 jbryer  staff   204 Apr 18 10:52 .
	drwxr-xr-x  110 jbryer  staff  3740 Apr 18 10:52 ..
	-rw-r--r--    1 jbryer  staff    35 Apr 18 10:52 .bookignore
	-rw-r--r--    1 jbryer  staff    49 Apr 18 10:52 .gitignore
	-rw-r--r--    1 jbryer  staff    75 Apr 18 10:52 README.md
	-rw-r--r--    1 jbryer  staff   231 Apr 18 10:52 SUMMARY.md

At this stage you can open `SUMMARY.md` and change the outline of your book. Once you are done, the `initGitbook` function will create the files and folders for your book. Unlike the Gitbook command line, this function will change the file extensions of all your files to `.Rmd` (excluding `README.md` and `SUMMARY.md`) even though you specify `.md` in the links in that file. The `buildRmd` function discussed below will convert those `.Rmd` files to `.md`.

	> initGitbook()

![Files](/images/gitbookfiles.png)

The `buildRmd` function will convert all `.Rmd` files in your project to `.md` using the `knitr` package. It should be noted that this function will create a file, `.rmdbuild.Rda`, in your working directory. This is an R data file that saves the status of the last build. This allows this function to only build R markdown files that have changed since the last build and therefore, increase the execution time. By default, all the `knitr` messages will be printed to the console. If you specify `log.dir` parameter, then all the output will be saved to log files in that given directory (one log file per Rmd file). There is also a `clean` parameter that will build all R markdown files regardless of their modification timestamp.

	> buildRmd()
	
The `buildGitbook` function is simply a wrapper to the Gitbook command line and will generate your book from the markdown sources. As of this writing, there is a [bug](https://github.com/GitbookIO/gitbook/issues/99) in Gitbook where the image URLS are incorrect. This function will fix the URLs. There is also another [issue](https://github.com/GitbookIO/gitbook/issues/113) that links to the Introduction point to `/` and not `/index.html`. These will also be fixed.

	> buildGitbook()
	
The `openGitbook` will open your built book using your system's default web browser.

	> openGitbook()
	
Lastly, the `publishGitbook` will publish your built Gitbook to the `gh-pages` branch of the specified Github repository. Special thanks to [Ramnath Vaidyanathan](https://github.com/ramnathv) who provided the [shell script](https://github.com/GitbookIO/gitbook/issues/106#issuecomment-40747887) to do this. Take a look at the link as you can optionally save this as a Git hook to publish atomically after checking in code to the master branch.

	> publishGitbook(repo='jbryer/testbook')
	
Please leave comments below or suggestions to make this better. I would also be interested if there is interest in rolling this up into an R package.

-------------------------

Here is the source code:

<script src="https://gist.github.com/jbryer/11049319.js"></script>

