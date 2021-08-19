# Yuan Independent Study Summer 2021

This is a report repository to record the procedure in my summer independent research, giving some information about the project I'm developing on as well. Below are the original contents of the report, and the guidance is filled up by my instructor.

--------

The work will explore adapting Visual Studio Code and the Haskell Language Server to understand how learners use type error messages.

# Expectations

This is a 3 semester hour independent study course running from June 14 to Aug 5.  According to [UI's credit hour definition](https://clas.uiowa.edu/faculty/credit-hour-definition), this means it should require about 10-15 hours of work per week.

By the Monday following each week, Guanda should provide a brief description of his progress in this Git repository, as linked below.

* [June 14--June 20](weekly/week1.md)
* [June 21--June 27](weekly/week2.md)
* [June 28--July 4](weekly/week3.md)
* [July 5--July 11](weekly/week4.md)
* [July 12--July 16](weekly/week5.md)
* [July 19--July 25](weekly/week6.md)
* [July 26--August 1](weekly/week7.md)
* [August 1--August 4](weekly/week8.md)

Garrett will provide feedback and suggestions in response to Guanda's reports.  We can also arrange a weekly Teams meeting if it seems helpful.

Successful completion of the independent study class will mean having developed skills with Haskell and some of its tooling, and making reasonable weekly progress.  It is not  matter of completing all of the tasks we identify---as a research project, there's always more work to do, and it's hard to say in advance exactly how long each step will take.

# Tasks

Here is an initial description of the tasks Guanda will undertake.  As this is essentially research, we will adjust and adapt the tasks as the summer progresses.

## Building HLS in a VS Code Remote Container

Building the Haskell Language Server requires several dependencies, and has additional irritating requirements on Windows.  VS Code supports [developing inside a container](https://code.visualstudio.com/docs/remote/containers), allowing us to standardize the build environment and assure that it works on all platforms.  Guanda's first task is to extend a [fork of the HLS](https://github.com/IaFP/haskell-language-server) with the suitable files (i.e., an `.devcontainer` directory and its contents) to support building and using it in a container.

* Some relevant documentation is here:
  - [VS Code: developing inside a container](https://code.visualstudio.com/docs/remote/containers)
  - [VS Code: remote development in containers](https://code.visualstudio.com/docs/remote/containers-tutorial)
  You will probably want to find additional documentation on Docker files and best practices.

* You may find it helpful to look at existing Haskell Dockerfile examples.

To keep things simple, let's use:

* The [`containerized`](https://github.com/IaFP/haskell-language-server/tree/containerized) branch within our fork of HLS.
* GHC version 8.10.5---HLS has the beginnings of support for GHC 9, but let's not complicate things yet.

## Using our modified HLS

You've mostly anticipated what I was going to write here, in your week 2 update.

## Collecting type errors

The next task is to start building HLS plugins.  My long-term goal is to be able to study how people use type error messages (and hopefully to improve the way errors are reported), but to begin, we need to find out what errors people are getting.  As a first step, you could try to write a plugin that captures and logs all type errors as they go by.  A couple of points:

1.	There is a tutorial on plugin writing [here](https://github.com/pepeiborra/hls-tutorial).  [Here are some notes](<Johnson HLS Notes 6-28.docx>) that another student made on writing plugins with current HLS.
2.	I’m not sure what the model is for HLS plugins writing to disk, and so forth.  For the time being, where and how things are logged is not really the important point.
3.	You’ll likely run into some unfamiliar Haskell constructs and libraries as you get into plugin programming.  That’s fine!  Take some time to learn about these things, and let me know if you get confusing or lost.


## Identify code changes relevant to type errors

## Reporting code changes