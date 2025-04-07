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
Problem-solving here followed a few clear stages (and no, I don’t mean momentary lapses of faith), so I’ll break this down accordingly.

## Conditional expression
As a Python user, my first instinct was to use a conditional expression—like an `if` statement—to detect the OS and select the correct requirements file.

I started by Googling: [_conditional setting for tox_](https://www.google.com/search?q=conditional+setting+for+tox), but that didn’t lead anywhere useful. After several failed attempts to rephrase the query, I refined it to include platform detection: [_conditional setting for tox detecting platform_](https://www.google.com/search?q=conditional+setting+for+tox+detecting+platform).

That search finally turned up a helpful Stack Overflow post: **[How to conditionally set tox variables depending on the platform](https://stackoverflow.com/questions/61510250/how-to-conditionally-set-tox-variables-depending-on-the-platform)**.

Interestingly, it didn’t involve traditional conditional logic but rather used tox’s environment-specific configuration. It wasn’t a complete fix though.

## Tox Environments
The Stack Overflow post linked to a relevant Tox documentation page: **[Platform specification](https://tox.wiki/en/latest/faq.html#platform-specification)**.

It described how to set up Tox environments targeting both Python versions and OS types:

```ini
[tox]
envlist = py{310,311}-{lin,win}
```

Then, within the test environment config, you can specify platform-based requirements:

```ini
[testenv]
platform = lin: linux
           win: win32

deps =
    linux: -r {toxinidir}/unix-requirements.txt
    windows: -r {toxinidir}/requirements.txt
```

This was a fantastic discovery—it directly solved my issue of selecting requirement files based on the OS. The only drawback was that this configuration had to live in a `tox.ini` file, as `pyproject.toml` doesn't support this syntax. Still, using multiple config files is a common issue in Python projects; avoiding it entirely would mean abandoning Python itself.

What truly disrupted the moment of relief, though, was trying to run Tox in GitHub Actions. That opened a whole new can of worms and pushed me toward solutions I had hoped never to revisit.

## Duplicate config files
To clarify my earlier frustration: the Tox setup worked flawlessly _locally_. Running `tox` in the terminal gave me exactly what I needed—testing across multiple Python versions and OSes.

But things broke down once the code hit the remote repository.

I had a GitHub Actions workflow set up to trigger tests (along with linting and type checking) on every pull request. For this, I used the [`tox-gh-actions`](https://github.com/ymyzk/tox-gh-actions) plugin, since Tox doesn’t support GitHub Actions out of the box. And that’s where my excitement came crashing down.

GitHub Actions creates the OS environment separately from Tox, so when Tox tried to create *both* Linux and Windows environments within the already-specified OS runner, things got weird fast. Wrong requirements files were being installed, and the environment setup just fell apart.

I nearly exploded. But, being the _mature_ developer I am, I took a deep breath… and went to cry in bed. After a brief crisis of faith, I came back to face the issue head-on—even if the solution was the one I wanted to forget.

That solution? Duplicating the Tox config—creating separate files like `tox_win.ini` and `tox_lin.ini`, one for each platform. Then I used GitHub Actions conditionals to choose the appropriate config:

```yml
- name: Install dependencies (Ubuntu)
  if: runner.os == 'Linux'
  run: |
    python -m pip install --upgrade pip
    pip install -r unix-requirements.txt
    pip install tox tox-gh-actions pytest pytest-cov

- name: Install dependencies (Windows)
  if: runner.os == 'Windows'
  run: |
    python -m pip install --upgrade pip
    pip install -r requirements.txt
    pip install tox tox-gh-actions pytest pytest-cov
```

So why did I hate this solution? Because now I had to maintain two nearly identical config files. Any change made in one had to be mirrored in the other, or the behavior between platforms would diverge—introducing inconsistencies and headaches.

This was especially painful during a phase of active development, where I was iterating quickly. Every quick tweak carried the risk of breaking tests on the other platform.

Sure, it worked—but I wasn’t happy about it.

I briefly considered a more elegant workaround: customizing the Tox installation step using an external script, via [installation customization](https://tox.wiki/en/latest/config.html#install_command). The idea was to write a platform-aware script with an `if` statement to select the appropriate requirements file. Sounds great in theory, but in practice:

1. Tox threw errors when trying to use a custom Python script to run `pip`—claiming Python wasn’t installed.
2. Bash scripts were off the table since they wouldn’t run on Windows. I did find a workaround, but by then I had found a better path forward (more on that later).

I should’ve been satisfied. The pipeline worked. But the nagging thought that _there has to be a better way_ just wouldn’t let me rest.

## Great return and solution
On one hand, I was _desperate_ to find a better solution. On the other, I was exhausted—I'd already sunk way too much time into this.

Then one day, while procrastinating and casually browsing popular GitHub repositories (think pandas, Django, etc.), I stumbled into the [`tox-gh-actions`](https://github.com/ymyzk/tox-gh-actions) repo. It had a surprisingly long README, so I started skimming.

And there it was: a section titled **[Factor-Conditional Settings: Environment Variable](https://github.com/ymyzk/tox-gh-actions?tab=readme-ov-file#factor-conditional-settings-environment-variable)**. It was _exactly_ the solution I’d been chasing.

It built on the setup I described in the [Tox Environments](#tox-environments) section, with two small additions that made everything work smoothly within GitHub Actions:

1. Add this snippet to `tox.ini` to map GitHub OS runners to platform names:
   ```ini
   [gh-actions:env]
   PLATFORM =
       ubuntu-latest: linux
       macos-latest: macos
       windows-latest: windows
   ```

2. Then, pass the GitHub OS matrix variable to Tox via the environment:
   ```yml
   - name: Run TOX tests
     run: tox
     env:
         PLATFORM: ${{ matrix.os }}
   ```

And that was it. _That_ was the fix.

After all the trial and error, rabbit holes, and half-baked workarounds, the answer had been sitting in the plugin’s README the whole time—I just hadn’t read it closely.

At least now, I’ve earned myself one of those mistakes you only make *once*. Brutal lesson, but it stuck.

# Closing and looking ahead
It’s been quite a journey—both solving the issue and now looking back on it.

Of course, I’ve skipped over plenty of other challenges along the way: GitHub runner memory limits, manually passing Tox environments, and more. But I wanted to keep the focus narrow, especially since this post is already pretty long. Plus, there are so many other topics I’d love to explore.

In the near future (May 9–10), we’ll be showcasing the current state of our project at the [Ghost Day](https://ghostday.pl) conference, where I’ll be one of the presenters. I plan to share some thoughts about the event and our presentation soon.

I’m also enrolled in a fascinating course—[Decoding Life Signals](https://github.com/SIPPRE/DecodingLifeSignals)—which I’d love to write about down the line.

But, as always, time is limited. So we’ll see what comes next.

That’s it for now—thanks for reading. Until next time!
