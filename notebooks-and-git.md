# Jupyter notebooks and git

A notebook (`.ipynb`) is a JSON file. It holds your code, and also every output, every
plot (as a long base64 string), and the execution count of every cell. Run a notebook
again without changing a line, and git sees a changed file. Change one number, and
`git diff` shows a wall of JSON. When two people have both run the same notebook,
merging their work almost always conflicts.

None of this means you should stop using notebooks or stop committing them. It means a
few habits, and one tool.

## 1. Habits, from your first commit

- **Restart & Run All before you commit** (the ⏩ button). The outputs you commit are
  then real, in order, and reproducible. They are not left over from cells you have
  since deleted.
- **Keep a `.gitignore`** at the top of every repository. See the [example](#a-gitignore-to-start-from)
  below. Never commit `.ipynb_checkpoints/`, virtual environments, or large data and
  output files.
- **Read notebook changes in a viewer that understands notebooks**, not in raw
  `git diff`. On GitHub, open a pull request and look at *Files changed*, which renders
  notebook diffs cell by cell. In VS Code, the Source Control panel does the same.
- **One owner per notebook.** In a team, agree who edits which notebook. Two people
  editing and running the same `.ipynb` is how you get a conflict nobody can read.
- **Move code you reuse into a `.py` file**, and `import` it from your notebooks.
  Functions in a module are easy to diff, test and share. The notebook then stays short:
  it calls those functions and shows the results.

## 2. Jupytext: pair each notebook with a script

[Jupytext](https://jupytext.readthedocs.io) keeps a plain-text copy of each notebook
next to it, and keeps the two in sync. The copy uses the *percent* format:

```python
# %% [markdown]
# ## Rabbits and foxes

# %%
k1 = 0.015
k2 = 0.00004
```

Every cell becomes a `# %%` block, and markdown cells become comments. There are no
outputs. So when you change a line, `git diff` shows that one line. When two people change
different cells, git merges the `.py` without any help.

The `.py` is also a working Python script. `python mynotebook.py` runs the whole
notebook top to bottom. So it can be submitted as a batch job on Explorer.

### Turn it on for a repository

Put a file called `jupytext.toml` in the top folder of the repository, containing one
line:

```toml
formats = "ipynb,py:percent"
```

Commit it. Every notebook in that repository now gets a paired `.py` the next time it is
saved, and so does everyone who clones the repository. You don't need to change anything
per notebook, and there is no personal config to forget.

(For a single notebook outside such a repository:
`jupytext --set-formats ipynb,py:percent mynotebook.ipynb`.)

### Install it

Jupytext has to be installed where the **Jupyter program** runs. That is not
necessarily where your kernel runs. It is the part of Jupyter that saves files, so
installing it into your kernel's environment alone does nothing.

**On your own computer (JupyterLab or Jupyter Notebook).** Install it into the
environment you start `jupyter lab` from, then restart Jupyter:

```bash
conda activate chme5137
conda install -c conda-forge jupytext
```

(or `pip install jupytext` in a venv).

**On Explorer (Open OnDemand JupyterLab).** OOD's JupyterLab runs from the shared
Anaconda module, not from your own environment. So install jupytext into your
user folder for that module. Do it from a terminal **on a compute node**, for example the
JupyterLab *Terminal* in an OOD session. The login nodes can kill `pip` partway through.

```bash
module load anaconda3/2024.06
python -m pip install --user jupytext
```

Then **end the OOD session and start a new one**. Jupyter only loads extensions at
startup. Your notebooks can still use your own `CHME5137` kernel, because the kernel choice
is separate from this.

Explorer's JupyterLab is a little too old for jupytext's *menu*, so you won't see
the Jupytext commands in the File menu or command palette. That's fine, because the
`jupytext.toml` file does the pairing without them. You can check it is working by saving
a notebook and seeing the `.py` appear beside it.

**In VS Code.** VS Code saves notebooks itself, so it needs the
[**Jupytext Sync** extension](https://marketplace.visualstudio.com/items?itemName=caenrigen.jupytext-sync)
(`caenrigen.jupytext-sync`):

1. Install `jupytext` into the Python environment VS Code uses for your project, with
   `conda install -c conda-forge jupytext` or `pip install jupytext`.
2. Install the extension from the Extensions panel (search for *jupytext-sync*).
3. Save a notebook in a repository that has a `jupytext.toml`, and the `.py` appears. After
   that, saving either file updates the other.

If it can't find jupytext, run **Jupytext: Locate Python and Jupytext** from the command
palette to see which Python it picked. You can point it at the right one with the
`jupytextSync.pythonExecutable` setting.

Without the extension, run `jupytext --sync *.ipynb` before each commit.

### What to commit

**Commit both files.** Read, review and resolve conflicts in the `.py`. The `.ipynb`
keeps your plots, so they show up on GitHub.

If your team keeps colliding on the `.ipynb` files, even with one owner per notebook,
add `*.ipynb` to `.gitignore` and treat the `.py` as the real file. Opening the `.py` as a
notebook recreates the `.ipynb`, without outputs until you run it. From a terminal, run
`jupytext --sync mynotebook.py`. In VS Code, use **Open paired Notebook via Jupytext**.

### After a `git pull`

If a pull changed the `.py`, just open the notebook. Jupytext notices the `.py` is
newer, takes the code from it, and keeps the outputs from the `.ipynb`. Then
Restart & Run All, so the outputs match the code again.

### When a merge conflicts

Usually the `.py` merges cleanly and only the `.ipynb` conflicts. If the `.py` also
conflicts, fix it first, like any other text file.

1. Resolve the `.py`, and check it looks right.
2. Take either side of the `.ipynb`. It doesn't matter which, because it is about to be rebuilt:
   ```bash
   git checkout --theirs mynotebook.ipynb
   ```
3. Rebuild the notebook's code from the `.py`, keeping whatever outputs it can:
   ```bash
   jupytext --to ipynb --update mynotebook.py
   ```
4. Open it, **Restart & Run All**, save, then `git add` both files and commit.

## 3. VS Code alternative: skip `.ipynb` altogether

VS Code can run a percent-format `.py` file directly. Each `# %%` line gets a
*Run Cell* link, and the output and plots appear in the *Interactive Window* beside
the code. So you can work in `.py` files the whole time and have no JSON to worry
about. It is the same format jupytext writes, so jupytext can turn such a file into a
notebook for anyone who wants one.

## 4. Other tools you may meet

- **[nbstripout](https://github.com/kynan/nbstripout)** strips outputs from notebooks as
  they are committed. Diffs get small, but your plots never reach GitHub. It has to be
  installed separately in every clone.
- **[nbdime](https://nbdime.readthedocs.io)** gives `git diff` and `git merge` a
  notebook-aware mode (`nbdime config-git --enable`), and a side-by-side diff in the browser.
- **[jupyterlab-git](https://github.com/jupyterlab/jupyterlab-git)** is a git panel inside
  JupyterLab.
- **[marimo](https://marimo.io)** is a different kind of notebook. It is stored as plain Python
  and reruns dependent cells automatically. Worth a look for a new project, but not
  something to switch to halfway through this course.
- **The old "post-save hook"**, which you may find in older course material, wrote
  a `.py` and an `.html` copy on every save. Those copies were one-way, so they couldn't be
  merged back. It has been replaced by jupytext.

## A `.gitignore` to start from

```gitignore
# Jupyter
.ipynb_checkpoints/

# Python
__pycache__/
*.pyc

# Environments
venv/
.venv/

# OS clutter
.DS_Store
Thumbs.db

# Big or generated files: adjust for your project
*.h5
*.npz
data/raw/
results/

# SLURM job output on Explorer
slurm-*.out
```

GitHub also keeps a longer [Python template](https://github.com/github/gitignore/blob/main/Python.gitignore).

---

*About this page:* written for CHME 5137, Fall 2026, replacing an older handout on
post-save hooks. *Use of AI:* drafted by Claude Code (Opus 5.5) and checked by
Prof. West. The Explorer and merge-conflict steps were tested on Explorer and locally on
2026-09-28.
