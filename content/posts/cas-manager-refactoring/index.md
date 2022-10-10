+++
title = "Creativity. Reflection 2 -  Refactoring CAS Manager"
date = "2022-10-09T08:03:08+02:00"
author = "Mateusz Konat"
authorTwitter = "" #do not include @
cover = "images/difference_branches.png"
tags = ["creativity", "coding"]
keywords = ["creativity", "coding"]
description = "Although I 'finished' coding the CAS portfolio manager and made it work, the code itself was crap. So I've decided to improve it and here I explain the process."
showFullContent = false
readingTime = false
hideComments = false
+++

# Content:
- [Decisions made so far](#decisions-made-so-far)
- [Plans for the future](#the-complaint-of-a-coder)
- [The complaint of a coder](#the-complaint-of-a-coder)
- [Learning outcomes](#learning-outcomes)

# Decisions made so far
**Before going further**<br>
let me introduce some concepts for better understanding. In computers science, there's a notion of cognitive complexity. It basically refers to the complexity of code in terms of its readibility. In other words, it's a measure of how threating your code is for the sanity of another party, maintaining this code. 

But what does the miantenance refer to? Well, low cognitive complexity yields the improvmenet in areas such as debugging, optimazation, future development, the efficiency of team work and many others, equally important. Therefore, for me, low congnitive complexity proves crucial since, first, it make the process of extending apps easier and, second, I plan to make my project global (see ["Plans for the future section"](#plans-for-the-future)).

**Progress so far**<br>
Having described the background, I shall present the already-made progress. 

First of all, I started refactorization with splitting up my program into different files to at least reduce the size of the original file. However, it quickly turned out to be almost impossible. I succeeded only with moving some functions (I didn't get rid of interconnection entirely though). Because of that, I discovered that my initial approach required very high dependency between parts of my code, so they couldn't be easily separated.

As a result, the second thing I've done is removing the responsibilites from functions and making these functions more generic. As I was gradually simplifying my code, I was astonished 

# Plans for the future

# The complaint of a coder

# Learning outcomes
