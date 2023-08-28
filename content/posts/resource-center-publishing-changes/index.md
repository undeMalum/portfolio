+++
title = "Resource Center Publishing Changes"
date = "2023-08-28T14:27:17+02:00"
author = "Mateusz Konat"
authorTwitter = "" #do not include @
cover = "images/cover.png"
tags = ["coding"]
keywords = ["coding"]
description = "This is another add-on to the main article about the Resource Center. This time, it's about publishing your work to the main repository."
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++

# Explanation
The reason I'm writing this is explained [here](/portfolio/posts/resource-center-environment-preparation). Now I'd like to move on to the step-by-step guide to publishing changes.

# Publishing our changes
We want to push our changes so we need to learn more git commands. The first one is:

```console
git add [files]
```

With the “git add” command we can add changes we made for git to track. We can either provide relative paths to the files from which we want to add changes to git (relative to the main directory, in our case “IB-CS-GeS”) or just type:

```console
git add .
```

More likely, you’ll be just using the second option, but who knows? Maybe one day it’ll come useful. Anyway, we’re going to use the dot variation to add our Test subsection (Fig.1). (Look at the color of file highlighting.)

{{< figure src="./images/git.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 1. Git add." captionPosition="right" captionStyle="color: white;" >}}

Don’t worry about the warning. You won’t feel its effect. After adding our changes to git, we need to prepare a commit – a single unit of change with description (kind of.) For that, we use:

```console
git commit
```

However, if you ran only this, you’ll receive an error. We need to also supply a “-m” flag with a commit description:

```console
git commit -m “Our first commit”
```

Of course, you can put whatever you want in between the quotation marks. However, try to make as meaningful messages as possible. It simplifies the reviewing process. You can also supply another “-m” flag to give a more detailed description (the first one should be a few words, here you can elaborate):

```console
git commit -m “Our first commit” -m “Some description”
```

To commit our changes we’re going to write “Add subsection Test” as a commit name and “Add a testing page with sample content to show the workings of open source” (Fig.2). (Look at the color of file highlighting.)

{{< figure src="./images/git2.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 2. Committing our changes." captionPosition="right" captionStyle="color: white;" >}}

You can see that the colors of the files have changed. That’s good because it means we’ve done everything correctly and we can push our changes to the remote repository. For that, we need to execute:

```console
git push -u origin main
```

Please note that it’s just for the first time. Thanks to the “-u” flag, git will remember the rest of the command (repository and branch) and we will have to just type:

```console
git push
```

But it’s our first time and we need to use the first option (Fig.3).

{{< figure src="./images/git3.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 3. Git push." captionPosition="right" captionStyle="color: white;" >}}

From that moment on, we will have to go through a few windows. As a first, choose “manager” from the “CredentialHelperSelectior” windows and follow the instructions to log in to the GitHub.
When you’re done, return to the VSCode where the following should appear:
 
{{< figure src="./images/git4.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 4. Pushing changes to the remote." captionPosition="right" captionStyle="color: white;" >}}

This way, we updated our remote repository with changes made on our device. If you go open our GitHub now, you should notice that a few things changed:
- A new box appeared with a message “This branch is one commit ahead of…” (Fig.5.I)
- A commit ID and time were updated (Fig.5.II)
 
{{< figure src="./images/git5.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 5. New information." captionPosition="right" captionStyle="color: white;" >}}

If we click the blue part of the above-mentioned message, we should be able to inspect changes we made (Fig.6).
 
{{< figure src="./images/git6.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 6. Changes." captionPosition="right" captionStyle="color: white;" >}}

Green color indicated what we added. Red indicated what we deleted. Since we only added stuff, there’s no red color. Nevertheless, what is particularly interesting in the page we opened it this green “Open pull request” button (Fig.7). This button will allows you to push your changes to the main repository (the original one which we forked.)

{{< figure src="./images/git7.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 7. Open pull request button." captionPosition="right" captionStyle="color: white;" >}}

If we click this button, we should see the following:
 
{{< figure src="./images/git8.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 8. Creating pull request." captionPosition="right" captionStyle="color: white;" >}}

There are few interesting things. First, the message that we can merge (Fig.8.I). Second, the name and description of our commit (you should recognize it) (Fig.8.II). We can beautify out message with some formatting, however, it is not necessary. When we’re sure that all messages are correct, we can once again hit the button “Create pull request” (Fig.8.III). After this step, the information about a new pull request should appear (Fig.9).

{{< figure src="./images/git9.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 9. New pull request." captionPosition="right" captionStyle="color: white;" >}}

You can also leave a comment with a box at the bottom (Fig.10).

{{< figure src="./images/git10.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 10. Pull request comment." captionPosition="right" captionStyle="color: white;" >}}

This place is very important because here we can talk about the changes. I can ask you a bunch of questions about a change and propose some improvement. You can defend your position, agree with me (and possibly others), or add something new. Here, we should have an open discussion about the course of our actions. And, actually, it is all you can do at this point.

I, as an owner, can either accept your pull request (your changes) or reject it. So unless I make a move, you can only think what else can be changed. However, if I do accept the change, you should go back to your repository where yet another message should pop up (Fig.11).
 
{{< figure src="./images/git11.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 11. 1 commit behind message." captionPosition="right" captionStyle="color: white;" >}}

The “this branch is 1 commit behind” indicates that some changes were made to the original repository and we should catch up (this happens after merging a pull request by me.) To do so, we need to use “Sync fork” (Fig.12.I)function and choose “Update branch” option (Fig.12.II).
 
{{< figure src="./images/git12.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 12. Update branch." captionPosition="right" captionStyle="color: white;" >}}

When we click “Update branch” button, we should receive two messages: “Successfully fetched and fast-forwarded from upstream” (Fig.13.I) and “This branch is up to date with” (Fig.13.II).
 
{{< figure src="./images/git13.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 13. Successfully updated repository." captionPosition="right" captionStyle="color: white;" >}}

If we update out remote repository, we have to update our local repository. So open VSCode and integrated terminal in it. Run this command:

```console
git pull
```

and your local version should get updated (Fig.14).

{{< figure src="./images/git14.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 14. Git pull." captionPosition="right" captionStyle="color: white;" >}}

Since you're now up-to-date, you can start thinking about another improvements!

