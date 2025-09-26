+++
title = "Marimo as the successor to Jupyter Notebook."
date = "2025-09-21T16:39:19+02:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "Mateusz Konat"
authorTwitter = "" #do not include @
cover = "images/marimo-logotype-thick.svg"
tags = ["my_projects", "creativity", "coding"]
keywords = ["my_projects", "creativity", "coding"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
draft = true
+++

# Why Should You Care?
[Jupyter Notebooks](https://jupyter.org) are a cornerstone of the Python ecosystem, especially in research and data science. Their ability to combine code, markdown, results, and visualizations makes them a powerful tool for analysis and communication.


<---! Include a graphic from the paper on how the example jpuyter looks like --->

But they also come with well-documented drawbacks: hidden execution order, reproducibility issues, and encouragement of poor coding practices. A 2019 study — [*"A Large-scale Study about Quality and Reproducibility of Jupyter Notebooks"*](https://leomurta.github.io/papers/pimentel2019a.pdf)[^fn] — highlighted how these problems undermine reliability at scale.

[^fn]: J. F. Pimentel, L. Murta, V. Braganholo and J. Freire, "A Large-Scale Study About Quality and Reproducibility of Jupyter Notebooks," 2019 IEEE/ACM 16th International Conference on Mining Software Repositories (MSR), Montreal, QC, Canada, 2019, pp. 507-517, doi: 10.1109/MSR.2019.00077.

It’s no surprise, then, that new tools are emerging to preserve Jupyter’s strengths while addressing its weaknesses.

One of the most promising is [marimo](https://marimo.io) — **a next-generation Python notebook**.

Launched in [August 2023](https://github.com/marimo-team/marimo/releases/tag/0.1.0), marimo has already gained significant traction, with nearly 700 forks :fork_and_knife: and over 16k stars :star: on [GitHub](https://github.com/marimo-team/marimo).

As the [README](https://github.com/marimo-team/marimo/blob/main/README.md) puts it:

> marimo is a reactive Python notebook: run a cell or interact with a UI element, and marimo automatically runs dependent cells (or marks them as stale), keeping code and outputs consistent. marimo notebooks are stored as pure Python (with first-class SQL support), executable as scripts, and deployable as apps.

In this post, I’ll highlight how marimo solves three persistent Jupyter pain points: **hidden states, git-friendliness, and reproducibility**. Instead of just listing marimo’s features, we’ll examine _how_ these issues arise in Jupyter and then see how marimo tackles them differently.

To make this concrete, I’ll use a simple, made-up example that mirrors a real-world workflow. This way, you can follow along step by step and clearly see the contrast.

To wrap up, I’ll share two demos that showcase marimo in action and (hopefully) convince you to give it a try.

Let’s dive in.

# A Very Real Scenario
In our “extremely realistic” scenario, we’ll conduct a *hardcore* mathematical analysis — one that we’ll progressively extend as we go.

We’ll begin with this simple setup:

*** 
{{< figure src="./images/jupyter/jupyter_initial_setup.png" alt="Screenshot presenting the initial setup of the Jupyter Notebook demo." position="center" style="border-radius: 8px;" caption="This is how the setup presents itself within Jupyter Notebook." captionPosition="right" captionStyle="color: white;" >}}
***

It may look trivial, but it already includes both markdown and code — enough to serve as a foundation for our example.

With that in place, let’s jump straight into the first problem: **hidden states**.

*Disclaimer: This post assumes some familiarity with Python and git. If you’re new to either, don’t worry — the examples should still be clear enough to follow along.*

# Pain Point #1: Hidden States

With our *beautifully crafted* setup in place, it’s finally time to unleash its power and run the cells!

So let’s execute everything top-to-bottom and admire the results:

*** 
{{< figure src="./images/jupyter/jupyter_first_run.png" alt="Screenshot presenting the first run of the Jupyter Notebook demo." position="center" style="border-radius: 8px;" caption="After executing all cells, we arrive at the expected result." captionPosition="right" captionStyle="color: white;" >}}
***

As expected, our **world-class analysis™** produced the number `11`. Glorious success. :tada:

But before we pat ourselves on the back too much, let’s talk about something sneaky lurking beneath the surface: **states**.

A variable’s *state* is simply the most recent value it was assigned when a cell ran. In our case:

* `a` is living its best life as **5**
* `b` is happily holding **6**

So far, so good. But here’s the catch: **states don’t always stay this innocent**. And this, dear reader, is where Jupyter starts to get… messy.

## Problem Definition
Let’s take our demo one step further:

1. Change the value of `a` to **6** and run that cell
2. Then run the addition cell

Anything weird going on?

***
{{< figure src="./images/jupyter/jupyter_hidden_state_no_re-execution.png" alt="Screenshot presenting the hidden state in the Jupyter Notebook demo." position="center" style="border-radius: 8px;" caption="How come a (6) + b (supposedly 7) equals 12?!" captionPosition="right" captionStyle="color: white;" >}}
***

Wait… does Jupyter not know how to add? Let’s do the math ourselves:

* `a` = **6**
* `b` = `a + 1` = **7**
* so `a + b` = **13**, not **12**!

And yet, Jupyter insists the answer is 12.

What’s going on here is the infamous **hidden state** problem. A hidden state is simply a value that *exists in memory* but isn’t visible or obvious in the notebook.

In this case, the cell that defines `b` wasn’t re-executed after we changed `a`, so Jupyter still thinks `b = 6` from the previous run.

It gets even sillier: if I *delete the cell* that defines `b`, the addition cell *still works*!

***
{{< figure src="./images/jupyter/jupyter_hidden_state_delete.png" alt="Screenshot presenting the hidden state in the Jupyter Notebook demo." position="center" style="border-radius: 8px;" caption="This is pure comedy and hidden states at their peak." captionPosition="right" captionStyle="color: white;" >}}
***

This is where Jupyter goes from quirky to dangerous. Hidden states make notebooks misleading, undermine reproducibility, and can even corrupt research results — which is no laughing matter.

But don’t worry: **marimo has a fix**.

## Marimo Solution
Let us perform the same workflow with marimo as we did with the Jupyter Notebook, meaning:

1. Run the initial setup of the notebook to see if it works
2. Update the value of `a` and see what happens
3. Delete `b` to observe how marimo handles that

Starting with the first step, here's how the executed marimo notebook looks like:

***
{{< figure src="./images/marimo/marimo_initial_run.png" alt="Screenshot presenting the executed marimo demo." position="center" style="border-radius: 8px;" caption="As anticipated, we got our beloved 11." captionPosition="right" captionStyle="color: white;" >}}
***

Everything looks just right - after all, we arrived at the expected result `11` as we did initially with Jupyter Notebook.

Apart from esthethic differences, all looks extremely similar.

For now, let's ingore `import marimo as mo` at the top - we'll come back to it in the next section.

Anyway, would marimo be able to handle changing variables more gracefully? Check it out!

***
{{< figure src="./images/marimo/marimo_no_hidden_state.png" alt="Screenshot presenting the executed marimo demo with change variable a." position="center" style="border-radius: 8px;" caption="Thanks to DAG, marimo was able to detect the changes in the variables." captionPosition="right" captionStyle="color: white;" >}}
***

Tada! Our analysis produced the longed-for `13`. But how was marimo able to detect the changes in multiple cells?

It's all thanks to the marimo's reliance on DAG - [Directed Acyclic Graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph).

Marimo utilizes DAG to build a logical representation of cell interdependencies. Therefore, changes in the variable definition from a given cell causes all cells that depend on this variable to also be run. Isn't that amazing?

In our demo analysis, the DAG looks like this:

***
{{< figure src="./images/dags/marimo_dag.png" alt="Picture of a marimo DAG of the demo." position="center" style="border-radius: 8px;" caption="DAG clearly maps the interdependencies of the marimo cells." captionPosition="right" captionStyle="color: white;" >}}
***

We can clearly see what depends on what and so does marimo - that's how it know it needs to run the cell with `b = a + 1` and consequently `a + b` upon changing `a`.

However, let's see how marimo's DAG handles missing variables and whether it's also somehow prone to the hidden states. For this sake, delete the cell that defines `b`:

***
{{< figure src="./images/marimo/marimo_delete_b.png" alt="Screenshot of the marimo demo with b cell deleted." position="center" style="border-radius: 8px;" caption="That is the correct, explicit bahavior to a missing variable." captionPosition="right" captionStyle="color: white;" >}}
***

Perfect! Marimo was able to detect that there is something fishy about adding two variables when one is missing and it threw a nice `NameError` message, which we can even read more about:

```text
Traceback (most recent call last):
  File "C:\Users\{SomeUser}\scoop\shims\portfolio\content\posts\marimo-overview\.venv\Lib\site-packages\marimo\_runtime\executor.py", line 139, in execute_cell
    return eval(cell.last_expr, glbls)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  Cell  marimo://C:\Users\{SomeUser}\scoop\shims\portfolio\content\posts\marimo-overview\math_analysis.py#cell=cell-4, line 1, in <module>
    a + b
        ^
NameError: name 'b' is not defined
```

We see that `NameError` was raised because `b` was not defined. The way marimo could detect it is also very easy to present as a graph:

***
{{< figure src="./images/dags/marimo_delete_b.png" alt="Picture of a marimo DAG of missing variable b." position="center" style="border-radius: 8px;" caption="Connections between nodes were deleted so marimo knew something is wrong." captionPosition="right" captionStyle="color: white;" >}}
***

Two birds with one - no more incorrect addition and no more addition on noexisting variables: all thanks to getting rid of hidden states.

The DAG architecture profoundness extends beyond just removing the issue of hidden states. Thanks to the clear execution order of cell and instructions withing defined by DAG, marimo notebooks cells can be order however you like without the risk of unexpected bahaviors.

This mitigates the issues of the Jupyter notebooks that questions the validity of research results.

Moreover, this ordered execution also mimick how "normal" python scripts are executed from top to bottom, with the clear order of execution of instructions - in fact, marimo notebooks _are_ Python scripts, but more on that in the next section.
