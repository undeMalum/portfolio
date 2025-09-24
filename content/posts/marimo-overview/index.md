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
[Jupyter Notebooks](https://jupyter.org) have been widely adopted by the Python community, _especially_ on the research and data science side. They are an excellent tool for data analysis as they allow you to combine code with documentation/annotation in the form of markdown, execution results, and visualisation.

However, as much as they are an awesome tool, they come with a number of drawbacks such as unexpected behaviours, encouraging poor coding practices, and issues with reproducibility - a case in point is this study from 2019: [_"A Large-scale Study about Quality and Reproducibility of Jupyter Notebooks"_](https://leomurta.github.io/papers/pimentel2019a.pdf)[^fn].

_(SIDE NOTE: The study is really short - 10 pages - and is certainly worth reading.)_

[^fn]: J. F. Pimentel, L. Murta, V. Braganholo and J. Freire, "A Large-Scale Study About Quality and Reproducibility of Jupyter Notebooks," 2019 IEEE/ACM 16th International Conference on Mining Software Repositories (MSR), Montreal, QC, Canada, 2019, pp. 507-517, doi: 10.1109/MSR.2019.00077.

Therefore, it should come as no surprise that people have attempted to devise solutions that preserve the massive result presentation abilities of Jupyter Notebooks, while mitigating inherent problems that come with them.

One such interesting solution, I would like to talk about, is [marimo](https://marimo.io) - **a next-generation Python notebook**.

Marimo is fairly recent, with first release dating back to the [August of 2023](https://github.com/marimo-team/marimo/releases/tag/0.1.0), but has already gained immense popularity on [GitHub](https://github.com/marimo-team/marimo), having nearly 700 forks and over 16 thousand stars.

In the project [README](https://github.com/marimo-team/marimo/blob/main/README.md), we can read:
> marimo is a reactive Python notebook: run a cell or interact with a UI element, and marimo automatically runs dependent cells (or marks them as stale), keeping code and outputs consistent. marimo notebooks are stored as pure Python (with first-class SQL support), executable as scripts, and deployable as apps.

I will attempt to explain some of these concepts in this blog posts. However, my approach is going to be slightly different. Rather than simply presenting the marimo features, I will begin with stating a problem that Jupyter Notebooks run into, and then explain how marimo tackles this issue.

The three main issue I would like to focus on in this posts are git-friendliness, order of execution, and reproducibility. Of course, there are way more topics that can be covered here, but let me stick with this for comprehensiveness of this post and my sanity (if you're interested, check out marimo [docs](https://docs.marimo.io) and [YouTube](https://www.youtube.com/@marimo-team) channel).

Later, I will also present two demos as a thought-provoking ending as ab encouragement for you to start with marimo if somehow the previous sections failed to persuade you.

That was a lengthy start, but, without further ado, let's start exploring marimo.
