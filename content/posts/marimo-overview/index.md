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

* Reviewing notebook [diffs](https://git-scm.com/docs/git-diff) is nearly impossible
* [Merge conflicts](https://git-scm.com/docs/git-merge#_how_conflicts_are_presented) are a nightmare
* GitHub rendering often breaks in confusing ways
* Collaboration suffers because metadata and outputs constantly churn

That last one hits especially close to home. In one of my projects, [GHOSTxIRIM](https://github.com/GHOST-Science-Club/tree-classification-irim/blob/main/README.md), we keep a Jupyter Notebook alongside modular Python code. Great for demos — terrible for git.

Case in point: [commit d485dc7](https://github.com/GHOST-Science-Club/tree-classification-irim/commit/d485dc7f5dcb5d0a3768faa7a8314c9b8095828a), where adding multithreading touched ~70 lines of real Python… but the notebook diff showed **-1,701 deletions** and **+1,719 additions**. Absolutely bonkers.

Now imagine trying to attribute individual contributions in a research project under those conditions. One tiny notebook change can rewrite the entire file. Git has no clue what’s meaningful and what’s noise.

So yeah - Jupyter and git don’t exactly make a dream team. Let’s see if marimo can do better.

## Marimo Solution
In [Problem Definition](#problem-definition-1) we kicked off with discussing how jupyter notebooks are acutally stored.

Let's do the same with marimo.

When we create a new notebook with marimo, we, surprisingly, get a file with a suspiciously familiar extension - `*.py` (in our case - `math_analysis.py`).

And to satisfy your curiosity - yes, marimo notebooks _are_ pure Python files under the hood as I've touched upon in the end of the [Marimo Solution](#marimo-solution) for the [Hidden States](#pain-point-1-hidden-states) chapter.

Let's now inspect the insides of the notebook for our analysis:

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

...

@app.cell
def _(a):
    b = a + 1
    return (b,)

...

if __name__ == "__main__":
    app.run()
```

That looks oddly familiar, provided that you know a bit or two about Python!

Cells are classes with parameters and return values, there are some importa, ifs, pretty cool stuff.

This design offers enourmous usage opportunites, unattainable for JSON-based Jupyter - among them, notably, a great git integration (I will mention more in the wrap up).

To demonstrate this, let's make sure marimo is reset to default so restore original marimo setup and commit the changes:

<iframe src="./notebooks/hidden_states/marimo/initial_run.html" width="100%" height="350px">
</iframe>

```bash
git add content/posts/marimo-overview/math_analysis.ipynb
git commit -m "Add changes to marimo and run it"
git push
```

After we made sure marimo is brought back to its original settings and that we have a commit remmembering these changes, we can add two new cells with `z` and commit it with git:

<iframe src="./notebooks/git_friendliness/marimo/add_changes.html" width="100%" height="500px">
</iframe>

```bash
git add content/posts/marimo-overview/math_analysis.ipynb
git commit -m "Reset marimo to default setup and run it"
git push
```

Cool, changes are made and committed. Let's inspect the changes for the commit [16032cf](https://github.com/undeMalum/portfolio/commit/16032cfdafc83cb66bddc27493b6099eb90f782b) to learn if it's more tractable for a human eye:

Here's what git shows us for this change:

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

Beautiful! :sparkles:

What git _actually_ sees is almost exactly the two lines of Python code — just wrapped in marimo's clean function decorators. No mysterious IDs, no execution counts, no output bloat.

The diff is **12 lines** of pure, readable Python instead of 37 lines of JSON noise. Every line serves a purpose, and a human can instantly understand what changed.

This creates several advantages:

* **Clear code reviews** — reviewers see actual logic, not metadata
* **Clean merge conflicts** — when they happen, they're in readable Python
* **Reliable GitHub rendering** — Python syntax highlighting just works
* **Meaningful git history** — commits reflect real code changes, not execution artifacts

No more guessing what actually changed behind the JSON curtain. With marimo, git diffs tell the real story.

However, some inquisitive readers might ask what about the cell output!? After all, people find the ability to have annotations, code, _and_ results all displayed a big Jupyter Notebook advantage - with some arguing that it improves the reproducibility of the notebooks[^fn].

Fear not, marimo _does_ store the cells outputs and the state of the notebook inside the `__marimo__` directory in the `session` subdirectory where each notebook have all of its outputs and states saved as a JSON.

I know I sound hypocritical when I say that marimo uses JSON to store the notebooks state, but it still keeps the distinction between what changed _inside_ the cells rather the in their _output_, which is far more useful for every project utlizing git, also in research because you still have the whole history of what's changed and by whom in a easily digestable format, while having the opportunity to share the results without rerunning the notebook, which can be in JSON because you don't need it in a pretty format as it's all meant to be displayed by the notebook nonetheless.

Uff, I wandered of from the main point, that is the fact that marimo works far better with git than Jupyter Notebook does. No question about it.

Moreover, having now explained both the **reactive** and **python script** nature of marimo I have explained two foundational features of marimo, which other features of marimo are built upon.

We'll get a glimpse of that in the last section about contrasting marimo with Jupyter Notebook, but I'll try to captivate it as best as I can with the demos. So things now really start to get interesting. Stay tuned!
