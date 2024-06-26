+++
title = "Resource Center Environment Preparation"
date = "2023-08-28T11:34:14+02:00"
author = "Mateusz Konat"
authorTwitter = "" #do not include @
cover = "images/cover.png"
tags = ["coding"]
keywords = ["coding"]
description = "This is an installation guide as an add-on to the main article from IBlieve about the Resource Center."
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++

# Explanation
I wrote an extremely detailed article about contributing to the Resource Center for IBlieve. The assumption was the reader would have no prior knowledge about any of the tools I presented. Therefore, I explained each and every step you need to go thorough - from installation, setting up the environment, to the actual contribution and all things that follow. I turned out almost to be almost 6000 words and 60 pages! I needed to cut it down, but I didn't want to simply delete it. 

That's why I'm moving a large part of the main article to my personal blog so that if you do not have required knowledge, you can quickly catch up and set up the work environment on your machine.

# Hugo installation
To install Hugo, you need to go to the installation page of your operating system – either [Windows](https://gohugo.io/installation/windows/) or [macOS](https://gohugo.io/installation/macos/) (I assume no reader uses [Linux](https://gohugo.io/installation/linux/), but there’s a page for that as well.) There, you can find several methods for installing Hugo. I’ll explain two methods (one for each operating system) I find the simplest.
(One more thing: all methods are different, but what they have in common is that they are all based on [Command Line Interface](https://en.wikipedia.org/wiki/Command-line_interface) (CLI). Normally, you use [Graphical User Interface](https://en.wikipedia.org/wiki/Graphical_user_interface) (GUI) and access functionalities by using mouse and graphical elements. In CLI, you use commands to do the same job instead of mouse and you have computer terminals instead of graphical elements. Different operating systems have different terminals and commands, though sometimes they’re similar. I hope this clarification was helpful. Now we can proceed)

## Windows
You need to open [Windows PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.3) (Windows’ terminal). You can do this by typing “PowerShell” in the search box (Fig.1). Make sure you open “Windows PowerShell” without “ISE” at the end.
 
{{< figure src="./images/terminal.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 1. Windows PowerShell." captionPosition="right" captionStyle="color: white;" >}}

This should pop up a console that looks like this:
 
{{< figure src="./images/console.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 2. Windows PowerShell console." captionPosition="right" captionStyle="color: white;" >}}

Now, we need to install a packet manager to install Hugo itself. (Reasons why we need this step are not important here, but feel free to look them up!) We will be using scoop as a packet manager, but chocolatey will do as well. With the PowerShell open, we need to type two command:

```console
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

After executing this command, agree to the execution policy by typing “y” (Fig.3)
 
{{< figure src="./images/powershell.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 3. Changing execution policy." captionPosition="right" captionStyle="color: white;" >}}

```console
irm get.scoop.sh | iex
```

And the installation of scoop should begin (Fig.4).
 
{{< figure src="./images/powershell2.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 4. Scoop installation." captionPosition="right" captionStyle="color: white;" >}}

Finally, we can install Hugo by executing the following command:
	scoop install hugo-extended
and Hugo should start installing (Fig.5).
 
{{< figure src="./images/hugo.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 5. Hugo installation." captionPosition="right" captionStyle="color: white;" >}}
 
Although, in our case, installing the extended version of Hugo is not necessary, it may be helpful in the future of with other projects. I also recommend running the command below to learn more about scoop:

```console
scoop --help
```

## macOS
To install Hugo on macOS, follow instructions from the video below:

{{< youtube WvhCGlLcrF8 >}}
 
In contrast to the Windows’ “scoop install hugo”, the “brew install hugo” command on mac installs the extended version of Hugo by default.


## Hugo commands
You can now execute the command “hugo --?” to get a glimpse of the Hugo functionality or go straight to the [documentation](https://gohugo.io/documentation/) to get a full picture. You may not need all of it, but it’s worth checking out as you go.
 
# Git
[Git](https://git-scm.com) is a version-control tool, meaning you can track you progress: see what changes you made, come back to a point in history, see what others did, store project in a cloud and many more.
We’ll need git to make changes to the Resource Center and host them online. To do so, we need to install git and set up a GitHub account (more on GitHub later). (Although you’ll learn the minimum of what you need to contribute to the Resource Center, I recommend watching [this video](https://www.youtube.com/watch?v=RGOj5yH7evk) as it is a good introduction to git and GitHub that’ll give you a broader perspective.)

## Git installation
Installing git looks very similar on both Windows and macOS.

### Windows
To install git on Windows you need to open either PowerShell again or Command Prompt a.k.a cmd (another Windows’ terminal). (For our purpose, it doesn’t matter which one you use, but you can read about [differences between them](https://support.microsoft.com/en-us/windows/powershell-is-replacing-command-prompt-fdb690cf-876c-d866-2124-21b6fb29a45f).) After opening of the above, run:

```console
scoop install git
```

And voila! You’ve just installed git (Fig.6a-c).
 
{{< figure src="./images/git.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 6a. Installing zip folder." captionPosition="right" captionStyle="color: white;" >}}

{{< figure src="./images/git2.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 6b. Installing git." captionPosition="right" captionStyle="color: white;" >}}

{{< figure src="./images/git2.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 6c. Additions." captionPosition="right" captionStyle="color: white;" >}}

## macOS
Before installing git on macOS by yourself, make sure it is not already installed on device. Many Apple devices come with preinstalled git. To check that, open the terminal and execute:

```console
git --version
```

If you get something like this “git version 2.7.0 (Apple Git-66)”, you don’t have to do anything. If you receive a “command git not found”, it means you need to install git manually. I recommend the video below:
 
{{< youtube pQGEfLokfN0 >}}

## GitHub
[GitHub](https://docs.github.com/en/get-started/quickstart/hello-world) is an online service for hosting git repositories, providing back-up of your work, enabling collaboration in open source projects (such as the Resource Center) and much more. There are other similar services such as [GitLab](https://about.gitlab.com), but the idea is the same.

To collaborate on the Resource Center, you need a GitHub account so let’s see how you can set it up.

## GitHub account
To kick off, go to the GitHub web and click “Sign up” (Fig.7).
 
{{< figure src="./images/github.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 7. Signing up to GitHub." captionPosition="right" captionStyle="color: white;" >}}

A nice invitation should appear along with a first blank to fill (Fig.8).

{{< figure src="./images/github2.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 8. Start oof sign up." captionPosition="right" captionStyle="color: white;" >}}

Now, provide all data and verify your account like so (remember to write your data, not from the example):
 
{{< figure src="./images/github3.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 9. Filling blanks" captionPosition="right" captionStyle="color: white;" >}}
 
And click “Create account” (Fig.10).
 
{{< figure src="./images/github4.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 10. Create account." captionPosition="right" captionStyle="color: white;" >}}

Go to your email where verification code was sent (Fig. 11). It will be different than the one below!
 
{{< figure src="./images/github5.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 11. Verification code." captionPosition="right" captionStyle="color: white;" >}}

You need to copy it and paste it here:
 
{{< figure src="./images/github6.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 12. Using verification code." captionPosition="right" captionStyle="color: white;" >}}

This will bring you to the personalization page (Fig.13). You may complete it you if you want or go straight to your newly-created dashboard by clicking “Skip personalization.”
 
{{< figure src="./images/github7.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 13. Personalization page." captionPosition="right" captionStyle="color: white;" >}}

And done! You’ve just set up your GitHub account!. We’ll return to it shortly after installing Visual Code Studio.
 
{{< figure src="./images/github8.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 14. GitHub dashboard." captionPosition="right" captionStyle="color: white;" >}}
 
# Visual Code Studio
Visual Code Studio (VSCode) is a popular code editor managed by Microsoft. Although we don’t have to use this particular code editor and even a notepad will do, it is recommended to use a good code editor as it simplifies writing projects. Besides, since VSC is popular, there’re plenty of materials about this tool across the web for you to learn from.

## VSCode installation
To get VSCode, go to the [download page](https://code.visualstudio.com/download) of VSCode website and choose your operating system (Fig.15).

{{< figure src="./images/vsc.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 15. VSCode for different operating systems." captionPosition="right" captionStyle="color: white;" >}}

The installation should begin and you should be directed to the tutorial page (Fig.8). 

{{< figure src="./images/vsc2.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 16. Tutorial page and installation." captionPosition="right" captionStyle="color: white;" >}}

After this step, things look a little bit different for mac and Windows users.
For macOS, the following video shows everything you need in the installation part:

{{< youtube 8CJXB4Nu1wo >}}

For Windows users, follows the steps from below.

Go to the “Downloads” folder and look for “VSCodeUserSetup”. Click it and another windows should pop up with the terms of use. Agree to it and click “Next” (Fig.17).
 
{{< figure src="./images/vsc3.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 17. Running installer." captionPosition="right" captionStyle="color: white;" >}}

Now, you’ll have to click “Next” twice. Do not change the default settings (unless you’re completely sure what you’re doing) because this may cause some problems later. Like so:
 
{{< figure src="./images/vsc4.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 18. Going through installer." captionPosition="right" captionStyle="color: white;" >}}
 
Then, in “Select Additional Tasks” choose option “Create a desktop icon” (Fig.19). This will make VSCode appear on your home screen.
 
{{< figure src="./images/vsc5.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 19. Creating desktop icon." captionPosition="right" captionStyle="color: white;" >}}

Next, click “Install” (Fig.20). This should create a usable shortcut on your computer after a few seconds/minutes, depending on your internet connection.
 
{{< figure src="./images/vsc6.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 20. VSCode installation." captionPosition="right" captionStyle="color: white;" >}}

Lastly, click “Finish” and wait for the VSCode to open (Fig.21a-b).
 
{{< figure src="./images/vsc7.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 21a. Finishing installation." captionPosition="right" captionStyle="color: white;" >}}

{{< figure src="./images/vsc8.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 21b. VSCode open." captionPosition="right" captionStyle="color: white;" >}} 

# Getting started with the project
Having all necessary software installed, we can now combine all the tools together!

## Forking the repository
To edit the Resource Center, we need to fork the original repository, that is, we need to make a copy of the original repository with the Resource Center on our GitHub.

Go to the following link: https://github.com/bprzybylski/IB-CS-GeS where the original repository is hosted. You should see a “Fork” button near a “Code” button (Fig.22). **Make sure you’re logged in!**
 
{{< figure src="./images/forking.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 22. The Resource Center repository." captionPosition="right" captionStyle="color: white;" >}} 

After clicking the Fork button a “Create a new fork” page should appear where you can edit the name and description of the repository (Fig.23.I-II). When you’re done with editing, click “Create fork” to finalize forking the repository (Fig.23.II).
 
{{< figure src="./images/forking2.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 23. Creating a new fork." captionPosition="right" captionStyle="color: white;" >}} 

Within a few moments, the repository should be forked (Fig.24).

{{< figure src="./images/forking3.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 24. Forked repository." captionPosition="right" captionStyle="color: white;" >}}  

However, it would be difficult ([though possible](https://github.com/github/dev)) to edit the Resource Center in the browser. Therefore, we need to create a local version of the repository (copy all the files into our device).
 
## Local version
First off, you need to create a folder where your project will be stored. Even though it is not required, it is **a matter of good practice** to do so. You just need to open a terminal and type the following command:

```console
mkdir Projects
```

You can replace "Projects" with whatever you like. Now we need to navigate to the folder we created. For that, we need a "cd" command. When you use:

```console
cd [relative_path]
```

you go inside the folder you typed (is one exists – relative_path is an actual path to a folder). When you type:

```console
cd ..
```
you return to the folder one level higher in the hierarchy. So if you created a folder "Projects", you type

```console
cd Projects
```

Now, we need to clone our repository (make copy on our device). Without closing terminal (PowerShell, cmd, or Terminal), go back to your online repository. Click the “Code” button (Fig.25.I), choose the “HTTPS” option (Fig.25.II) and copy the link from there (Fig.25.III).
 
{{< figure src="./images/forking4.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 25. Getting link for cloning the repository." captionPosition="right" captionStyle="color: white;" >}}  

Your link will be different, containing your user name. After copying, open the terminal once again (make sure you’re in the Projects folder) and write “git clone” (Fig.26.I) followed by the link we copied (Fig.26.II).
 
{{< figure src="./images/forking5.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 26. Command for cloning." captionPosition="right" captionStyle="color: white;" >}}  

Run the command and the cloning of the repository should be initiated (Fig.27).

{{< figure src="./images/forking6.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 27. Cloning the repository." captionPosition="right" captionStyle="color: white;" >}} 

# Setting git

We're not ready yet as we have to more things to do.

## Adding theme submodule
First, we need to update our theme because this folder is now empty (theme is appended as a git submodule which is not initiated when the main repository is cloned) (Fig.28).
 
{{< figure src="./images/submodule.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 28. Empty theme directory." captionPosition="right" captionStyle="color: white;" >}} 

Fortunately, all we need to do is run the command:

```console
git submodule init
```

followed by:

```console
git submodule update
```

as depicted on Figure 29.

{{< figure src="./images/submodule2.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 29. Updated the submodule." captionPosition="right" captionStyle="color: white;" >}} 

And if we return to the theme/hugo-book folder, we should see the files of the theme (Fig.30).
 
{{< figure src="./images/submodule3.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 30. Files of the theme" captionPosition="right" captionStyle="color: white;" >}} 

## Adding our data to git
Second, we need to tell git who we are. Fortunately, it only requires two commands:

```console
git config --global user.email you@example.com
```

and

```console
git config --global user.name "Your Name"
```

You don’t have to use the “--global” flag. It just sets the data you provides as a default configuration for all your git projects. So we’re going to use this flag (Fig.31).
 
{{< figure src="./images/data.png" alt="Hello Friend" position="center" style="border-radius: 8px;" caption="Figure 31. Setting user data." captionPosition="right" captionStyle="color: white;" >}}

Your environment is set up and ready to go!
