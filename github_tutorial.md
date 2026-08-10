# Git and GitHub for Analytics, Data Science, and Data Engineering

## A Beginner's Guide to Version Control, Collaboration, and a Reliable Source of Truth

---

# 1. Why Are We Using Git?

Before learning any Git commands, it is worth answering a more important question:

**Why do we need Git at all?**

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

* Which version is current?
* What did we change?
* Can I safely make an experimental change?
* Can I recover the previous version?
* Which version produced a particular result?
* What did another analyst change?
* Which version should the team use?

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

# 2. Why This Matters for Analytical Work

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

# 3. Git, GitHub, and VS Code Are Different Things

These terms are related, but they are not interchangeable.

## Git

**Git is the version-control system.**

Git tracks files and their history.

Git can operate on your computer even without GitHub.

---

## GitHub

**GitHub provides a shared location for Git repositories and tools for collaboration.**

Our organization uses GitHub Enterprise.

GitHub provides capabilities such as:

* centrally hosted repositories;
* access control;
* shared branches;
* pull requests;
* code review;
* change history;
* collaboration among team members.

A useful simplified distinction is:

> **Git manages versions. GitHub helps teams share and collaborate on those versions.**

---

## Visual Studio Code

**Visual Studio Code (VS Code) is the environment in which we can work with our files and interact with Git.**

VS Code provides both:

* an integrated terminal where Git commands can be entered; and
* a graphical Source Control interface for performing many of the same Git operations.

For example, you can create a Git commit from the terminal:

```bash
git commit -m "Add missing-value validation"
```

or use VS Code's Source Control interface to enter the commit message and select **Commit**.

These are not separate version-control systems.

**VS Code is providing a graphical interface to Git.**

This guide teaches both because understanding the underlying Git operations makes the graphical interface much easier to understand.

---

# 4. A Note for Jupyter Notebook Users

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

# 5. The Fundamental Git Mental Model

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

# 6. What Is a Repository?

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

# 7. Getting a Repository: `git clone`

In our environment, repositories are provisioned through the organization's GitHub Enterprise process.

Once you have access to a repository, you normally need a working copy on your computer.

This is where `clone` comes in.

```bash
git clone <repository-url>
```

For example:

```bash
git clone <your-organization-repository-url>
```

## Why does `clone` matter?

It is tempting to think of cloning as downloading a folder.

It does more than that.

Cloning gives you:

* the project files;
* the Git history;
* information about the branches;
* a connection to the GitHub repository from which it was cloned.

After cloning, you have a **local repository** on your computer.

GitHub contains the shared remote repository.

Your computer contains your local copy.

> **Clone means: Give me a local working copy of this Git repository and its history.**

### VS Code

VS Code can also clone a repository through the Command Palette or Source Control interface.

**SCREENSHOT PLACEHOLDER — VS Code: Clone Repository**

Show the VS Code option for cloning a Git repository and selecting the local destination.

---

# 8. Your Most Useful Diagnostic Command: `git status`

After opening the repository, one of the most useful commands you can learn is:

```bash
git status
```

`git status` tells you about the current state of your working repository.

For example, it can tell you:

* which branch you are on;
* which files have changed;
* which files are untracked;
* which changes are staged;
* whether your local branch differs from its remote counterpart.

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

# 9. `main`: The Team's Accepted Source of Truth

Most repositories have a primary branch called:

```text
main
```

For this introductory workflow, we will treat `main` as:

> **The accepted version of the project.**

Suppose `main` contains a working data pipeline.

You are asked to add a new validation function.

You *could* immediately change the files on `main`.

But consider what that means.

The team's accepted version and your unfinished experimental work would now be mixed together.

What happens if:

* your function is incomplete?
* the code does not work?
* you need three days to finish it?
* another person needs the current production version?
* your approach is reviewed and rejected?
* two people are developing different features simultaneously?

This is the problem branches solve.

---

# 10. Branches: A Safe Place to Make a Change

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

# 11. A Simple Branching Strategy

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

# 12. Creating a Branch

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

**SCREENSHOT PLACEHOLDER — VS Code: Current Branch**

Show the current branch and the option to create or switch branches.

---

# 13. Make a Change

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

# 14. See Exactly What Changed: `git diff`

Before recording the change, you can ask Git to show exactly what is different:

```bash
git diff
```

This displays the differences between the current working files and the version Git previously recorded.

## Why does `diff` matter?

Without Git, reviewing a change may involve opening:

```text
scoring_old.py
```

and:

```text
scoring_new_final.py
```

and manually trying to determine what is different.

Git already knows.

A diff allows you to review changes at the line level.

This becomes particularly valuable during code review.

### VS Code

Selecting a changed file in the Source Control view opens a visual comparison showing removed and added lines.

**SCREENSHOT PLACEHOLDER — VS Code: File Diff**

Show the original and modified versions of `scoring.py` with the changed lines highlighted.

---

# 15. Staging: Choosing What Belongs in the Next Commit

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

* modify the scoring function;
* update the README;
* create an unrelated experimental notebook.

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

**SCREENSHOT PLACEHOLDER — VS Code: Staging Changes**

Show a modified file first under Changes and then under Staged Changes.

---

# 16. Commits: Recording a Meaningful Point in History

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
```

or:

```text
update
```

or:

```text
stuff
```

The goal is not to write an essay.

The goal is to leave enough context that someone—including you six months from now—can understand why this point exists in the project's history.

> **A commit says: These related changes represent a meaningful state of the project worth recording.**

---

# 17. Saving a File Is Not the Same as Committing It

This distinction is important.

When you save a file:

```text
Edit → Save
```

you update the file on your computer.

When you stage it:

```text
git add
```

you select the change for the next commit.

When you commit it:

```text
git commit
```

you record that selected change in your local Git history.

When you push it:

```text
git push
```

you send your local commits to the remote repository on GitHub.

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

# 18. Viewing History: `git log`

To inspect recorded history:

```bash
git log
```

Git displays commits and information such as:

* commit identifier;
* author;
* date;
* commit message.

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

# 19. Local Work Versus GitHub: Why `push` Exists

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

* `push` sends commits to the remote repository;
* `origin` is the conventional name Git usually gives the remote repository you cloned;
* `feature/add-validation` is the branch being pushed;
* `-u` establishes the relationship between your local branch and its remote counterpart.

After that relationship has been established, later pushes can usually be performed with:

```bash
git push
```

### VS Code

VS Code provides controls to publish, push, or synchronize the branch.

**SCREENSHOT PLACEHOLDER — VS Code: Publish/Push Branch**

Show the feature branch being published to the organization's GitHub Enterprise repository.

---

# 20. A Branch on GitHub Is Still Only a Proposal

This distinction is critical.

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

# 21. Pull Requests: From "My Change" to "Our Accepted Change"

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

* explain the purpose of the change;
* see exactly which files changed;
* inspect line-level differences;
* discuss the implementation;
* request modifications;
* document review;
* approve the change;
* merge the accepted work.

This makes a pull request much more than an administrative approval step.

It provides a controlled transition from:

> **my proposed change**

to:

> **our accepted version**

For analytical work, a pull request might document:

> Add missing-value validation to prevent customer records with incomplete income or debt fields from entering the scoring process.

A reviewer can inspect exactly what changed before it becomes part of `main`.

**SCREENSHOT PLACEHOLDER — GitHub Enterprise: Create Pull Request**

Show the feature branch being proposed for merge into `main`.

**SCREENSHOT PLACEHOLDER — GitHub Enterprise: PR Diff**

Show the Files Changed view and reviewer controls.

---

# 22. Merging: Accepting the Change

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

After the feature is accepted into `main`:

```text
main
  │
  A
  │
  B
  │
  C
  │
  D
  │
  E
```

The exact history can vary depending on the repository's configured merge method, but the important beginner concept remains the same:

**The accepted feature is now part of `main`.**

The feature branch has served its purpose and can normally be deleted.

---

# 23. Getting the Newly Accepted Version: `git pull`

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

This illustrates why `pull` exists.

GitHub and your local repository are separate.

Changes made or merged on GitHub do not magically appear in your local files.

> **Push sends your commits to the remote repository. Pull brings remote changes into your local branch.**

---

# 24. The Everyday Workflow

At this point, the core workflow can be summarized.

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

This cycle is the heart of the introductory workflow:

```text
Update main
    │
    ▼
Create branch
    │
    ▼
Develop and test
    │
    ▼
Review changes
    │
    ▼
Stage
    │
    ▼
Commit
    │
    ▼
Push
    │
    ▼
Pull Request
    │
    ▼
Review
    │
    ▼
Merge
    │
    ▼
Updated main
```

---

# 25. Guided Exercise: A Complete Analytics Workflow

This exercise walks through the entire process.

The purpose is not to build a sophisticated analytical model.

The purpose is to experience the complete Git workflow using a project that resembles analytical work.

---

## Scenario

Your team maintains a simple customer-risk scoring project.

The repository contains:

```text
customer-risk-analysis/
│
├── README.md
└── src/
    └── scoring.py
```

`main` contains the team's currently accepted scoring logic.

You have been assigned a new task:

> Add validation so that records missing required inputs cannot be scored.

You will develop this change without modifying `main` directly.

---

## Step 1 — Clone the repository

Obtain the repository URL from GitHub Enterprise.

In the VS Code terminal:

```bash
git clone <repository-url>
```

Open the cloned folder in VS Code.

### What did we accomplish?

You now have a local Git repository containing the project and its Git history.

---

## Step 2 — Inspect the repository

Run:

```bash
git status
```

Confirm that you are on `main` and that the working tree has no unexpected changes.

### Why?

Before starting work, establish what state the repository is currently in.

---

## Step 3 — Make sure `main` is current

Run:

```bash
git pull
```

### Why?

Someone else may have changed the shared repository since your local copy was created or last updated.

You want your new work to begin from the current accepted version.

---

## Step 4 — Create a feature branch

Run:

```bash
git switch -c feature/add-validation
```

Then:

```bash
git status
```

### Why?

You are about to develop a proposed change.

`main` should continue representing the accepted version while that work is underway.

Your branch is your safe development space.

---

## Step 5 — Modify the code

Suppose `src/scoring.py` initially contains:

```python
def calculate_score(income, debt):
    return income - debt
```

Add:

```python
def validate_inputs(income, debt):
    if income is None or debt is None:
        raise ValueError("Income and debt are required.")


def calculate_score(income, debt):
    validate_inputs(income, debt)
    return income - debt
```

Save the file.

---

## Step 6 — Ask Git what changed

Run:

```bash
git status
```

You should see that `src/scoring.py` has been modified.

### What does this tell us?

Git sees the change, but the change has not yet been recorded in the repository history.

---

## Step 7 — Inspect the actual change

Run:

```bash
git diff
```

Review the added lines.

### Why?

Before recording a change, verify what you actually changed.

This is a useful habit even when you are the author.

---

## Step 8 — Stage the change

Run:

```bash
git add src/scoring.py
```

Then:

```bash
git status
```

### What happened?

You selected `src/scoring.py` for inclusion in the next commit.

---

## Step 9 — Commit the change

Run:

```bash
git commit -m "Add required input validation"
```

Then:

```bash
git status
```

### What happened?

Git recorded a meaningful checkpoint containing the staged change.

This commit exists in your local repository.

---

## Step 10 — Examine the history

Run:

```bash
git log --oneline
```

Your new commit should appear at the top of the history.

### Why?

You can now identify the exact recorded change, including its commit identifier and message.

---

## Step 11 — Push the feature branch

Run:

```bash
git push -u origin feature/add-validation
```

### What happened?

Your local branch and its commits were published to GitHub Enterprise.

`main` is still unchanged.

---

## Step 12 — Create a pull request

In GitHub Enterprise, create a pull request:

```text
feature/add-validation → main
```

Give the pull request a useful title, such as:

```text
Add required input validation to customer scoring
```

In the description, briefly explain:

* why the change is needed;
* what changed;
* how the change was tested.

### Why?

The pull request provides context and allows another person to review the proposed change before it becomes part of the accepted project.

---

## Step 13 — Review the change

The reviewer examines the pull request.

The reviewer can see the exact differences introduced by the feature branch.

If changes are requested, return to the same feature branch, make the corrections, stage them, and create another commit:

```bash
git add src/scoring.py
git commit -m "Address validation review comments"
git push
```

The pull request updates to include the new commit.

You do **not** need to create another branch or another pull request for ordinary revisions to the same proposed change.

---

## Step 14 — Merge the pull request

Once approved, merge the pull request according to the repository's process.

The validation feature is now part of `main`.

### What changed conceptually?

Before:

```text
feature/add-validation = proposed work
main                   = accepted work
```

After the merge:

```text
main = accepted work including validation
```

That distinction is the purpose of the workflow.

---

## Step 15 — Update your local `main`

Your local computer may not yet contain the newly merged `main`.

Run:

```bash
git switch main
git pull
```

Your local `main` now includes the accepted validation feature.

The development cycle is complete.

---

# 26. What If Two People Work at the Same Time?

This is one of the primary reasons Git exists.

Suppose two analysts start from the same `main`.

Analyst A creates:

```text
feature/add-validation
```

Analyst B creates:

```text
feature/add-income-feature
```

Conceptually:

```text
                  feature/add-validation
                 /
main ───────────●
                 \
                  feature/add-income-feature
```

Each person can work independently.

One feature can be reviewed and accepted without requiring the other unfinished feature to be accepted.

This is substantially safer than both analysts editing:

```text
customer_model_final.py
```

in a shared folder and later trying to determine whose copy contains which changes.

Git is specifically designed to manage parallel development.

---

# 27. What Is a Merge Conflict?

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

When encountering an unfamiliar conflict, inspect the situation before attempting destructive or unfamiliar Git commands.

---

# 28. Git Does Not Replace Good Project Practices

Git provides history and change management, but it does not automatically make a project reproducible.

For example, a model may depend on:

* particular source data;
* package versions;
* configuration;
* environment settings;
* random seeds;
* external services.

Git is one part of a reproducible analytical workflow.

It does, however, provide an essential foundation by allowing the team to identify exactly which version of tracked project files existed at a particular point in history.

---

# 29. What Should Go Into Git?

Git is particularly useful for files such as:

* Python source code;
* SQL;
* R code;
* Jupyter notebooks;
* configuration files;
* tests;
* documentation;
* project README files;
* small supporting text files.

Not every file belongs in Git.

Large datasets, generated outputs, credentials, secrets, temporary files, caches, and environment-specific artifacts often require different treatment.

Git supports a file called:

```text
.gitignore
```

which tells Git that specified files or patterns should not be tracked.

For example, Python projects commonly exclude generated cache directories.

Repository-specific decisions about data and generated artifacts should follow organizational policy.

Most importantly:

> **Do not commit passwords, access tokens, API keys, credentials, or other secrets to a repository.**

Deleting a secret in a later commit does not necessarily remove it from the repository's earlier history.

---

# 30. Git Is Not a Cloud Storage Drive

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

# 31. Why Branches Are More Than File Copies

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

* which directory is authoritative;
* what differs;
* who changed it;
* why it changed;
* which pieces should be combined;
* whether one copy contains changes missing from another.

Branches provide these parallel lines of work **inside the version-control system**.

They retain their relationship to the shared history and can be compared and merged systematically.

---

# 32. Why Pull Requests Matter Even When "I Know My Code Works"

A pull request is not an accusation that the author cannot be trusted.

It creates an organizational record of a proposed change.

For analytical work, that record can answer:

* What was changed?
* Why was it changed?
* Who proposed it?
* What files were affected?
* What exactly changed within those files?
* Was it reviewed?
* What questions arose during review?
* When was it accepted?

This becomes particularly valuable months later, when the original developer may no longer remember the details—or may no longer be working on the project.

The goal is not bureaucracy.

The goal is to preserve context around important changes.

---

# 33. A Practical Way to Think About the Entire System

Consider four questions.

## What is accepted?

```text
main
```

## Where do I develop a proposed change?

```text
feature/my-change
```

## How do I record meaningful progress?

```text
commit
```

## How does my proposed change become accepted?

```text
pull request → review → merge
```

Everything else becomes easier once these four ideas are understood.

---

# 34. Command-Line and VS Code Equivalents

Many everyday operations can be performed either way.

| Goal                 | Command Line              | VS Code                                                     |
| -------------------- | ------------------------- | ----------------------------------------------------------- |
| Clone repository     | `git clone <url>`         | Clone Repository                                            |
| See repository state | `git status`              | Source Control view                                         |
| View changed lines   | `git diff`                | Select changed file                                         |
| Create/switch branch | `git switch`              | Branch selector                                             |
| Stage file           | `git add <file>`          | `+` beside change                                           |
| Commit               | `git commit -m "message"` | Enter message → Commit                                      |
| Push                 | `git push`                | Push / Sync Changes                                         |
| Pull                 | `git pull`                | Pull / Sync Changes                                         |
| View history         | `git log`                 | Git/history UI depending on configured VS Code capabilities |

The important lesson is:

> **The graphical interface does not replace Git. It provides controls for Git operations.**

Knowing what the operations mean allows you to use either interface confidently.

---

# 35. Core Commands for Everyday Work

These commands cover much of what a beginner needs for the workflow described in this guide.

## Get a repository

```bash
git clone <repository-url>
```

**Why:** Create a local working copy of a Git repository and its history.

---

## Check what is happening

```bash
git status
```

**Why:** Understand the current branch and the state of your working changes.

---

## See your branches

```bash
git branch
```

**Why:** See local branches and identify the currently selected branch.

---

## Create and switch to a new branch

```bash
git switch -c feature/my-feature
```

**Why:** Develop a proposed change separately from `main`.

---

## Switch branches

```bash
git switch main
```

**Why:** Move your working environment to another branch.

---

## See exactly what changed

```bash
git diff
```

**Why:** Review line-level changes before recording them.

---

## Stage a file

```bash
git add src/example.py
```

**Why:** Select that change for the next commit.

---

## Create a commit

```bash
git commit -m "Add customer input validation"
```

**Why:** Record a meaningful checkpoint in local Git history.

---

## View history

```bash
git log --oneline
```

**Why:** Inspect previous recorded changes.

---

## Send commits to GitHub

```bash
git push
```

**Why:** Publish local commits to the corresponding remote branch.

For the first push of a newly created branch:

```bash
git push -u origin feature/my-feature
```

---

## Get current remote changes

```bash
git pull
```

**Why:** Bring changes from the corresponding remote branch into your current local branch.

---

# 36. Terms You Should Now Recognize

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

# 37. Beginner Safety Rules

Git is powerful, but the everyday workflow does not require memorizing a large number of commands.

A few habits prevent many problems.

### 1. Check your branch before starting work

Run:

```bash
git status
```

Make sure you know where you are.

### 2. Update `main` before creating new work

A common starting sequence is:

```bash
git switch main
git pull
git switch -c feature/my-new-work
```

### 3. Review before committing

Use:

```bash
git status
git diff
```

or inspect the changes in VS Code.

Know what you are about to record.

### 4. Write meaningful commit messages

Prefer:

```text
Correct quarterly date filter
```

over:

```text
update
```

### 5. Do not put secrets in Git

Do not commit passwords, tokens, credentials, or other sensitive secrets.

### 6. Do not panic when Git reports something unfamiliar

Start with:

```bash
git status
```

Read what Git is reporting.

If you do not understand the repository's state, get assistance before using commands intended to force, reset, rewrite, or delete history.

---

# 38. The Most Important Takeaway

You do not need Git because filenames such as:

```text
analysis_final_FINAL_v3.ipynb
```

look untidy.

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

# Quick Reference: The Basic Workflow

```bash
# Move to the accepted version
git switch main

# Make sure it is current
git pull

# Create a branch for your proposed change
git switch -c feature/my-feature

# Work on your files...

# See what changed
git status
git diff

# Select the files for the next commit
git add src/example.py

# Record the change
git commit -m "Add descriptive message"

# Publish the new branch to GitHub
git push -u origin feature/my-feature

# Create a Pull Request in GitHub Enterprise
# Review → Approve → Merge

# Return to main
git switch main

# Retrieve the newly accepted version
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
