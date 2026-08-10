# Git and GitHub for Analytics, Data Science, and Data Engineering

**A self-study beginner's guide to version control, collaboration, and a reliable source of truth**

---

## How to Use This Guide

This guide is designed for self-study and assumes no prior Git experience. You should be comfortable locating files and opening a terminal, but you do not need to know Git commands in advance.

The guide has two practical tracks:

- **Self-study:** You will use a supplied project folder and work entirely on your computer. No GitHub repository creation permission is required.
- **Organizational work:** You will see how the same Git concepts apply when working with an IT-provisioned GitHub Enterprise repository, including cloning, pushing, pull requests, review, and merging.

VS Code is used as the primary working environment. Commands are shown explicitly so you understand what Git is doing, while VS Code equivalents are introduced where they are useful.

> **Goal:** Do not try to memorize every command on the first pass. Concentrate on understanding the workflow: what is accepted, where you make a proposed change, how you record it, and how it becomes part of the team's source of truth.

---

# Part 1 — Why Git?

Git is often introduced as *a version-control system*. That definition is accurate, but it does not explain why an analyst, data scientist, or data engineer should care.

The easier place to start is with a problem most teams already recognize.

Most teams already practice some form of version control, even if they do not call it version control.

You may have encountered files such as:

```text
customer_model.py
customer_model_v2.py
customer_model_final.py
customer_model_final_updated.py
customer_model_final_FINAL.py
customer_model_final_FINAL_v3.py
```

Or perhaps:

```text
Quarterly Analysis/
    old/
    current/
    new/
    archive/
    archive_old/
```

These approaches are attempts to answer reasonable questions:

- Which version is current?
- What did we change?
- Can I safely make an experimental change?
- Can I recover the previous version?
- Which version produced a particular result?
- What did another analyst change?
- Which version should the team use?

The problem is that filenames and folders do not answer these questions reliably.

Git does.

**Git is a system for deliberately recording and managing changes to files over time.**

Instead of creating another copy of a file every time something changes, we keep the file under Git's control and let Git maintain its history.

Instead of this:

```text
scoring.py
scoring_v2.py
scoring_v3.py
scoring_final.py
scoring_final_fixed.py
```

we can continue working with:

```text
scoring.py
```

while Git maintains a history such as:

```text
August 3    Initial customer scoring logic
August 5    Add missing-value handling
August 7    Add income feature
August 8    Correct scoring threshold
```

More importantly, Git can tell us **exactly what changed between those versions**.

This becomes increasingly important when analytical products are developed by teams rather than individuals.

---

## Why This Matters for Analytical Work

Version control is sometimes presented as something software developers use to manage application code.

That description is much too narrow.

Consider some common questions in analytics, data science, and data engineering.

### For an analytical product

Which code produced the results presented last month?

### For a machine-learning model

When was a feature added, and what exactly changed in the model code?

### For a data pipeline

Why did this month's output differ from last month's output?

### For a production process

What is the currently accepted version of the code?

### For collaboration

What exactly did another analyst change?

### For experimentation

Can I modify this code without breaking the version everyone else depends on?

### For review

Can someone examine my proposed changes without manually comparing two complete files?

### For recovery

Can we return to a previous working version if a change causes a problem?

Git helps answer all of these questions.

The value of Git is therefore not simply:

> "Git stores versions of code."

Its larger value is that it provides a **controlled history of how an analytical product changes**.

Used together with GitHub, it also provides a structured way to propose, review, discuss, and accept those changes.

---

# Part 2 — Understanding Git, GitHub, and VS Code

These terms are related, but they are not interchangeable.

## Git

**Git is the version-control system.**

Git tracks files and their history.

Git can operate on your computer even without GitHub.

## GitHub

**GitHub provides a shared location for Git repositories and tools for collaboration.**

Our organization uses GitHub Enterprise.

GitHub provides capabilities such as:

- centrally hosted repositories;
- access control;
- shared branches;
- pull requests;
- code review;
- change history;
- collaboration among team members.

A useful simplified distinction is:

> **Git manages versions. GitHub helps teams share and collaborate on those versions.**

## Visual Studio Code

**Visual Studio Code (VS Code) is the environment in which we can work with our files and interact with Git.**

VS Code provides both:

- an integrated terminal where Git commands can be entered; and
- a graphical Source Control interface for performing many of the same Git operations.

For example, you can create a Git commit from the terminal:

```bash
git commit -m "Add missing-value validation"
```

or use VS Code's Source Control interface to enter the commit message and select **Commit**.

These are not separate version-control systems.

**VS Code is providing a graphical interface to Git.**

This guide teaches both because understanding the underlying Git operations makes the graphical interface much easier to understand.

---

## A Note for Jupyter Notebook Users

Using Git does not require abandoning notebooks.

VS Code supports Jupyter notebooks (`.ipynb` files). Notebooks can be opened and executed interactively in VS Code, including running individual cells.

A project might contain both notebooks and conventional Python files:

```text
customer-risk-analysis/
│
├── notebooks/
│   └── exploratory_analysis.ipynb
│
├── src/
│   └── scoring.py
│
└── README.md
```

All of these files can be managed in the same Git repository.

Notebook files do present some special version-control considerations because their underlying file format contains more than the code visible in each cell. Those considerations are beyond the scope of this introductory guide.

The important point for now is:

> **Notebook-driven analytical work can still use Git, GitHub, and VS Code.**

---

## The Fundamental Git Mental Model

Before learning commands, understand the basic workflow.

A useful simplified model is:

```text
GitHub Enterprise
       │
       │ clone
       ▼
Your Local Repository
       │
       │ edit files
       ▼
Your Working Files
       │
       │ stage
       ▼
Changes Selected for Commit
       │
       │ commit
       ▼
Local Git History
       │
       │ push
       ▼
GitHub Enterprise
```

This introduces an important concept:

**Your computer and GitHub are not automatically the same thing.**

You can change a file on your computer without changing GitHub.

You can even commit that change to your local Git repository without changing GitHub.

A `push` sends your local Git commits to GitHub.

This distinction explains several Git commands that otherwise seem unnecessary.

---

## What Is a Repository?

A **repository**, commonly called a **repo**, is a project whose files and history are managed by Git.

For example:

```text
customer-risk-analysis/
│
├── README.md
├── notebooks/
│   └── analysis.ipynb
├── src/
│   └── scoring.py
└── tests/
    └── test_scoring.py
```

The repository contains the project's files.

But Git also maintains the history of changes made to those files.

This is an important difference between a Git repository and an ordinary shared folder.

A repository is not simply a place where files are stored.

It represents both:

1. **the project as it exists now**, and
2. **the recorded history of how it reached that state.**

---

# Part 3 — Learn Git Locally: Self-Study Exercise

Before beginning, you need:

- VS Code;
- Git installed and available from the terminal;
- Python if you want to run the small practice code; and
- the supplied `customer-risk-analysis` practice folder.

You do **not** need permission to create a GitHub repository.

> **Practice safely:** Everything in this part happens locally. You can learn the mechanics without changing an organizational repository.



In organizational work, repositories are provisioned through the organization's GitHub Enterprise process. Individual users may not be able to create a new GitHub repository themselves.

That should not prevent you from learning Git.

The first part of this self-study exercise happens entirely on your computer. You will begin with an ordinary project folder and turn it into a local Git repository.

The supplied practice project has this structure:

```text
customer-risk-analysis/
│
├── README.md
├── data/
│   └── sample_customers.csv
├── notebooks/
│   └── analysis.ipynb
├── src/
│   └── scoring.py
├── tests/
│   └── test_scoring.py
├── requirements.txt
└── .gitignore
```

Open the `customer-risk-analysis` folder in VS Code. Then open the integrated terminal.

Before Git is initialized, this is simply a folder containing project files. Git is not yet tracking its history.

Run:

```bash
git init
```

## What did `git init` accomplish?

`git init` creates the Git metadata needed to manage the folder as a repository.

The visible project files do not need to move anywhere. Instead, Git begins maintaining repository information inside the project.

Now run:

```bash
git status
```

Git will report the state of the new repository. Because this is a new repository, the supplied project files have not yet been recorded in Git history.

This distinction is important:

> **A folder becomes a local Git repository when Git is initialized for it. GitHub is not required for local version control.**

### VS Code

Once Git has been initialized, VS Code's Source Control view will recognize the repository and display files that Git sees as new or changed.

> **SCREENSHOT PLACEHOLDER — VS Code: Initialize Repository / Source Control**  
> Show the supplied project opened in VS Code and the Source Control view after Git initialization.

---

## Recording the supplied project as the starting point

Before developing a new feature, record the supplied project as the baseline version.

Review the repository state:

```bash
git status
```

Stage the supplied project files:

```bash
git add .
```

Check the state again:

```bash
git status
```

Then create the first commit:

```bash
git commit -m "Add initial customer risk analysis project"
```

This commit establishes the starting point for the exercise.

Run:

```bash
git log --oneline
```

You should now see the initial commit in the repository history.

If the initial branch is not named `main`, rename the current branch:

```bash
git branch -M main
```

For the remainder of this guide, `main` represents the accepted baseline version of the project.

### What have we accomplished?

You started with an ordinary folder and created:

```text
Project files
     │
     │ git init
     ▼
Local Git repository
     │
     │ git add + git commit
     ▼
Recorded baseline on main
```

Nothing has been sent to GitHub. Everything so far exists only on your computer.

Later in the guide, we will connect this local Git mental model to the organization's GitHub Enterprise workflow.

---

## Your Most Useful Diagnostic Command: `git status`

After opening the repository, one of the most useful commands you can learn is:

```bash
git status
```

`git status` tells you about the current state of your working repository.

For example, it can tell you:

- which branch you are on;
- which files have changed;
- which files are untracked;
- which changes are staged;
- whether your local branch differs from its remote counterpart.

## Why does `git status` matter?

Git is stateful. What a command will do often depends on what has already happened.

When you are unsure what Git thinks is happening, use:

```bash
git status
```

A useful beginner rule is:

> **When in doubt, start with `git status`.**

Do not respond to Git confusion by immediately searching for powerful commands to undo or reset things.

First determine the state of the repository.

---

## `main`: The Team's Accepted Source of Truth

Most repositories have a primary branch called `main`.

For this introductory workflow, we will treat `main` as:

> **The accepted version of the project.**

Suppose `main` contains a working data pipeline.

You are asked to add a new validation function.

You *could* immediately change the files on `main`.

But consider what that means.

The team's accepted version and your unfinished experimental work would now be mixed together.

What happens if:

- your function is incomplete?
- the code does not work?
- you need three days to finish it?
- another person needs the current production version?
- your approach is reviewed and rejected?
- two people are developing different features simultaneously?

This is the problem branches solve.

---

## Branches: A Safe Place to Make a Change

A **branch** provides a separate line of development.

Suppose the current accepted project is:

```text
main
  │
  A
  │
  B
  │
  C
```

You need to develop a new validation feature.

You create a branch:

```text
main
  │
  A
  │
  B
  │
  C
   \
    \ feature/add-validation
```

You can now develop the feature on that branch while `main` remains unchanged.

As you work:

```text
main
  │
  A
  │
  B
  │
  C
   \
    D  Add validation function
    │
    E  Add validation tests
    │
    F  Handle null values
```

`main` still points to the team's accepted version.

Your branch contains your proposed changes.

This is one of the most important ideas in Git:

> **A branch lets you change the project without immediately changing the team's accepted version.**

---

## A Simple Branching Strategy

Git supports many sophisticated branching strategies.

We do not need them to begin using Git effectively.

A simple strategy is:

```text
main
  │
  ├── feature/add-validation
  │
  ├── feature/new-model-feature
  │
  └── fix/missing-value-error
```

The principle is straightforward:

### `main`

Contains the accepted version of the project.

### Short-lived working branches

Contain proposed changes.

Examples:

```text
feature/add-income-variable
feature/add-validation
fix/null-handling
fix/incorrect-date-filter
```

Once the work is complete and accepted, it is merged into `main`.

The temporary branch can then be deleted.

This avoids permanent collections of branches whose purposes are no longer clear.

---

## Creating a Branch

Suppose you have been asked to add validation to a scoring pipeline.

Create a branch:

```bash
git switch -c feature/add-validation
```

The `-c` means create a new branch.

This command both:

1. creates `feature/add-validation`; and
2. switches your working environment to that branch.

Check where you are:

```bash
git status
```

or:

```bash
git branch
```

`git branch` lists local branches and marks the currently checked-out branch with an asterisk.

For example:

```text
* feature/add-validation
  main
```

### VS Code

VS Code displays the current branch in its interface and allows branches to be created and selected without entering the command manually.

> **SCREENSHOT PLACEHOLDER — VS Code: Current Branch**  
> Show the current branch and the option to create or switch branches.

---

## Make a Change

Suppose `src/scoring.py` contains:

```python
def calculate_score(income, debt):
    return income - debt
```

We want to add a simple input-validation function:

```python
def validate_inputs(income, debt):
    if income is None or debt is None:
        raise ValueError("Income and debt are required.")
```

Save the file.

Git now detects that the file differs from the last recorded version.

Run:

```bash
git status
```

Git will report that `src/scoring.py` has been modified.

Notice what has *not* happened.

You have not created a Git version simply by saving the file.

You have only changed your working copy.

---

## See Exactly What Changed: `git diff`

Before recording the change, you can ask Git to show exactly what is different:

```bash
git diff
```

This displays the differences between the current working files and the version Git previously recorded.

## Why does `diff` matter?

Without Git, reviewing a change may involve opening `scoring_old.py` and `scoring_new_final.py` and manually trying to determine what is different.

Git already knows.

A diff allows you to review changes at the line level.

This becomes particularly valuable during code review.

### VS Code

Selecting a changed file in the Source Control view opens a visual comparison showing removed and added lines.

> **SCREENSHOT PLACEHOLDER — VS Code: File Diff**  
> Show the original and modified versions of `scoring.py` with the changed lines highlighted.

---

## Staging: Choosing What Belongs in the Next Commit

After making changes, Git does not automatically assume that every changed file belongs in the same recorded version.

You first **stage** the changes you intend to commit.

For example:

```bash
git add src/scoring.py
```

Now run:

```bash
git status
```

The file will appear under the changes to be committed.

## Why does staging exist?

Imagine that during one work session you:

- modify the scoring function;
- update the README;
- create an unrelated experimental notebook.

Those changes may not logically belong together.

Staging lets you say:

> **These are the changes I want included in my next recorded checkpoint.**

You can stage multiple files:

```bash
git add src/scoring.py tests/test_scoring.py
```

A common shortcut is:

```bash
git add .
```

which stages changes beneath the current directory.

For beginners, however, explicitly reviewing and staging the intended files is often safer than automatically staging everything.

### VS Code

In Source Control, changed files appear under **Changes**.

Files can be staged using the `+` control and will then appear under **Staged Changes**.

> **SCREENSHOT PLACEHOLDER — VS Code: Staging Changes**  
> Show a modified file first under Changes and then under Staged Changes.

---

## Commits: Recording a Meaningful Point in History

Suppose the validation function works and you want to permanently record this point in the project's history.

Create a commit:

```bash
git commit -m "Add missing-value validation"
```

A **commit** is a recorded checkpoint in the repository's history.

The message describes the purpose of the change.

Good:

```text
Add missing-value validation
```

Useful:

```text
Correct date filter for quarterly records
```

Useful:

```text
Add debt-to-income feature to scoring model
```

Less useful:

```text
changes
update
stuff
```

The goal is not to write an essay.

The goal is to leave enough context that someone—including you six months from now—can understand why this point exists in the project's history.

> **A commit says: These related changes represent a meaningful state of the project worth recording.**

---

## Saving a File Is Not the Same as Committing It

This distinction is important.

When you save a file, you update the file on your computer.

When you stage it with `git add`, you select the change for the next commit.

When you commit it with `git commit`, you record that selected change in your local Git history.

When you push it with `git push`, you send your local commits to the remote repository on GitHub.

These are separate operations:

```text
SAVE
  │
  ▼
File changed locally
  │
  │ git add
  ▼
Change staged
  │
  │ git commit
  ▼
Change recorded in local Git history
  │
  │ git push
  ▼
Commit sent to GitHub
```

Understanding these states eliminates much of the mystery surrounding Git.

---

## Viewing History: `git log`

To inspect recorded history:

```bash
git log
```

Git displays commits and information such as:

- commit identifier;
- author;
- date;
- commit message.

A more compact view is:

```bash
git log --oneline
```

You might see:

```text
8e71d4a Add missing-value validation
44a39c2 Add customer scoring function
92c81b1 Initial project structure
```

Each commit has a unique identifier.

This means discussions can refer to an exact recorded version rather than:

> "I think it was the version from Tuesday before we changed the model."

---

## Local Work Versus GitHub: Why `push` Exists

At this point, the commit exists on your computer.

It does not necessarily exist on GitHub yet.

This is a fundamental distinction:

```text
YOUR COMPUTER                 GITHUB

feature/add-validation        feature/add-validation
       │                              │
       D                              ?
```

To publish your branch and its commits to GitHub, push it.

The first time you push a new branch, you can use:

```bash
git push -u origin feature/add-validation
```

Here:

- `push` sends commits to the remote repository;
- `origin` is the conventional name Git usually gives the remote repository you cloned;
- `feature/add-validation` is the branch being pushed;
- `-u` establishes the relationship between your local branch and its remote counterpart.

After that relationship has been established, later pushes can usually be performed with:

```bash
git push
```

### VS Code

VS Code provides controls to publish, push, or synchronize the branch.

> **SCREENSHOT PLACEHOLDER — VS Code: Publish/Push Branch**  
> Show the feature branch being published to the organization's GitHub Enterprise repository.

---

## A Branch on GitHub Is Still Only a Proposal

You have:

1. created a branch;
2. changed code;
3. tested the change;
4. committed it;
5. pushed the branch to GitHub.

But you have **not changed `main`**.

That is intentional.

Your branch represents:

> **My proposed change to the project.**

Now the team needs a controlled way to decide whether that proposed change should become part of the accepted version.

That is the purpose of a **pull request**.

---

## Pull Requests: From "My Change" to "Our Accepted Change"

A **pull request**, commonly called a **PR**, proposes that changes from one branch be incorporated into another branch.

In our simple workflow:

```text
feature/add-validation
          │
          │ Pull Request
          ▼
        Review
          │
          ▼
        Merge
          │
          ▼
         main
```

A pull request provides a place to:

- explain the purpose of the change;
- see exactly which files changed;
- inspect line-level differences;
- discuss the implementation;
- request modifications;
- document review;
- approve the change;
- merge the accepted work.

This makes a pull request much more than an administrative approval step.

It provides a controlled transition from:

> **my proposed change**

to:

> **our accepted version**

For analytical work, a pull request might document:

> Add missing-value validation to prevent customer records with incomplete income or debt fields from entering the scoring process.

A reviewer can inspect exactly what changed before it becomes part of `main`.

> **SCREENSHOT PLACEHOLDER — GitHub Enterprise: Create Pull Request**  
> Show the feature branch being proposed for merge into `main`.

> **SCREENSHOT PLACEHOLDER — GitHub Enterprise: PR Diff**  
> Show the Files Changed view and reviewer controls.

---

## Merging: Accepting the Change

Once the pull request has been reviewed and approved according to the team's process, it can be merged.

Before the merge:

```text
main
  │
  A
  │
  B
  │
  C
   \
    D  Add missing-value validation
    │
    E  Add validation tests
```

After the feature is accepted into `main`, the accepted feature is part of the primary branch.

The exact history can vary depending on the repository's configured merge method, but the important beginner concept remains the same:

**The accepted feature is now part of `main`.**

The feature branch has served its purpose and can normally be deleted.

---

## Getting the Newly Accepted Version: `git pull`

Something important has now happened.

GitHub's `main` has changed.

Your computer may still have an older local copy of `main`.

Switch back to `main`:

```bash
git switch main
```

Then retrieve the latest changes:

```bash
git pull
```

Your local `main` is now brought up to date with the remote version.

> **Push sends your commits to the remote repository. Pull brings remote changes into your local branch.**

---

# Part 5 — The Everyday Team Workflow

Start from `main`:

```bash
git switch main
git pull
```

Create a branch for the work:

```bash
git switch -c feature/add-validation
```

Make and test your changes.

Inspect them:

```bash
git status
git diff
```

Stage the intended files:

```bash
git add src/scoring.py
```

Commit:

```bash
git commit -m "Add missing-value validation"
```

Push the branch:

```bash
git push -u origin feature/add-validation
```

Create a pull request in GitHub Enterprise.

Have the change reviewed.

Merge the accepted change into `main`.

Return locally to `main` and update it:

```bash
git switch main
git pull
```

Then begin the next piece of work from the updated `main`.

---

## Guided Exercise: A Complete Local Git Workflow

This exercise is designed to be completed independently. It does **not** require permission to create a repository in GitHub Enterprise.

## Scenario

Your team maintains a simple customer-risk scoring project.

You have already initialized the supplied project as a local Git repository and recorded its starting state on `main`.

You have been assigned a new task:

> Add validation so that records missing required inputs cannot be scored.

You will develop this change on a feature branch without changing the accepted version on `main`.

## Step 1 — Confirm the starting state

In the VS Code terminal, run:

```bash
git status
```

Confirm that you are on `main` and that there are no unexpected working changes.

Then inspect the recorded history:

```bash
git log --oneline
```

You should see the initial project commit created earlier.

**Why?** Before beginning new work, establish where you are and what Git has already recorded.

## Step 2 — Create a feature branch

Run:

```bash
git switch -c feature/add-validation
```

Then:

```bash
git status
```

**Why?** The validation feature is proposed work. `main` should continue to represent the accepted baseline while you develop it.

## Step 3 — Modify the code

Suppose `src/scoring.py` initially contains:

```python
def calculate_score(income, debt):
    return income - debt
```

Change it to:

```python
def validate_inputs(income, debt):
    if income is None or debt is None:
        raise ValueError("Income and debt are required.")


def calculate_score(income, debt):
    validate_inputs(income, debt)
    return income - debt
```

Save the file.

## Step 4 — Ask Git what changed

Run:

```bash
git status
```

Git should report that `src/scoring.py` has been modified.

**What does this mean?** Git sees a difference between your working file and the last committed version, but the change has not yet been recorded in history.

## Step 5 — Inspect the actual change

Run:

```bash
git diff
```

Review the lines that were added or changed.

**Why?** Before recording a change, verify what you actually changed. VS Code can display the same comparison visually from the Source Control view.

## Step 6 — Stage the change

Run:

```bash
git add src/scoring.py
```

Then:

```bash
git status
```

The file should now appear as a change to be committed.

**What happened?** You selected this change for inclusion in the next commit.

## Step 7 — Commit the feature

Run:

```bash
git commit -m "Add required input validation"
```

Then inspect the history:

```bash
git log --oneline
```

You should now see both the initial project commit and your validation commit.

**What happened?** Git recorded the validation work as a meaningful checkpoint on `feature/add-validation`.

## Step 8 — See what a branch actually means

This step is intentionally visual.

While you are on `feature/add-validation`, open `src/scoring.py` and confirm that the validation function is present.

Now switch back to `main`:

```bash
git switch main
```

Open `src/scoring.py` again.

The validation change is no longer present in your working file because `main` still represents the baseline version.

Now switch back:

```bash
git switch feature/add-validation
```

The validation code reappears.

Nothing was manually copied, renamed, or restored.

Git changed the working files to represent the branch you selected.

This is the central value of a branch:

> **Different lines of development can coexist in the same repository without creating renamed copies of the project.**

## Step 9 — Merge the completed feature locally

In normal organizational work, the feature would usually be proposed through a GitHub pull request and reviewed before being merged into `main`.

For this self-study exercise, you can practice the underlying merge locally.

Switch to `main`:

```bash
git switch main
```

Merge the completed feature:

```bash
git merge feature/add-validation
```

Now inspect `src/scoring.py`.

The validation code is part of `main`.

Inspect the history:

```bash
git log --oneline
```

### What changed conceptually?

Before the merge:

```text
main                    = accepted baseline
feature/add-validation  = proposed change
```

After the merge:

```text
main = accepted baseline + validation change
```

The exercise has demonstrated the underlying Git mechanics without requiring GitHub access.

## Step 10 — Clean up the completed feature branch

After a feature has been accepted, its short-lived branch is no longer needed.

Delete the local feature branch:

```bash
git branch -d feature/add-validation
```

Run:

```bash
git branch
```

You should now see `main` as the remaining local branch.

This completes the local Git exercise.

---

# Part 4 — Working with GitHub Enterprise

The local exercise deliberately stopped before GitHub because repository creation in the organization is handled through IT.

In actual organizational work, an authorized repository already exists in GitHub Enterprise. You typically begin by cloning that repository rather than running `git init` on a supplied folder:

```bash
git clone <repository-url>
```

Cloning gives you:

- the project files;
- the Git history;
- branch information; and
- a configured connection to the shared GitHub repository.

A useful distinction is:

```text
SELF-STUDY                         ORGANIZATIONAL WORK

supplied project folder            IT-provisioned GitHub repository
        │                                      │
     git init                               git clone
        │                                      │
local repository                    local repository connected
only                                to shared remote repository
        │                                      │
branch / edit / commit              branch / edit / commit
        │                                      │
local merge                         push feature branch
                                               │
                                               ▼
                                        pull request
                                               │
                                               ▼
                                          review
                                               │
                                               ▼
                                           merge
                                               │
                                               ▼
                                             main
```

The Git concepts are the same. GitHub adds the shared collaboration layer.

## Publishing a feature branch

In organizational work, instead of merging your own feature locally, you normally publish the feature branch:

```bash
git push -u origin feature/add-validation
```

The first push creates the corresponding branch in the shared GitHub repository and establishes the tracking relationship. Later pushes from that branch can normally use:

```bash
git push
```

## Creating the pull request

In GitHub Enterprise, create a pull request proposing:

```text
feature/add-validation → main
```

The pull request provides the place to explain, inspect, discuss, and review the proposed change.

If a reviewer requests changes, continue working on the same feature branch:

```bash
git add src/scoring.py
git commit -m "Address validation review comments"
git push
```

The existing pull request updates with the additional commit.

Once the change is approved, it is merged into `main` according to the repository's process.

## Updating your local `main`

After the pull request is merged on GitHub, your computer does not update automatically.

Run:

```bash
git switch main
git pull
```

Your local `main` now contains the newly accepted change.

> **The self-study exercise teaches the Git mechanics. GitHub Enterprise adds sharing, pull requests, review, and the organizational path for accepting changes.**

# Part 6 — Collaboration, Conflicts, and Safe Habits

This is one of the primary reasons Git exists.

Suppose two analysts start from the same `main`.

Analyst A creates `feature/add-validation`.

Analyst B creates `feature/add-income-feature`.

Each person can work independently.

One feature can be reviewed and accepted without requiring the other unfinished feature to be accepted.

This is substantially safer than both analysts editing `customer_model_final.py` in a shared folder and later trying to determine whose copy contains which changes.

Git is specifically designed to manage parallel development.

---

## What Is a Merge Conflict?

Sometimes two branches make incompatible changes to the same portion of a file.

Git may not be able to determine automatically which change should win.

This is called a **merge conflict**.

A merge conflict does **not** mean Git has failed or that the repository is corrupted.

It means Git is asking a human:

> Two changes affect the same area. Which result is correct?

VS Code provides tools for examining and resolving conflicts.

Conflict resolution deserves its own guided exercise and is not necessary for understanding the fundamental workflow.

For now, remember:

> **A conflict is a request for a decision, not a disaster.**

---

## Git Does Not Replace Good Project Practices

Git provides history and change management, but it does not automatically make a project reproducible.

For example, a model may depend on:

- particular source data;
- package versions;
- configuration;
- environment settings;
- random seeds;
- external services.

Git is one part of a reproducible analytical workflow.

It does, however, provide an essential foundation by allowing the team to identify exactly which version of tracked project files existed at a particular point in history.

---

## What Should Go Into Git?

Git is particularly useful for files such as:

- Python source code;
- SQL;
- R code;
- Jupyter notebooks;
- configuration files;
- tests;
- documentation;
- project README files;
- small supporting text files.

Not every file belongs in Git.

Large datasets, generated outputs, credentials, secrets, temporary files, caches, and environment-specific artifacts often require different treatment.

Git supports a file called `.gitignore`, which tells Git that specified files or patterns should not be tracked.

Repository-specific decisions about data and generated artifacts should follow organizational policy.

Most importantly:

> **Do not commit passwords, access tokens, API keys, credentials, or other secrets to a repository.**

Deleting a secret in a later commit does not necessarily remove it from the repository's earlier history.

---

## Git Is Not a Cloud Storage Drive

A GitHub repository may visually resemble a collection of folders and files.

That can encourage a shared-drive approach:

```text
analysis_v1.ipynb
analysis_v2.ipynb
analysis_final.ipynb
analysis_final2.ipynb
```

This throws away much of Git's value.

The repository already contains history.

Instead of encoding history into filenames, allow Git to record that history.

Instead of:

```text
model_v1.py
model_v2.py
model_final.py
```

prefer a stable, meaningful filename:

```text
model.py
```

and meaningful commits:

```text
Initial logistic regression model
Add debt-to-income feature
Add missing-value handling
Tune classification threshold
```

The file represents the current version.

Git represents its history.

---

## Why Branches Are More Than File Copies

A common first reaction is:

> Couldn't I just copy the project folder before making changes?

You could.

But consider several people doing this repeatedly:

```text
model/
model_new/
model_new_jane/
model_new_jane_fixed/
model_production/
model_production_old/
```

Now someone must manually determine:

- which directory is authoritative;
- what differs;
- who changed it;
- why it changed;
- which pieces should be combined;
- whether one copy contains changes missing from another.

Branches provide these parallel lines of work **inside the version-control system**.

They retain their relationship to the shared history and can be compared and merged systematically.

---

## Why Pull Requests Matter Even When "I Know My Code Works"

A pull request is not an accusation that the author cannot be trusted.

It creates an organizational record of a proposed change.

For analytical work, that record can answer:

- What was changed?
- Why was it changed?
- Who proposed it?
- What files were affected?
- What exactly changed within those files?
- Was it reviewed?
- What questions arose during review?
- When was it accepted?

This becomes particularly valuable months later, when the original developer may no longer remember the details—or may no longer be working on the project.

The goal is not bureaucracy.

The goal is to preserve context around important changes.

---

## A Practical Way to Think About the Entire System

Consider four questions.

## What is accepted?

`main`

## Where do I develop a proposed change?

`feature/my-change`

## How do I record meaningful progress?

`commit`

## How does my proposed change become accepted?

`pull request → review → merge`

Everything else becomes easier once these four ideas are understood.

---

## Command-Line and VS Code Equivalents

| Goal | Command Line | VS Code |
|---|---|---|
| Clone repository | `git clone <url>` | Clone Repository |
| See repository state | `git status` | Source Control view |
| View changed lines | `git diff` | Select changed file |
| Create/switch branch | `git switch` | Branch selector |
| Stage file | `git add <file>` | `+` beside change |
| Commit | `git commit -m "message"` | Enter message → Commit |
| Push | `git push` | Push / Sync Changes |
| Pull | `git pull` | Pull / Sync Changes |
| View history | `git log` | Git/history UI depending on configured VS Code capabilities |

> **The graphical interface does not replace Git. It provides controls for Git operations.**

---

# Part 7 — Reference

## Initialize an existing local project folder

```bash
git init
```

**Why:** Begin managing an existing folder as a local Git repository. This is used in the self-study exercise; normal work with an existing organizational GitHub repository usually begins with `git clone`.

## Get an existing organizational repository

```bash
git clone <repository-url>
```

**Why:** Create a local working copy of an existing Git repository, including its history and connection to the shared remote repository.

## Check what is happening

```bash
git status
```

**Why:** Understand the current branch and the state of your working changes.

## See your branches

```bash
git branch
```

**Why:** See local branches and identify the currently selected branch.

## Create and switch to a new branch

```bash
git switch -c feature/my-feature
```

**Why:** Develop a proposed change separately from `main`.

## Switch branches

```bash
git switch main
```

**Why:** Move your working environment to another branch.

## See exactly what changed

```bash
git diff
```

**Why:** Review line-level changes before recording them.

## Stage a file

```bash
git add src/example.py
```

**Why:** Select that change for the next commit.

## Create a commit

```bash
git commit -m "Add customer input validation"
```

**Why:** Record a meaningful checkpoint in local Git history.

## View history

```bash
git log --oneline
```

**Why:** Inspect previous recorded changes.

## Send commits to GitHub

```bash
git push
```

**Why:** Publish local commits to the corresponding remote branch.

For the first push of a newly created branch:

```bash
git push -u origin feature/my-feature
```

## Get current remote changes

```bash
git pull
```

**Why:** Bring changes from the corresponding remote branch into your current local branch.

---

## Terms You Should Now Recognize

### Initialize

Create Git repository metadata for an existing local project folder using `git init`.


### Repository

A Git-managed project containing files and their recorded history.

### Local repository

The repository on your computer.

### Remote repository

The shared repository hosted elsewhere, such as GitHub Enterprise.

### `origin`

The conventional default name for the remote repository from which a repository was cloned.

### `main`

The primary branch, treated in this workflow as the accepted source of truth.

### Branch

A separate line of development used to work on changes without immediately changing `main`.

### Working tree

The project files as they currently exist on your computer.

### Stage / staging area

The set of changes selected for the next commit.

### Commit

A recorded checkpoint in Git history.

### Push

Send local commits to a remote repository.

### Pull

Bring remote changes into the current local branch.

### Diff

A comparison showing what changed.

### Pull request

A proposal to incorporate changes from one branch into another, providing a place for review and discussion.

### Merge

Incorporating the accepted changes from one line of development into another.

---

## Beginner Safety Rules

1. **Check your branch before starting work.** Run `git status` and make sure you know where you are.
2. **Update `main` before creating new work.**
   ```bash
   git switch main
   git pull
   git switch -c feature/my-new-work
   ```
3. **Review before committing.** Use `git status` and `git diff`, or inspect changes in VS Code.
4. **Write meaningful commit messages.** Prefer `Correct quarterly date filter` over `update`.
5. **Do not put secrets in Git.**
6. **Do not panic when Git reports something unfamiliar.** Start with `git status`. If you do not understand the repository's state, get assistance before using commands intended to force, reset, rewrite, or delete history.

---

## The Most Important Takeaway

You do not need Git because filenames such as `analysis_final_FINAL_v3.ipynb` look untidy.

You need version control because analytical products change.

Models change.

Pipelines change.

Business rules change.

Data definitions change.

Assumptions change.

People work simultaneously.

People leave projects.

Bugs are discovered.

Experiments succeed and fail.

Results sometimes need to be reproduced months or years later.

Without deliberate version control, the history and rationale for those changes become scattered across filenames, folders, emails, chat messages, personal knowledge, and memory.

Git provides a structured record of change.

Branches allow changes to be developed without immediately disturbing the accepted version.

Commits record meaningful points in that development.

GitHub provides a shared repository and collaboration environment.

Pull requests make proposed changes visible and reviewable.

Merging turns accepted changes into the new source of truth.

The result is not simply better-organized code.

It is a more reliable way to develop, understand, review, and maintain analytical products.

---

> **Self-study vs. everyday work:** The exercise uses `git init` because it must work without GitHub repository-creation permission. In normal organizational work, IT provisions the GitHub Enterprise repository, authorized users `clone` it, and proposed changes move through feature branches, pushes, pull requests, review, and merge.

---

## Quick Reference: The Basic Workflow

```bash
## Move to the accepted version
git switch main

## Make sure it is current
git pull

## Create a branch for your proposed change
git switch -c feature/my-feature

## Work on your files...

## See what changed
git status
git diff

## Select the files for the next commit
git add src/example.py

## Record the change
git commit -m "Add descriptive message"

## Publish the new branch to GitHub
git push -u origin feature/my-feature

## Create a Pull Request in GitHub Enterprise
## Review → Approve → Merge

## Return to main
git switch main

## Retrieve the newly accepted version
git pull
```

The commands are important.

But the workflow behind them is more important:

```text
ACCEPTED WORK
    main
      │
      ▼
CREATE A SAFE PLACE TO WORK
    feature branch
      │
      ▼
DEVELOP AND TEST
      │
      ▼
RECORD MEANINGFUL CHANGES
    commits
      │
      ▼
SHARE THE PROPOSED CHANGE
    push
      │
      ▼
PROPOSE IT FOR ACCEPTANCE
    pull request
      │
      ▼
REVIEW
      │
      ▼
ACCEPT
    merge
      │
      ▼
NEW SOURCE OF TRUTH
    main
```

That is the core of Git-based collaborative development.
