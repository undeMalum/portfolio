+++
title = "Creativity. Reflection 2 -  Refactoring CAS Manager"
date = "2022-10-15T08:03:08+02:00"
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
- [Plans for the future](#plans-for-the-future)
- [The complaint of a coder](#the-complaint-of-a-coder)
- [Learning outcomes](#learning-outcomes)

# Decisions made so far
**Before going further**<br>
let me introduce some concepts for a better understanding. In computer science, there's a notion of `cognitive complexity`. It basically refers to **the complexity of a code in terms of its readability**. In other words, it's a measure of how `threatening` your code is for the sanity of `another party`, maintaining this code. 

But what does maintenance refer to? Well, low cognitive complexity yields improvements in areas such as `debugging`, `optimization`, `future development`, `team work efficiency` and many others, equally important. For me, however, low cognitive complexity proves crucial because of its two major aspects. **First, it makes the process of extending the app easier and, second, it will help me to go global as I intended to.** (see ["Plans for the future section"](#plans-for-the-future)).

***
{{< neighbouring >}}
{{< figure src="./images/first_pair/first_separation_deletion_main.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Removing code from the initial (huge) file." captionPosition="right" captionStyle="color: white;" >}}break{{< figure src="./images/first_pair/first_separations.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Removing some more code." captionPosition="right" captionStyle="color: white;" >}}
{{< /neighbouring >}}
***

**Progress so far**<br>

{{< code language="python" title="Design patterns and such..." id="1" expand="Show" collapse="Hide" isCollapsed="true" >}}
import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!

{{< /code >}}

Having described the background, I shall discuss **the progress made so far** (see my [Github](https://github.com/undeMalum/CAS-manager) for more in-depth explanation). 

First of all, I started refactorization by splitting up my program into different files to at least **reduce the size of the original file**. However, this quickly turned out to be almost `impossible`. I succeeded only with moving some functions. **The good thing though was I eventually realized why this was impossible.** Namely, my initial approach required very `high dependency` between parts of my code, so they couldn't be easily separated.

**To tackle the issue,** I began removing the responsibilities from functions and making them more generic. As I was gradually applying the above principles to my code, **the benefits started showing up**. As of this moment, it's become easy to put, the `now-distinct`, pieces of the program into their own files. I didn't go for that after all (i.e. **creating separate files**) because I had found out that `the then methods` were too simple, too "_newbie_" - even though now they were more generic.

**My focus has switched therefore** to the way I insert students and classes into the database. What I gave a go in this context were `classes`. Class is a concept derived from so-called `object-oriented programming`. **It is a template for creating an object.** Moreover, a class (and objects it creates) has attributes (that store data, e.g. student's *first_name*, *surname*, *URL* and *class_name* in my program) and methods (that provide behaviours, like "*insert_student*"). **The advantage of a class is its preciseness**. For instance, NewStudent class deals with everything that is connected with managing a student. **Isn't it as plain as day?**

**To summarize this part,** I am putting my effort into `removing` the responsibilities from functions and into `making` them more generic as well as into `replacing` current methods/approaches with better ones.

***
{{< neighbouring >}}
{{< figure src="./images/second_pair/moving_function_not_finished.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Facing problems with dependency." captionPosition="right" captionStyle="color: white;" >}}break{{< figure src="./images/second_pair/managing_db_main.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Attempting to deal with this issue." captionPosition="right" captionStyle="color: white;" >}}
{{< /neighbouring >}}
***

# Plans for the future
Apart from improving the way _how_ I develop the program, I also have plans _what_ is going to develop. As mentioned in [the previous section](#decisions-made-so-far), **I aim to make the process of extending the app easier and to make this app global.** 

**In the first place,** I want to shift focus toward extending the program. New features will include `changing a theme` and `choosing a sorting option`. The former is less important and addresses individual preferences of the user. This is because _the default look of widgets is unarguably boring_. I've learned, however, that I can incorporate ready-to-use themes into my software. Furthermore, these themes are usually available in two color versions - white or black. **I can take advantage of that and enable the user to choose in accordance with his/her preferences.** The latter is crucial as it not only provides greater convenience, but also changes significantly the overall appearance of the app. **I plan to give the user opportunity to select the order in which classes and students are displayed.** _However, it affects the deep design of my program so it has to be cleverly planned_.

**Another aspect is serving the global audience of IBDP.** After talking to my CAS coordinator about the program, I thought about sharing my work with a `broader audience`. If one CAS coordinator has a certain problem, so does another, right? Based upon this reasoning, I made attempts to give the software to others. Before sharing however, _I wanted to invite other people to develop this program_. **To do so, I needed improvements described previously.** I'm working hard to eventually post my program here: [the unofficial subreddit for all things concerning the International Baccalaureate](https://www.reddit.com/r/IBO/).

That's all I'm planning now.

***
{{< neighbouring >}}
{{< figure src="./images/third_pair/alter_db_abc.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Creating ABC for function altering db." captionPosition="right" captionStyle="color: white;" >}}break{{< figure src="./images/third_pair/implementation_of_abc.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Implementing ABC to manage classes." captionPosition="right" captionStyle="color: white;" >}}
{{< /neighbouring >}}
***

# The complaint of a coder
This is the moment to share my thoughts about the process.

As times goes by, **I do see improvements being made**. Applying the above-mentioned techniques comes with a `great benefit` as the program gets less complicated. Admittedly, these are `small things`, however, it's undoable to have all things done in two evenings. **Nonetheless, no matter how wise a man is, sometimes his own ambition overcomes him and the process is damaged by such mishaps.** This has happened to me several time. 

A case in point could be **the missing letter "i" in the initializer of a class** (initializer is a method that lets you set some initial values for a class). My IDE (place for writing software) gave me the following hint: `__int__`. However, an initializer is indicated with `__init__`. You don't see a difference? **Pay close attention to the spelling and you'll see a "minor" mistake.** This flaw made me furius and caused the whole program to crush. After its discovery, I felt _a mix of relief_ (everything suddenly worked) _and madness_ (I believed it was a problem with the design!).

All things considered, I should be less `outcome-dependent` for my own mental health, I suppose. All in all, I have chosen a path and prepared a plan, therefore, **I am confident that I'll achieve my goals**.

# Learning outcomes:
- LO 1 Strength and Growth
- LO 2 Challenge and Skills
- LO 3 Initiative and Planning
- LO 4 Commitment and Perseverance
- LO 5 Collaborative Skills
- LO 6 Global Engagement
- LO 7 Ethics of Choices and Actions
