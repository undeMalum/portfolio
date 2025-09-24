+++
title = "Marimo as the successor to Jupyter Notebook."
date = "2025-09-21T16:39:19+02:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "Mateusz Konat"
authorTwitter = "" #do not include @
cover = ""
tags = ["my_projects", "creativity", "coding"]
keywords = ["my_projects", "creativity", "coding"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
draft = true
+++

# Why should you care?
[Jupyter Notebooks](https://jupyter.org) are a cornerstone of the Python ecosystem, especially in research and data science. Their ability to combine code, markdown, results, and visualizations makes them a powerful tool for analysis and communication.

But they also come with well-documented drawbacks: hidden execution order, reproducibility issues, and encouragement of poor coding practices. A 2019 study — [*"A Large-scale Study about Quality and Reproducibility of Jupyter Notebooks"*](https://leomurta.github.io/papers/pimentel2019a.pdf)[^fn] — highlighted how these problems undermine reliability at scale.

[^fn]: J. F. Pimentel, L. Murta, V. Braganholo and J. Freire, "A Large-Scale Study About Quality and Reproducibility of Jupyter Notebooks," 2019 IEEE/ACM 16th International Conference on Mining Software Repositories (MSR), Montreal, QC, Canada, 2019, pp. 507-517, doi: 10.1109/MSR.2019.00077.

It’s no surprise, then, that new tools are emerging to preserve Jupyter’s strengths while addressing its weaknesses.

One of the most promising is [marimo](https://marimo.io) — **a next-generation Python notebook**.

Launched in [August 2023](https://github.com/marimo-team/marimo/releases/tag/0.1.0), marimo has already gained significant traction, with nearly 700 forks :fork_and_knife: and over 16k stars :star: on [GitHub](https://github.com/marimo-team/marimo).

As the [README](https://github.com/marimo-team/marimo/blob/main/README.md) puts it:

> marimo is a reactive Python notebook: run a cell or interact with a UI element, and marimo automatically runs dependent cells (or marks them as stale), keeping code and outputs consistent. marimo notebooks are stored as pure Python (with first-class SQL support), executable as scripts, and deployable as apps.

In this post, I’ll highlight how marimo solves three persistent Jupyter pain points: **execution order, git-friendliness, and reproducibility**. However, rather than just listing marimo features, we’ll look at _how_ these specific problems look like in Jupyter and _why_ they occur and, then, present how marimo approaches them differently.

To wrap up, I’ll share two demos that showcase marimo in action and (hopefully) convince you to give it a try.

Let’s dive in.

# Git Friendliness

