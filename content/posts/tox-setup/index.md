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

# What's What  
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

This search didn't prove useful though. And after a couple of trial-and-error attempts at paraphrasing this inquiry, I started added new information. 

