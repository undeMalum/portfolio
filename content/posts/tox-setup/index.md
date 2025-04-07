+++
title = "Tox. Platform Dependent Setup (GHOSTxIRIM)"
date = "2025-03-31T08:51:16+02:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "Mateusz Konat"
authorTwitter = "" #do not include @
cover = "images/cover.jpg"
tags = ["creativity", "coding"]
keywords = ["creativity", "coding"]
description = "For the blog’s revival, I’m tackling a small but intriguing Tox-related issue I faced in the GHOSTxIRIM project. Let’s dive in!"
showFullContent = false
readingTime = false
hideComments = false
draft = true
+++

# Long time no see  
Hello, everyone! It’s been a while since I last shared my thoughts, but I’m thrilled to be back in my _"somewhere I belong"!_  

In this post, I’ll discuss an issue I encountered while setting up the `Tox` environment for the test pipeline in the `GHOSTxIRIM` project. It seems trivial _now_, but I spent far too long trying to resolve it—something every programmer can probably relate to.  

Anyway, I also owe you a quick explanation of some of the terms I just used, as they haven’t appeared on this blog before.  

So, let’s start with a brief update, then dive right into the good stuff.  

Let’s go!

# What's what  
Before diving into the tools, I think it’s worth introducing the project first.  

## GHOST  
**[`GHOSTxIRIM`](https://github.com/GHOST-Science-Club/tree-classification-irim)** is a student-led project focused on classifying tree species from aerial photos (we use Python btw). It operates as a section of **[`GHOST`](https://ghost.put.poznan.pl)**, a student organization dedicated to machine learning in a broader sense.  

_(Find out more in the links above.)_  

On a personal note, I joined GHOST—specifically GHOSTxIRIM—during my first semester at [PUT](https://put.poznan.pl/en) and have been enjoying it ever since (I’m now in my second semester of freshman year). I considered joining other GHOST sections, but time constraints didn’t allow it. That’s a story for another time.  

This high-level overview should suffice for now. Let’s move on to **Tox**.  

## Tox  
**[`Tox`](https://tox.wiki/en/4.25.0/)** is a virtual environment management tool **crucial** to our test pipeline. It allows us to create isolated environments from scratch, enabling testing across multiple Python versions and operating systems.  

Testing across different OSes is particularly important since team members work on different platforms. For instance, I use Windows, while our project leader works on a Linux distribution (Debian, I believe—but I wouldn’t bet my life on it).  

The need for cross-platform testing becomes even **more apparent** given that we maintain two separate Python requirements files—one for Linux/macOS ([unix-requirements.txt](https://github.com/GHOST-Science-Club/tree-classification-irim/blob/main/unix-requirements.txt)) and another for Windows ([requirements.txt](https://github.com/GHOST-Science-Club/tree-classification-irim/blob/main/requirements.txt)). Unfortunately, while this is the most straightforward and cleanest approach to cross-platform compatibility, it comes with a downside.  

This method **requires** creating test environments with different configuration files depending on the platform. And that’s where the core issue lies:

> **How can we automatically select the correct requirements file for testing based on the platform?**  

Let’s explore why this turned out to be so troublesome.

# Problem-solving
Problem-solving involved a couple of pretty well laid-out stages (_and I'm not referring to momentarily lapses of faith_), so I'll split the following section accordingly.

## Conditional expression
Being used to Python, I initialy thought about using some conditional expression, something like an if statement to detect the OS that is being run and choose the appropriate requirements file. 

So I searched more or less this: _[conditional setting for tox](https://www.google.com/search?q=conditional+setting+for+tox)_.

This search didn't prove useful though. And after a couple of unsuccessful trial-and-error attempts at paraphrasing this inquiry, I started added new information.

At some point, I added a part about detecting platform to the previous inquiry. It looked like this: _[conditional setting for tox detecting platform](https://www.google.com/search?q=conditional+setting+for+tox+detecting+platform)_. To my satisfaction, it yielded a promising Stack Overflow post: **[How to conditionally set tox variables depending on the platform](https://stackoverflow.com/questions/61510250/how-to-conditionally-set-tox-variables-depending-on-the-platform)**.

It did **not** resemble any conditional statement because it utilized tox environment for platform-targeted configuration. However, it failed to solve all problems, initially.

## Tox environments
This Stack Overflow post linked to a page from Tox documentation titled: **[Platform specification](https://tox.wiki/en/latest/faq.html#platform-specification)**.

This page presented a way to create a couple python version + os for environment initialization like so:

```ini
[tox]
envlist = py{310,311}-{lin,win}
```
Then, you can specify what requirements file should be used for which OS under test environment:

```ini
[testenv]
platform = lin: linux
           win: win32

deps =
    linux: -r {toxinidir}/unix-requirements.txt
    windows: -r {toxinidir}/requirements.txt
```

This seemes marvellous because it gave me a solution to the very problem I aimed to solve, i.e. choose reuiqrements files based on OS. The only downside was that I had to use the `tox.ini` file instead of packaging it inside `pyproject.toml` as the above syntax is not supported by the `toml` format. However, multiple configuration files is a general problem of all Python projects so to avoid it we would have to not use Python.

What proved to be more problematic though and caused me enourmous anxiety after a glimpse of sweet relief was running Tox in GitHub actions, which promted me so switch me seek a kind of solution that I wanted to erase from my memory.

## Duplicate config files
To be more specific about what I've just complained about, the above-mnetioned fix worked perfectly locally. If you run `tox` command in your terminal, you'll get what you asked for-testing across multiple python versions and OSes.

However, it didn't go so well once you uploaded your code to the remote repository.

I set a trigger in GitHub actions that, on pull request, test would be run automatically (along with a linter and type checker). To do so, I used a to plugin called [`tox-gh-actions`](https://github.com/ymyzk/tox-gh-actions) because Tox doesn't support GitHub actions out-of-the-box. And that's what ripped my excitation to shreds.

Because GitHub actions creates the OS environment independently of Tox, running Tox in this environment caused a bizzare situation where Tox wanted to create both Linux and Windows environments in the already-established Linux or Windows machine. Not to mention the mess created by installation of wrong requirements files.

I thought I'm gonna explode. But I, _being an adult programmer_, decided to face my problems manly and go cry to bed. After a short episode of faith loss I proceeded to looking for different solutions. Amittedly, I did have solution, but I tried to reject it from my mind in seek of something better.

The solution that I so desperately wanted to avoid was creating to copies of Tox config files-one for each platform-and then choosing an appropriate one depending on the platform using the GitHub actiong syntax for if statement.





And it turned out that you can customize instllation in Tox.

I though it's gonna be okay since [installaztion customization](https://tox.wiki/en/latest/config.html#install_command) can be done using an external scripts that calls `pip`. This seemed genious as then you could write a platform checker with an if statement and choose a different requirements files based on the results of this if.

