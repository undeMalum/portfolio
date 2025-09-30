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

So how does marimo pull this off? The secret is its reliance on a **DAG (Directed Acyclic Graph)** — [yes, the graph theory kind](https://en.wikipedia.org/wiki/Directed_acyclic_graph).

Marimo builds a DAG of cell dependencies. When a variable changes, every dependent cell automatically re-runs. In our case, changing `a` triggers both `b = a + 1` and `a + b`.

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

So with one mechanism, we solve two problems at once:

* No incorrect results from stale values
* No “zombie variables” persisting after deletion

But DAGs give us more than just protection against hidden states. Because dependencies are explicit, marimo notebooks don’t care about **cell order**. You can rearrange cells however you like without breaking execution — something Jupyter can’t guarantee.

This eliminates a whole class of headaches around execution order and brings marimo notebooks closer to how Python scripts naturally run: clear, deterministic, and reproducible.

*In fact*, marimo notebooks **are** Python scripts. But we’ll get to that in the next section.

### Jupyter vs. marimo (Execution & States)

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
Before we go back to talking about git, let's talk a little about how Jupyter Notebooks are _stored_.

When we create a notebook with Jupyter, a file with the `*.ipynb` extension pops up (in our case, we have `math_analysis.ipynb` file).

However, contrary to what you might think, `*.ipynb` files are _not_ plain Python files with a strange extension, but are actually **JSON** files under the hood! What a surprise!

This means that if we open the `math_analysis.ipynb` file, we're going to see something like this:

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

As you can see, we have a list of cells, each cell has some keys, denoting the _type_ of the cell (e.g markdown), its id, output, etc., and we also have some metedata about the language used, the notebook itself and a bunch of other stuff.

This is _not_ a full notebook, just its simplified and more comprehensible verion, but you should get the general idea.

So, although this form of representing the notebook is pretty understandable, it is certainly NOT human-readable, especially with huge notebooks and cells with multiple lines of code.

Admittedly, this is not a problem for developing the code - after all, we use Jupyter engine to present this file in a nice-looking form. 

However, it is not so nice when it comes to working with git as git sees only the raw JSON file.

To exemplify this, let's go back to the commit in which we added our new cells: [fd204ae](https://github.com/undeMalum/portfolio/commit/fd204aeab3f8aaf1b519cf26e184d589fb72877d).

Here's what git shows us for this change:

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

Yikes! :grimacing: 

What we _meant_ to add was simply:

```python
z = a + b
z * 3
```

But what git _actually_ sees is **37 lines of JSON metadata** including cell IDs, execution counts, output formatting, and all sorts of internal bookkeeping that has nothing to do with our actual code changes.

This creates several problems:

* Difficulty in reviewing notebooks [diffs]()
* Painful [merge conflicts]()
* Problems with rendering on git platfroms GitHub
* Cumbersome collaboration caused by frequent changes in metadata and output cells

The last one is particularly painful to me because I encountered it in one of my projects I am involved in titled [GHOSTxIRIM](https://github.com/GHOST-Science-Club/tree-classification-irim/blob/main/README.md).

In this project, we maintain a Jupyter Notebook alongside our modular Python code so others can play more easily with the project. However, applying git on this notebook is a pain in the ass, which you can easily see in the commit [d485dc7](https://github.com/GHOST-Science-Club/tree-classification-irim/commit/d485dc7f5dcb5d0a3768faa7a8314c9b8095828a) that added multithreading in data loaders.

While changes in plain Python files + some configuration files added up to ~70 lines, the updated and re-run notebook yielded **-1,701** deletions and **+1,719** additions! That's insane!

Imagine now trying to determine everyobdy's individual contributions in a research when changing/adding a few cells in a notebooks, causes **_all_** cells within this notebooks to be updated because of updating metadata.

As you can infer from our math analysis and my real-life project, Jupyter Notebooks do _not_ integrate well with git.

Having said that, let's see if marimo offers a better git integration.

## Marimo Solution
