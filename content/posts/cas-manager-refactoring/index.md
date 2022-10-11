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

But what does the miantenance refer to? Well, low cognitive complexity yields the improvmenet in areas such as debugging, optimazation, future development, the efficiency of team work and many others, equally important. Therefore, for me, low congnitive complexity proves crucial since, first, it make the process of extending the app easier and, second, I plan to make my project global (see ["Plans for the future section"](#plans-for-the-future)).

**Progress so far**<br>
Having described the background, I shall present the already-made progress. 

First of all, I started refactorization with splitting up my program into different files to at least reduce the size of the original file. However, it quickly turned out to be almost impossible. I succeeded only with moving some functions. The good thing though was I eventually realized the cause of this impossibility. Namely, my initial approach required very high dependency between parts of my code, so they couldn't be easily separated.

To tackle the issue, I began removing the responsibilites from functions and making these functions more generic. As I was gradually applying the above principles to my code, the benefits started showing up. As of this moment, it's become easy to put, now distinct, pieces of program into their own files. Nevertheless, I didn't go for that because I found out that the contemporary methods were too simple, too "newbie" - even though now they were more generic.

My focus has swithced therefore to the way I insert students and classes into the database. What I gave a go in this context, were classes. Class is a concept from so-called object-oriented programming. It is a template for creating an object, it acts as a constructor. Classes (and pbjects they create) have attributes (such as student's first name, surname, website's link and class name in my program) and methods (like "insert_student"). The advantage of classes is their preciseness. NewStudent class deals with everything that is connected with managing a student. Isn't it obvious?

To summarize this part, I am putting my effort in removing the responsibilites from functions and in making these functions more generic as well as in replacing current methods/approaches with bettwer ones.

# Plans for the future
Apart from improving _how_ I develop the program, I also have plans of _what_ the developments will be made.

As I mentioned in the previous section in "Before going further", I aim to make the process of exteding the a

# The complaint of a coder

# Learning outcomes
