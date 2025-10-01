+++
title = "Marimo as the successor to Jupyter Notebook."
date = "2025-09-21T16:39:19+02:00"
#dateFormat = "2006-01-02" # This value can be configured for per-posBut DAGs give us more than just protection against hidden states. Because dependencies are explicit and **interactivity** is built-in, marimo notebooks don't care about **cell order**. You can rearrange cells however you like without breaking execution — something Jupyter can't guarantee. The **interactive** execution will still follow the logical dependency order defined by the DAG. date formatting
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

In this post, I’ll highlight how marimo solves three persistent Jupyter pain points: **[hidden states](#pain-point-1-hidden-states), [git-friendliness](#pain-point-2-git-friendliness), and [reproducibility](#pain-point-3-reproducibility)**. Instead of just listing marimo’s features, we’ll examine _how_ these issues arise in Jupyter and then see how marimo tackles them differently.

To make this concrete, I’ll use a simple, made-up example that mirrors a real-world workflow. This way, you can follow along step by step and clearly see the contrast.

To wrap up, I’ll share two demos that showcase marimo in action and (hopefully) convince you to give it a try.

Let’s dive in.

# A Very Real Scenario
In our “extremely realistic” scenario, we’ll conduct a *hardcore* mathematical analysis — one that we’ll progressively extend as we go.

We’ll begin with this simple setup:

<iframe src="./notebooks/hidden_states/jupyter/setup.html" width="100%" height="250px">
</iframe>

It may look trivial, but it already includes both markdown and code — enough to serve as a foundation for our example.

With that in place, let’s jump straight into the first problem: **hidden states**.

*Disclaimer: This post assumes some familiarity with Python and git. If you’re new to either, don’t worry — the examples should still be clear enough to follow along.*

# Pain Point #1: Hidden States

With our *beautifully crafted* setup in place, it’s finally time to unleash its power and run the cells!

So let’s execute everything top-to-bottom and admire the results:

<iframe src="./notebooks/hidden_states/jupyter/initial_run.html" width="100%" height="250px">
</iframe>

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

<iframe src="./notebooks/hidden_states/jupyter/no_re-execution.html" width="100%" height="250px">
</iframe>

Wait… does Jupyter not know how to add? Let’s do the math ourselves:

* `a` = **6**
* `b` = `a + 1` = **7**
* so `a + b` = **13**, not **12**!

And yet, Jupyter insists the answer is 12.

What’s going on here is the infamous **hidden state** problem. A hidden state is simply a value that *exists in memory* but isn’t visible or obvious in the notebook.

In this case, the cell that defines `b` wasn’t re-executed after we changed `a`, so Jupyter still thinks `b = 6` from the previous run.

It gets even sillier: if I *delete the cell* that defines `b`, the addition cell *still works*!

<iframe src="./notebooks/hidden_states/jupyter/delete_b.html" width="100%" height="250px">
</iframe>

This is where Jupyter goes from quirky to dangerous. Hidden states make notebooks misleading, undermine reproducibility, and can even corrupt research results — which is no laughing matter.

But don’t worry: **marimo has a fix**.

## Marimo Solution
Let’s repeat the same workflow in marimo:

1. Run the initial setup
2. Update the value of `a`
3. Delete `b` and see what happens

First, the initial run:

<iframe src="./notebooks/hidden_states/marimo/initial_run.html" width="100%" height="350px"></iframe>  

Everything looks great — just like in Jupyter, we get the expected result `11`. Apart from some aesthetic differences, it feels almost identical.

(For now, ignore the mysterious `import marimo as mo` at the top — we’ll revisit that later.)

Now, what happens if we change `a`?

<iframe src="./notebooks/hidden_states/marimo/update_a.html" width="100%" height="350px"></iframe>  

Tada! We get the correct `13`. No hidden states, no misleading results.

So how does marimo pull this off? The secret is its reliance on a **DAG (Directed Acyclic Graph)** — [yes, the graph theory kind](https://en.wikipedia.org/wiki/Directed_acyclic_graph) — combined with **interactivity**.

Marimo builds a DAG of cell dependencies and uses it to enable true **interactivity**: when a variable changes, every dependent cell automatically re-executes in real-time. In our case, changing `a` triggers both `b = a + 1` and `a + b` to re-run immediately. This **interactivity** means your notebook stays consistent without any manual intervention.

Here’s the DAG behind our demo:

<object type="image/svg+xml" data="./notebooks/hidden_states/dags/marimo_dag.svg" style="width:100%; height:auto;"></object>

Crystal clear. We see exactly what depends on what — and so does marimo.

But what if we delete `b` entirely?

<iframe src="./notebooks/hidden_states/marimo/delete_b.html" width="100%" height="350px"></iframe>  

Perfect. Instead of running with a ghost value, marimo raises an explicit `NameError`:

```text
NameError: name 'b' is not defined
```

Exactly what should happen.

And the DAG makes the failure just as clear visually:

<object type="image/svg+xml" data="./notebooks/hidden_states/dags/marimo_delete_b.svg" style="width:100%; height:auto;"></object>

Remove a node, break the graph — marimo instantly knows the logic is broken. Simple, yet powerful.

This **interactivity** is what sets marimo apart: the DAG doesn't just track dependencies, it actively uses them to keep your notebook synchronized in real-time. Change one cell, and marimo's **interactive** execution automatically propagates that change through the entire dependency chain.

So with one mechanism, we solve two problems at once:

* No incorrect results from stale values
* No “zombie variables” persisting after deletion

But DAGs give us more than just protection against hidden states. Because dependencies are explicit, marimo notebooks don’t care about **cell order**. You can rearrange cells however you like without breaking execution — something Jupyter can’t guarantee.

This eliminates a whole class of headaches around execution order and brings marimo notebooks closer to how Python scripts naturally run: clear, deterministic, and reproducible.

*In fact*, marimo notebooks **are** Python scripts. But we’ll get to that in the next section.

### Jupyter vs. marimo (Hidden States)

| Aspect                      | Jupyter Notebook                                                        | marimo                                                          |
| --------------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------- |
| **Hidden states**           | Variables can persist invisibly, leading to stale or misleading results | Impossible — DAG ensures all dependencies update automatically  |
| **Execution order**         | Depends on user clicks; easy to get out of sync                         | Explicit DAG determines order; always consistent                |
| **Deleting variables**      | “Zombie” values can still linger in memory                              | Raises clear errors (`NameError`) when dependencies are missing |
| **Cell ordering**           | Must be careful — reordering cells can break logic                      | Cells can be arranged freely; DAG keeps execution correct       |
| **Clarity of dependencies** | Implicit; users must track mentally                                     | Explicit; dependencies visible in DAG                           |

# Pain Point #2: Git-Friendliness
[Version Control Systems](https://en.wikipedia.org/wiki/Version_control) (VCS) — especially [git](https://git-scm.com) — are the backbone of modern software development. They let us track changes, roll back mistakes, safely experiment with new features, and (in team projects) assign credit (or blame :sweat_smile:) to the right developer.

This isn’t just useful in software: VCS also helps in research, where proper attribution is critical (see [this project](https://github.com/teorth/equational_theories) for a math-flavored example).

So it’s no surprise that researchers, data scientists, and developers all lean heavily on git — and since Jupyter is so popular in these domains, the two are often used together.

Now, before we dive into why Jupyter and git aren’t exactly best friends, let’s extend our mathematical analysis demo a bit:

* Add a cell that defines a new variable `z` as the result of our addition
* Add another cell that multiplies `z` by `3`

We’re not focused on *how* these cells work (we already know they’ll behave). What matters is how git *sees* these changes.

First, I’ll reset everything: restart the Jupyter Kernel (to clear results), re-run all cells, stage the file, and commit:

<iframe src="./notebooks/hidden_states/jupyter/initial_run.html" width="100%" height="250px"></iframe>  

```bash
git add content/posts/marimo-overview/math_analysis.ipynb
git commit -m "Restore initial Jupyter Notebook settings and run it"
git push
```

Now, let’s add our two new cells:

<iframe src="./notebooks/git_friendliness/jupyter/add_changes.html" width="100%" height="300px"></iframe>  

And once again, the git magic:

```bash
git add content/posts/marimo-overview/math_analysis.ipynb
git commit -m "Add changes to Jupyter Notebook and run it"
git push
```

Cool. Notebook updated, changes saved. Now let’s see why Jupyter + git can turn into a headache.

## Problem Definition
Before we get back to git, let’s peek under the hood of how Jupyter Notebooks are actually stored.

When you create a notebook, you get a `*.ipynb` file (ours is `math_analysis.ipynb`). You might assume it’s just a Python script in disguise, but nope - it’s actually **JSON**.

Here’s a simplified snippet:

```json
{
  "cells": [
    {
      "cell_type": "markdown",
      "id": "d6605f7b",
      "source": ["This is the _beginning_ of the **hard** math analysis"]
    },
    {
      "cell_type": "code",
      "execution_count": 1,
      "id": "a8feff8f",
      "source": ["a = 5"]
    }
  ],
  "metadata": {
    "kernelspec": { "display_name": "Python 3 (ipykernel)", "name": "python3" },
    "language_info": { "name": "python", "version": "3.11.6" }
  },
  "nbformat": 4,
  "nbformat_minor": 5
}
```

So, each notebook is basically a long JSON file with cell metadata, IDs, outputs, execution counts, and other bookkeeping sprinkled everywhere. It’s *sort of* understandable, but definitely **not human-friendly** — and the bigger the notebook, the worse it gets.

Normally this doesn’t matter because Jupyter prettifies it for us. But when git comes into play, it’s game over. Git doesn’t see “cells” — it just sees raw JSON.

For example, let’s revisit the commit where we added our new `z` and `z * 3` cells: [fd204ae](https://github.com/undeMalum/portfolio/commit/fd204aeab3f8aaf1b519cf26e184d589fb72877d).

Here’s what git shows:

```diff
+  {
+   "cell_type": "code",
+   "execution_count": 4,
+   "id": "882cd319-f4e1-4a19-befc-8ffae114f7d1",
+   "metadata": {},
+   "outputs": [],
+   "source": [
+    "z = a + b"
+   ]
+  },
+  {
+   "cell_type": "code",
+   "execution_count": 6,
+   "id": "97318bef-729c-489a-81bc-d8237d10f1ea",
+   "metadata": {},
+   "outputs": [
+    {
+     "data": {
+      "text/plain": [
+       "33"
+      ]
+     },
+     "execution_count": 6,
+     "metadata": {},
+     "output_type": "execute_result"
+    }
+   ],
+   "source": [
+    "z * 3"
+   ]
+  }
```

Yikes. :grimacing:

All we *actually* added was:

```python
z = a + b
z * 3
```

But git thinks we added **37 lines of JSON noise**. Cell IDs, execution counts, output blobs… all this extra fluff drowns out the real change.

This causes serious pain:

* **Unreadable diffs** — reviewing notebook [diffs](https://git-scm.com/docs/git-diff) is nearly impossible
* **Messy merge conflicts** — [merge conflicts](https://git-scm.com/docs/git-merge#_how_conflicts_are_presented) are a nightmare in JSON
* **Broken rendering** — GitHub often chokes or displays confusing diffs
* **Noisy history** — commits are bloated with metadata and output churn

That last one hits especially close to home. In one of my projects, [GHOSTxIRIM](https://github.com/GHOST-Science-Club/tree-classification-irim/blob/main/README.md), we keep a Jupyter Notebook alongside modular Python code. Great for demos — terrible for git.

Case in point: [commit d485dc7](https://github.com/GHOST-Science-Club/tree-classification-irim/commit/d485dc7f5dcb5d0a3768faa7a8314c9b8095828a), where adding multithreading touched ~70 lines of real Python… but the notebook diff showed **-1,701 deletions** and **+1,719 additions**. Absolutely bonkers.

Now imagine trying to attribute individual contributions in a research project under those conditions. One tiny notebook change can rewrite the entire file. Git has no clue what’s meaningful and what’s noise.

So yeah - Jupyter and git don’t exactly make a dream team. Let’s see if marimo can do better.

## Marimo Solution
In [Problem Definition](#problem-definition-1), we saw how Jupyter notebooks are secretly JSON files in disguise.

Marimo takes a very different approach. When you create a new notebook, you get something surprisingly familiar: a `*.py` file (ours is `math_analysis.py`). Yep — marimo notebooks are **just Python**.

Here’s what the inside looks like:

```python
import marimo

__generated_with = "0.16.1"
app = marimo.App(width="medium")


@app.cell
def _():
    import marimo as mo
    return (mo,)


@app.cell(hide_code=True)
def _(mo):
    mo.md(r"""This is the _beginning_ of the **hard** math analysis""")
    return


@app.cell
def _(a):
    b = a + 1
    return (b,)


if __name__ == "__main__":
    app.run()
```

Pretty familiar, right? It’s pure Python: imports, functions, returns, and a main guard. The only twist is marimo’s `@app.cell` decorator that turns these functions into notebook cells.

And that mysterious `import marimo as mo`? Turns out it’s simply how marimo handles things like rendering markdown. Nothing spooky — just neat.

This design unlocks a ton of benefits that JSON-based Jupyter can’t touch — most notably, **git-friendliness**.

To demonstrate, let’s reset marimo to its original setup, commit, and then add two new cells:

<iframe src="./notebooks/git_friendliness/marimo/add_changes.html" width="100%" height="500px"></iframe>  

Here’s the commit diff: [16032cf](https://github.com/undeMalum/portfolio/commit/16032cfdafc83cb66bddc27493b6099eb90f782b)

```diff
+@app.cell
+def _(a, b):
+    z = a + b
+    return (z,)
+
+
+@app.cell
+def _(z):
+    z * 3
+    return
```

And that’s it. :sparkles:

No 37 lines of JSON noise. No mysterious IDs or execution counts. Just clean, readable Python. The diff is **12 lines** long and every line matters. A human can instantly understand the change.

This has huge advantages:

* **Clear code reviews** — reviewers see logic, not metadata
* **Cleaner merge conflicts** — when they happen, they’re in Python, not JSON spaghetti
* **Reliable GitHub rendering** — syntax highlighting just works
* **Meaningful git history** — commits show real code changes, not execution artifacts

Now, you might be wondering: *what about outputs?* Isn’t one of the joys of notebooks seeing results inline?

Good question. Marimo does store outputs — but not mixed in with the source. Instead, outputs and session state live in a separate `__marimo__/session` directory as JSON. Yes, JSON. But crucially:

* The **code** stays clean and diffable in Python
* The **results** are stored separately, so you can still share and reproduce them without rerunning the notebook

This split is what makes marimo far saner for version control. You track meaningful code changes in git history, while still preserving outputs when you need them.

So yeah, marimo and git are basically BFFs, whereas Jupyter and git are more like that toxic couple you wish would just break up already.

With that, we’ve now seen two of marimo’s foundational ideas: **reactivity (via DAGs)** and **Python as the notebook format**. These two pillars are what make marimo’s other features possible.

Next, we’ll put them side by side against Jupyter in one more problem and then some demos. Things are about to get even more interesting.

### Jupyter vs. marimo (Git-Friendliness)

| Aspect               | Jupyter Notebook (`.ipynb`)                            | marimo Notebook (`.py`)                               |
| -------------------- | ------------------------------------------------------ | ----------------------------------------------------- |
| **File format**      | JSON (metadata + code + outputs all mixed together)    | Pure Python (clean source); outputs stored separately |
| **Git diffs**        | Bloated with IDs, execution counts, and metadata noise | Concise, readable Python code changes                 |
| **Merge conflicts**  | Frequent and messy (conflicting JSON structures)       | Rare and understandable (conflicts in Python code)    |
| **GitHub rendering** | Raw JSON diffs are unreadable                          | Python diffs have syntax highlighting                 |
| **Commit history**   | Buried in metadata churn, hard to attribute real work  | Clear reflection of actual code and logic changes     |

# Pain Point #3: Reproducibility
So far, we’ve been circling around reproducibility without naming it outright. Hidden states? Out-of-order execution? Git-unfriendly chaos? All roads lead to the same headache: **can someone else actually rerun this notebook and get the same results?**

Now let’s zoom in on another crucial piece of reproducibility: the **environment**. Specifically, how do we ensure the right dependencies are installed so our notebook actually runs on another machine (or six months later on our own)?

To illustrate, let’s extend our math analysis with something spicier: a function to generate the first *n* Fibonacci numbers and a quick plot.

<iframe src="./notebooks/reproducibility/jupyter/plotting.html" width="100%" height="500px"></iframe>  

The code itself isn’t the focus here (unless you’re into Fibonacci art). What matters is that it pulls in two external Python packages: `pandas` and `matplotlib`.

And that means we’ve entered the land of **[dependency management](https://packaging.python.org/en/latest/tutorials/managing-dependencies)** — notoriously tricky territory for Jupyter, and a perfect lens for comparing it with marimo.

## Problem Definition
First of all, Jupyter **has no built-in mechanism** for menaging its dependencies and virtual environments.

Therefore, we need to rely on other package and project management tools, which, in Python, is a [whole another topic](https://realpython.com/python-virtual-environments-a-primer/#use-third-party-tools) in itself!

However, I distinguish between managing third-party dependencies and declaring those dependencies for a simple reason:

> One thing is to isolate your environment such that it works _locally_, on _your_ computer, and another thing is to present this environment to _others_ so they can work on the project on _their_ computers.

Moreover, in the aforementioned study[^fn] it has been shown that while many Jupyter Notebook users can shoulder their way through setting up the environment locally, many of them fail to even declare the dependencies (in files like `requirements.txt` or `setup.py`), which renders majority of the notebooks unreproducible.

Even worse, majority of notebooks that _do_ declare dependencies have many errors in those dependencies, which still makes it unreproducible!

_I recommend you read the Chapter G of the study[^fn] that adresses the Research Question 7: How reproducible are notebooks?. It's full of different funny problems related to reproducing notebooks!_

Admittedly, the study is a few years old, and a lot has changed in the Python packaging ecosystem, epsecially with the advent of [uv](https://docs.astral.sh/uv) (which BTW I use to manage notebooks for this post). Moreover, this is not really a Jupyter-specific issue, but rather _Python-specific_.

Yet, I'd argue that it shouldn't be so godamn hard and boilerplate to just setup a notebook - which by definition should be easy to use - just to play with some research result, or to get some insights into data, or whatever you want to do with Jupyter!
