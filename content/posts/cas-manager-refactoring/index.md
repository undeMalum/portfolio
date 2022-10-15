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
let me introduce some concepts for better understanding. In computers science, there's a notion of `cognitive complexity`. It basically refers to **the complexity of code in terms of its readability**. In other words, it's a measure of how `threating` your code is for the sanity of `another party`, maintaining this code. 

But what does the maintenance refer to? Well, low cognitive complexity yields the improvement in areas such as `debugging`, `optimization`, `future development`, the `efficiency of team` work and many others, equally important. For me however, low cognitive complexity proves crucial because of two main aspects. **First, it makes the process of extending the app easier and, second, I plan to make my project global** (see ["Plans for the future section"](#plans-for-the-future)).

***
{{< neighbouring >}}
{{< figure src="./images/first_pair/first_separation_deletion_main.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Removing code from the initial (huge) file." captionPosition="right" captionStyle="color: white;" >}}break{{< figure src="./images/first_pair/first_separations.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Removing some more code." captionPosition="right" captionStyle="color: white;" >}}
{{< /neighbouring >}}
***

**Progress so far**<br>
Having described the background, I shall discuss **the already-made progress** (see my [Github](https://github.com/undeMalum/CAS-manager) for more in-depth explanation). 

First of all, I started refactorization with splitting up my program into different files to at least **reduce the size of an original file**. However, it quickly turned out to be almost `impossible`. I succeeded only with moving some functions. **The good thing though was I eventually realized the cause of this impossibility.** Namely, my initial approach required very `high dependency` between parts of my code, so they couldn't be easily separated.

**To tackle the issue,** I began removing the responsibilities from functions and making these functions more generic. As I was gradually applying the above principles to my code, **the benefits started showing up**. As of this moment, it's become easy to put, now `distinct`, pieces of program into their own files. Nevertheless, I didn't go for that (ie **creating separate files**) because I found out that the `contemporary methods` were too simple, too "_newbie_" - even though now they were more generic.

**My focus has switched therefore** to the way I insert students and classes into the database. What I gave a go in this context, were `classes`. Class is a concept derived from so-called `object-oriented programming`. **Class is a template for creating an object.** Moreover, class (and objects is creates) has attributes (such as student's *first_name*, *surname*, *URL* and *class_name* in my program) and methods (like "*insert_student*"). **The advantage of class is its preciseness**. NewStudent class deals with everything that is connected with managing a student. **Isn't it as clear as day?**

**To summarize this part,** I am putting my effort in `removing` the responsibilities from functions and in `making` these functions more generic as well as in `replacing` current methods/approaches with better ones.

***
{{< neighbouring >}}
{{< figure src="./images/second_pair/moving_function_not_finished.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Facing problems with dependency." captionPosition="right" captionStyle="color: white;" >}}break{{< figure src="./images/second_pair/managing_db_main.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Attempting to deal with this issue." captionPosition="right" captionStyle="color: white;" >}}
{{< /neighbouring >}}
***

# Plans for the future
Apart from improving _how_ I develop the program, I also have plans of _what_ developments will be made. As I mentioned in the previous section in ["Before going further"](#decisions-made-so-far) part, **I aim to make the process of extending the app easier and to make this app global.** 

**In the first place,** I want to shift focus toward extending the program. New features will include `changing theme` and `choosing sorting option`. The former is less important and addresses individual preferences of the user. This is because _the default look of widgets is boring and it's not even debatable_. I've learned however that I can incorporate ready-to-use themes into my software. Furthermore, these themes are usually available in two color versions - white and black. **I can take advantage of that and enable the user to choose with accordance to his/her preferences.** The latter is more crucial as it not only provides greater convenience, but also changes significantly the overall appearance of the app. **I plan to give the user opportunity to pick the order in which classes and students are displayed.** _Nonetheless, it affects the deep design of my program so it has to be cleverly planned_.

**Another aspect is serving the global audience of IBDP.** When I was talking to my CAS coordinator about the program, I thought about sharing my work with a `broader audience`. If one CAS coordinator has a certain problem, so does another, right? Based upon this reasoning, I made attempts to give the software to others. Before sharing however, _I wanted to invite other people to develop this program_. **To do so, I needed improvements described previously.** I'm working hard to eventually post my program here: [the unofficial subreddit for all things concerning the International Baccalaureate](https://www.reddit.com/r/IBO/).

That's all I'm planning now.

***
{{< neighbouring >}}
{{< figure src="./images/third_pair/alter_db_abc.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Creating ABC for function altering db." captionPosition="right" captionStyle="color: white;" >}}break{{< figure src="./images/third_pair/implementation_of_abc.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Implementing ABC to manage classes." captionPosition="right" captionStyle="color: white;" >}}
{{< /neighbouring >}}
***

# The complaint of a coder
This is the moment to share my thoughts about the process.

As times goes on, **I do see the improvements being made**. Applying the above-mentioned technics comes with a `great benefit` as the program gets less complicated. Admittedly, these are `small things`, however it's undoable to have all things done in two evenings. **Nonetheless, no matter how wise a man is, sometimes his own ambition overcomes him and the process is diminished by the events.** This has happened to me several time. 

Case in point would be **the missing letter "i" in the initializer of a class** (initializer is a method that let's you set some initial values for a class). My IDE (place for writing software) gave me the following hint: `__int__`. However, initializer is indicated with `__init__`. You don't see a difference? **Pay close attention to the spelling and you'll see a "minor" mistake.** This little thing made me suffer from anger and the whole program to crush. After the discovery, I felt _a mix of relief_ (everything suddenly worked) _and madness_ (I believed it was a problem with the design!).

All things considered, I should be less `outcome dependent` - for my own mental health, I suppose. Nevertheless, I have chosen a path and prepared the plan, therefore, **I am confident that I'll achieve my goals**.

# Learning outcomes
- LO 1 Strength and Growth
- LO 2 Challenge and Skills
- LO 3 Initiative and Planning
- LO 4 Commitment and Perseverance
- LO 5 Collaborative Skills
- LO 6 Global Engagement
- LO 7 Ethics of Choices and Actions
