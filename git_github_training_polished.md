# Git and GitHub for Analytics, Data Science, and Data Engineering

## A Beginner's Guide to Version Control, Collaboration, and a Reliable Source of Truth

**Audience:** Analysts, data scientists, data engineers, and other technical staff who are new to Git  
**Primary tools:** Git, Visual Studio Code (VS Code), and GitHub Enterprise  
**Prior Git experience required:** None

---

## How to Use This Guide

This guide is designed for two kinds of readers:

- **If you primarily want to understand Git and why it matters**, Parts I and II explain the value and the core mental model without requiring you to complete an exercise.
- **If you want to learn by doing**, Part III is a self-contained hands-on lab using a supplied practice project. It does not require permission to create a GitHub repository.
- **If you will use Git in organizational work**, Part IV connects the local Git skills to GitHub Enterprise, feature branches, pull requests, review, and merging.
- **When you begin using Git regularly**, Part V provides practical working habits, and the appendices provide a compact command reference and glossary.

You do not need to memorize every command in this guide. The goal is to understand the workflow well enough that the commands have meaning.

---

# Part I — Why Git?

## 1. We Already Do Version Control — Just Informally

Before learning a Git command, it is worth answering a more important question:

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

Or folders such as:

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
- What changed?
- Who changed it?
- Can I safely experiment?
- Can I recover an earlier working version?
- Which version produced a particular result?
- What did another analyst change?
- Which version should the team use?

The problem is that filenames and folders do not answer these questions reliably.

Git does.

**Git is a system for deliberately recording and managing changes to files over time.**

Instead of creating another copy of a file every time something changes, we keep a stable filename and let Git maintain its history.

Instead of:

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

Git can also tell us **exactly what changed between those recorded versions**.

The file represents the current state. Git represents its history.

---

## 2. Why This Matters for Analytical Work

Version control is sometimes presented as something software developers use to manage application code.

That description is too narrow.

For analytical products, machine-learning models, and data pipelines, we routinely need to answer questions such as:

### Analytical products

Which code produced the results presented last month?

### Machine-learning models

When was a feature added, and what exactly changed in the model code?

### Data pipelines

Why did this month's output differ from last month's output?

### Production processes

What is the currently accepted version of the code?

### Collaboration

What exactly did another analyst change?

### Experimentation

Can I modify this code without disrupting the version everyone else depends on?

### Review

Can someone examine my proposed changes without manually comparing two complete files?

### Recovery

Can we return to a previous working version if a change causes a problem?

Git helps answer all of these questions.

The value of Git is therefore not simply:

> Git stores versions of code.

Its larger value is that it provides a **controlled history of how an analytical product changes**.

Used with GitHub, it also provides a structured way to propose, inspect, discuss, review, and accept those changes.

---

## 3. Git Is Not Just Better File Storage

A GitHub repository can visually resemble a shared drive: folders and files appear in a browser.

That can lead to using GitHub like this:

```text
analysis_v1.ipynb
analysis_v2.ipynb
analysis_final.ipynb
analysis_final2.ipynb
```

But this throws away much of the value of version control.

Git already records history.

Instead of encoding history in filenames:

```text
model_v1.py
model_v2.py
model_final.py
```

prefer a stable, meaningful filename:

```text
model.py
```

and meaningful recorded changes:

```text
Initial logistic regression model
Add debt-to-income feature
Add missing-value handling
Tune classification threshold
```

The objective is not simply cleaner filenames.

It is an identifiable source of truth with a history that can be inspected and understood.

---

## 4. Why Teams Need a Source of Truth

Suppose three people each possess a slightly different copy of a model.

One is called:

```text
model_final.py
```

another:

```text
model_final_updated.py
```

and another:

```text
model_final_jane.py
```

Which is authoritative?

Even if someone can answer today, will the answer still be obvious six months from now?

A version-controlled workflow gives the team an explicit answer. In the simple workflow used throughout this guide:

> **`main` represents the team's accepted version of the project.**

Proposed changes are developed separately and become part of `main` only after they are accepted.

That is more than file organization. It establishes a controlled path for changing the team's source of truth.

---

## 5. Git, GitHub, and VS Code Are Different Things

These tools are related, but they are not interchangeable.

### Git

**Git is the version-control system.**

Git tracks files and their history. Git can operate entirely on your computer without GitHub.

### GitHub Enterprise

**GitHub provides a shared location for Git repositories and tools for collaboration.**

Our organization uses GitHub Enterprise. Organizational repositories are provisioned through the established IT process, and authorized users can then interact with them.

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

### Visual Studio Code

**VS Code is an environment in which we can work with project files and interact with Git.**

VS Code provides both:

- an integrated terminal where Git commands can be entered; and
- a graphical Source Control interface for many of the same Git operations.

For example, you can create a commit in the terminal:

```bash
git commit -m "Add missing-value validation"
```

or use VS Code's Source Control interface to stage files, enter the commit message, and select **Commit**.

These are not different version-control systems.

**VS Code is providing an interface to Git.**

This guide uses both the command line and VS Code because understanding the underlying Git operation makes the graphical interface much easier to understand.

---

## 6. A Note for Jupyter Notebook Users

Using Git does not require abandoning notebooks.

VS Code supports Jupyter notebooks (`.ipynb` files). Notebooks can be opened and executed interactively in VS Code, including running individual cells.

A project can contain notebooks alongside Python modules, tests, documentation, and other files:

```text
customer-risk-analysis/
│
├── notebooks/
│   └── analysis.ipynb
├── src/
│   └── scoring.py
└── README.md
```

All of these files can be managed in the same Git repository.

Notebook files have some special version-control considerations because the underlying `.ipynb` format contains more than the source code visible in each cell. Those considerations are beyond the scope of this introductory guide.

For now, the important point is:

> **Notebook-driven analytical work can still use Git, GitHub, and VS Code.**

---

# Part II — How Git Works

## 7. The Fundamental Mental Model

Before learning the workflow, understand one important distinction:

**Your working files, your local Git history, and GitHub are not automatically the same thing.**

A simplified model is:

```text
Your Working Files
       │
       │ select changes
       ▼
Changes Staged for Commit
       │
       │ commit
       ▼
Local Git History
       │
       │ push
       ▼
GitHub Enterprise
```

You can change a file on your computer without recording the change in Git.

You can record a commit in your local Git repository without sending it to GitHub.

A `push` sends local commits to the shared remote repository.

A `pull` brings remote changes into your local branch.

This distinction explains several Git commands that otherwise seem arbitrary.

---

## 8. What Is a Repository?

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

A repository represents both:

1. **the project as it exists now**, and
2. **the recorded history of how it reached that state.**

That history is what distinguishes a Git repository from an ordinary folder of files.

---

## 9. `main`: The Accepted Version

Most repositories have a primary branch called:

```text
main
```

For the simple workflow in this guide, treat `main` as:

> **The accepted version of the project.**

Suppose `main` contains a working data pipeline and you are asked to add a validation function.

You could immediately edit the accepted version, but then unfinished development and accepted work would be mixed together.

What happens if:

- the function is incomplete?
- the code does not work?
- development takes several days?
- another analyst needs the current accepted version?
- the proposed approach is rejected?
- two people are developing different features simultaneously?

Branches solve this problem.

---

## 10. Branches: A Safe Line of Development

A **branch** provides a separate line of development.

Suppose the accepted project is:

```text
main
  │
  A
  │
  B
  │
  C
```

You need to develop a validation feature.

You create:

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

As you work, your branch can accumulate changes while `main` continues to represent accepted work:

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
```

This is one of the most important ideas in Git:

> **A branch lets you develop a proposed change without immediately changing the team's accepted version.**

---

## 11. A Simple Branching Strategy

Git supports sophisticated branching strategies. Beginners do not need them to use Git effectively.

This guide uses a simple model:

```text
main
  │
  ├── feature/add-validation
  ├── feature/new-model-feature
  └── fix/missing-value-error
```

### `main`

Contains accepted work.

### Short-lived working branches

Contain proposed changes.

Examples:

```text
feature/add-income-variable
feature/add-validation
fix/null-handling
fix/incorrect-date-filter
```

Once the work is accepted and merged into `main`, the short-lived branch can normally be deleted.

---

## 12. Commits: Meaningful Recorded Checkpoints

A **commit** records a meaningful point in a repository's history.

A useful commit message describes the purpose of the change:

```text
Add missing-value validation
Correct date filter for quarterly records
Add debt-to-income feature to scoring model
```

Messages such as these provide little context:

```text
changes
update
stuff
```

The goal is not to write an essay.

The goal is to leave enough information that another person—or you six months later—can understand why the recorded change exists.

> **A commit says: These related changes represent a meaningful state of the project worth recording.**

---

## 13. Staging: Choosing What Goes Into a Commit

Git does not require every change made during a work session to become part of the same commit.

Suppose you:

- modify a scoring function;
- update the README;
- create an unrelated experimental notebook.

Those changes may not logically belong together.

The **staging area** lets you select which changes belong in the next commit.

Conceptually:

```text
Working files
     │
     │ stage selected changes
     ▼
Staging area
     │
     │ commit
     ▼
Recorded history
```

This is why `git add` exists: it selects changes for the next commit.

---

## 14. `git status` and `git diff`: Know What Is Happening

Two commands are particularly useful for understanding repository state.

```bash
git status
```

tells you things such as:

- which branch you are on;
- which files have changed;
- which files are untracked;
- which changes are staged;
- whether the branch differs from its remote counterpart.

A useful beginner rule is:

> **When in doubt, start with `git status`.**

The second command is:

```bash
git diff
```

which shows line-level differences in files that have changed.

VS Code can display these same differences visually through the Source Control view.

Together, `status` and `diff` help answer:

> Where am I, and what have I actually changed?

---

## 15. Local Git Versus GitHub

Git works without GitHub.

That distinction is important enough to make explicit:

```text
LOCAL GIT                         GITHUB ENTERPRISE

Working files                     Shared repository
Local branches                    Shared branches
Local commits       push  ─────►  Shared commits
                    pull  ◄─────  Remote changes
                                  Pull requests
                                  Review
```

Git provides the version-control mechanics.

GitHub adds a shared collaboration environment.

You will experience local Git directly in the hands-on lab before adding the GitHub collaboration layer in Part IV.

---

# Part III — Hands-On Self-Study Lab

## 16. Learn Git by Doing

This section is designed to stand on its own for readers who want hands-on practice.

**You do not need permission to create a GitHub repository to complete this lab.** Everything in the exercise occurs locally on your computer.

### What you will practice

By the end of the lab, you will have:

- turned an ordinary project folder into a Git repository;
- recorded an initial project version;
- created a feature branch;
- changed a Python file;
- inspected the change;
- staged and committed it;
- switched between branches and observed the project change;
- inspected Git history;
- merged the completed feature into `main`;
- deleted the completed feature branch.

The exercise uses a deliberately small analytical project so that the focus remains on Git.

---

## 17. Get the Practice Project

Download the supplied:

```text
customer-risk-analysis-starter.zip
```

Extract it to a location on your computer where you normally keep working files.

After extracting it, you should have a folder named:

```text
customer-risk-analysis
```

Open **VS Code**, select **File → Open Folder**, and select the extracted `customer-risk-analysis` folder.

Then select **Terminal → New Terminal**.

The terminal should open in the project folder.

> **Do not initialize Git before the exercise instructs you to do so.** Starting with an ordinary folder is intentional. You are going to turn it into a Git repository.

> **SCREENSHOT PLACEHOLDER — VS Code: Practice Project Opened**  
> Show the extracted `customer-risk-analysis` folder in the VS Code Explorer and the integrated terminal opened at the project root.

---

## 18. Examine the Starter Project

The supplied project contains:

```text
customer-risk-analysis/
├── README.md
├── data/
│   └── sample_customers.csv
├── notebooks/
│   └── analysis.ipynb
├── src/
│   ├── __init__.py
│   └── scoring.py
├── tests/
│   └── test_scoring.py
├── requirements.txt
└── .gitignore
```

The project is intentionally simple. It exists to provide realistic analytical files for the Git exercise, not to demonstrate a production risk model.

The starting `src/scoring.py` contains:

```python
def calculate_score(income, debt):
    return income - debt
```

Later, you will add input validation on a feature branch.

---

## 19. Turn the Folder Into a Git Repository: `git init`

At this point, the project is an ordinary folder.

Run:

```bash
git init
```

Now run:

```bash
git status
```

Git should report the state of the new repository and show project files that have not yet been recorded.

### What did `git init` accomplish?

`git init` created the Git metadata needed to manage this folder as a repository.

The visible project files did not need to move anywhere.

You have changed this:

```text
Ordinary project folder
```

into this:

```text
Local Git repository
```

GitHub was not involved.

> **A folder can be version-controlled locally with Git without being hosted on GitHub.**

> **SCREENSHOT PLACEHOLDER — VS Code: Source Control After `git init`**  
> Show the Source Control view recognizing the new repository and displaying the untracked project files.

---

## 20. Record the Starting Version

Before developing a feature, record the supplied project as the baseline.

Run:

```bash
git status
```

Stage the project files:

```bash
git add .
```

Run:

```bash
git status
```

again.

Notice that Git now reports the files as changes to be committed.

Create the initial commit:

```bash
git commit -m "Add initial customer risk analysis project"
```

Inspect the history:

```bash
git log --oneline
```

You should see the initial commit.

If your initial branch is not named `main`, rename it:

```bash
git branch -M main
```

For the remainder of the exercise, `main` represents the accepted baseline.

### What just happened?

```text
Project files
     │
     │ git init
     ▼
Local Git repository
     │
     │ git add .
     ▼
Files selected for commit
     │
     │ git commit
     ▼
Recorded baseline on main
```

> **SCREENSHOT PLACEHOLDER — VS Code: Initial Commit**  
> Show the Source Control view after staging, and/or the clean Source Control state after the initial commit.

---

## 21. Create a Feature Branch

You have been assigned a new task:

> Add validation so that records missing required inputs cannot be scored.

Rather than changing `main` directly, create a branch:

```bash
git switch -c feature/add-validation
```

Check your state:

```bash
git status
```

You can also list local branches:

```bash
git branch
```

The current branch is marked with an asterisk:

```text
* feature/add-validation
  main
```

### Why are we doing this?

The validation feature is proposed work.

`main` remains the accepted baseline while you develop the change separately.

> **SCREENSHOT PLACEHOLDER — VS Code: Feature Branch**  
> Show `feature/add-validation` as the current branch in VS Code.

---

## 22. Make the Proposed Change

Open:

```text
src/scoring.py
```

Replace the starting function with:

```python
def validate_inputs(income, debt):
    if income is None or debt is None:
        raise ValueError("Income and debt are required.")


def calculate_score(income, debt):
    validate_inputs(income, debt)
    return income - debt
```

Save the file.

You have changed the working file, but you have not yet recorded that change in Git history.

---

## 23. Ask Git What Changed

Run:

```bash
git status
```

Git should report that:

```text
src/scoring.py
```

has been modified.

Now inspect the actual change:

```bash
git diff
```

Git displays the lines that differ from the last committed version.

### Why does this matter?

Without Git, reviewing a change may mean manually comparing:

```text
scoring_old.py
```

and:

```text
scoring_new_final.py
```

Git already knows exactly what changed.

In VS Code, selecting the changed file in the Source Control view opens a visual diff.

> **SCREENSHOT PLACEHOLDER — VS Code: File Diff**  
> Show the original and modified `scoring.py` with the changed lines highlighted.

---

## 24. Stage the Change

Select the modified file for the next commit:

```bash
git add src/scoring.py
```

Check the state:

```bash
git status
```

The file should now appear as a change to be committed.

### What did staging accomplish?

You told Git:

> **Include this change in my next recorded checkpoint.**

VS Code performs the same operation when you use the `+` control beside a changed file.

> **SCREENSHOT PLACEHOLDER — VS Code: Staged Change**  
> Show `scoring.py` moving from Changes to Staged Changes.

---

## 25. Commit the Feature

Create a commit:

```bash
git commit -m "Add required input validation"
```

Inspect the history:

```bash
git log --oneline
```

You should now see both:

- the initial project commit; and
- the validation commit.

The validation change is recorded on `feature/add-validation`.

---

## 26. See What a Branch Actually Does

This step is intentionally visual.

While on `feature/add-validation`, open `src/scoring.py` and confirm that the validation function is present.

Now switch to `main`:

```bash
git switch main
```

Look at `src/scoring.py` again.

The validation code is no longer present because `main` still represents the baseline version.

Now switch back:

```bash
git switch feature/add-validation
```

The validation code reappears.

You did not manually copy, rename, restore, or replace the file.

Git changed the working files to represent the branch you selected.

> **Different lines of development can coexist in the same repository without creating renamed copies of the project.**

This is the practical meaning of branching.

---

## 27. Merge the Completed Feature Locally

In normal organizational work, a proposed feature would usually be pushed to GitHub Enterprise, reviewed through a pull request, and then merged into `main`.

For this self-study lab, practice the underlying merge locally.

Switch to `main`:

```bash
git switch main
```

Merge the feature:

```bash
git merge feature/add-validation
```

Open `src/scoring.py`.

The validation code is now part of `main`.

Inspect the history:

```bash
git log --oneline
```

### What changed conceptually?

Before:

```text
main                    = accepted baseline
feature/add-validation  = proposed change
```

After:

```text
main = accepted baseline + validation change
```

You have practiced the underlying Git mechanics without requiring a GitHub repository.

---

## 28. Clean Up the Completed Branch

The feature branch has served its purpose.

Delete it:

```bash
git branch -d feature/add-validation
```

Then:

```bash
git branch
```

You should see `main` as the remaining local branch.

### You have completed the local Git workflow

You started with:

```text
customer-risk-analysis/
```

as an ordinary folder.

You then performed:

```text
ordinary folder
      ↓
git init
      ↓
initial commit on main
      ↓
feature branch
      ↓
edit
      ↓
status + diff
      ↓
stage
      ↓
commit
      ↓
switch branches
      ↓
merge
      ↓
updated main
```

You now know the core mechanics that GitHub builds upon.

---

# Part IV — From Local Git to GitHub Enterprise

## 29. What GitHub Adds

The hands-on lab happened entirely on your computer.

Organizational work introduces an additional requirement:

**Other authorized people need access to the repository and its proposed changes.**

GitHub Enterprise provides that shared collaboration layer.

In our organization, repositories are provisioned through the established IT process. Once a repository exists and you have access, you can work with it.

The relationship is:

```text
LOCAL GIT                         GITHUB ENTERPRISE

Local repository                  Shared repository
Local branches                    Shared branches
Local commits       push  ─────►  Shared commits
                    pull  ◄─────  Remote changes
                                  Pull requests
                                  Review
                                  Organizational history
```

---

## 30. Getting an Existing Organizational Repository: `git clone`

In the self-study lab, you began with an ordinary folder and ran:

```bash
git init
```

That was useful for learning what makes a folder a Git repository.

In normal organizational work, the repository already exists in GitHub Enterprise.

You therefore usually begin with:

```bash
git clone <repository-url>
```

Cloning gives you:

- the project files;
- the Git history;
- information about branches; and
- a configured connection to the GitHub repository.

> **`git init` begins version control for a local folder. `git clone` gives you a local copy of an existing Git repository.**

### VS Code

VS Code can also clone a repository through its Command Palette or Source Control interface.

> **SCREENSHOT PLACEHOLDER — VS Code: Clone Repository**  
> Show the VS Code option for cloning an authorized GitHub Enterprise repository.

---

## 31. `origin`: The Shared Repository Connection

When you clone a repository, Git normally gives the source remote repository the conventional name:

```text
origin
```

So when you later see:

```bash
git push -u origin feature/add-validation
```

`origin` refers to the shared remote repository associated with your local clone.

You do not need to think of `origin` as another branch.

It is a name Git uses for a remote repository location.

---

## 32. Publishing Your Feature Branch: `git push`

Suppose you created:

```text
feature/add-validation
```

and committed your work locally.

At this point, the commit exists on your computer.

It does not automatically exist in GitHub.

For the first push of the new branch:

```bash
git push -u origin feature/add-validation
```

This:

- sends the branch and its commits to GitHub Enterprise; and
- establishes a tracking relationship between the local and remote branch.

After that relationship is established, later pushes from the same branch can normally use:

```bash
git push
```

> **Commit records work locally. Push publishes those commits to the remote repository.**

> **SCREENSHOT PLACEHOLDER — VS Code: Publish/Push Branch**  
> Show the feature branch being published to GitHub Enterprise.

---

## 33. Pull Requests: From Proposed Change to Accepted Change

Publishing a feature branch does **not** change `main`.

That is intentional.

The branch represents:

> **My proposed change to the project.**

A **pull request**, commonly called a **PR**, proposes that changes from one branch be incorporated into another.

In our workflow:

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

- explain why the change is needed;
- see which files changed;
- inspect line-level differences;
- discuss the implementation;
- request modifications;
- document review;
- approve the change;
- merge accepted work.

A pull request is therefore more than an administrative approval step.

It provides a controlled transition from:

> **my proposed change**

to:

> **our accepted version**

> **SCREENSHOT PLACEHOLDER — GitHub Enterprise: Create Pull Request**  
> Show a feature branch being proposed for merge into `main`.

> **SCREENSHOT PLACEHOLDER — GitHub Enterprise: Pull Request Diff**  
> Show the changed files and line-level review interface.

---

## 34. Responding to Review

Suppose a reviewer requests a change.

You normally remain on the same feature branch, make the correction, and create another commit:

```bash
git add src/scoring.py
git commit -m "Address validation review comments"
git push
```

The existing pull request updates to include the additional commit.

You do not need a new branch or a new pull request for ordinary revisions to the same proposed feature.

---

## 35. Merging: Accepting the Change

Once the pull request has been reviewed and approved according to the repository's process, it can be merged.

Conceptually:

```text
feature branch
      │
      │ proposed
      ▼
pull request
      │
      │ reviewed and accepted
      ▼
    merge
      │
      ▼
    main
```

The exact commit history can vary depending on the merge method configured for the repository.

The important beginner concept is simpler:

> **After the merge, the accepted change is part of `main`.**

The short-lived feature branch can normally be deleted after it has served its purpose.

---

## 36. Getting Changes From GitHub: `git pull`

After a pull request is merged, GitHub's `main` has changed.

Your computer does not update automatically.

Switch to your local `main`:

```bash
git switch main
```

Then retrieve the latest remote changes:

```bash
git pull
```

Your local `main` is brought up to date.

A useful distinction is:

> **Push sends your commits to the remote repository. Pull brings remote changes into your local branch.**

---

## 37. The Everyday Organizational Workflow

Once you are working with an existing organizational repository, the normal cycle becomes:

```text
Update main
    │
    ▼
Create feature branch
    │
    ▼
Develop and test
    │
    ▼
Review your changes
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

At the command line:

```bash
# Start from the accepted version
git switch main

# Get the latest accepted changes
git pull

# Create a branch for your proposed work
git switch -c feature/my-feature

# Work on your files...

# Understand what changed
git status
git diff

# Select the intended changes
git add src/example.py

# Record them locally
git commit -m "Add descriptive message"

# Publish the new branch
git push -u origin feature/my-feature

# In GitHub Enterprise:
# Create Pull Request → Review → Approve → Merge

# Return to main
git switch main

# Get the newly accepted version
git pull
```

The commands matter, but the workflow matters more.

---

# Part V — Everyday Working Practices

## 38. Command Line and VS Code Are Two Views of the Same Workflow

Many everyday Git operations can be performed either way.

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

> **The graphical interface does not replace Git. It provides controls for Git operations.**

Understanding the operation allows you to use either interface confidently.

---

## 39. What Should Go Into Git?

Git is particularly useful for files such as:

- Python source code;
- SQL;
- R code;
- Jupyter notebooks;
- configuration files;
- tests;
- documentation;
- README files;
- small supporting text files.

Not every file belongs in Git.

Large datasets, generated outputs, credentials, secrets, temporary files, caches, and environment-specific artifacts often require different treatment.

Git supports a file called:

```text
.gitignore
```

which tells Git that specified files or patterns should not be tracked.

Repository-specific decisions about data and generated artifacts should follow organizational policy.

Most importantly:

> **Do not commit passwords, access tokens, API keys, credentials, or other secrets to a repository.**

Deleting a secret in a later commit does not necessarily remove it from earlier repository history.

---

## 40. Why Branches Are Better Than Project Copies

A common reaction is:

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

Branches provide parallel lines of work **inside the version-control system**.

They retain their relationship to shared history and can be compared and merged systematically.

---

## 41. What If Two People Work at the Same Time?

This is one of the problems Git is designed to solve.

Suppose two analysts begin from the same `main`.

Analyst A creates:

```text
feature/add-validation
```

Analyst B creates:

```text
feature/add-income-feature
```

Each person can work independently.

One feature can be reviewed and accepted without requiring the other unfinished feature to be accepted.

This is substantially safer than both analysts editing:

```text
customer_model_final.py
```

in a shared folder and later trying to determine whose copy contains which changes.

---

## 42. What Is a Merge Conflict?

Sometimes two lines of development make incompatible changes to the same portion of a file.

Git may not be able to determine automatically which result is correct.

This is called a **merge conflict**.

A merge conflict does **not** mean Git has failed or that the repository is corrupted.

It means Git needs a human decision:

> Two changes affect the same area. Which result is correct?

VS Code provides tools for examining and resolving conflicts.

Detailed conflict resolution is beyond the scope of this introductory guide.

For now, remember:

> **A conflict is a request for a decision, not a disaster.**

When encountering an unfamiliar conflict, inspect the situation before attempting destructive or unfamiliar Git commands.

---

## 43. Why Pull Requests Matter Even When the Code Works

A pull request is not an accusation that the author cannot be trusted.

It creates an organizational record of a proposed change.

For analytical work, that record can help answer:

- What changed?
- Why did it change?
- Who proposed it?
- What files were affected?
- What exactly changed within those files?
- Was it reviewed?
- What questions arose during review?
- When was it accepted?

This becomes especially valuable months later, when the original developer may no longer remember the details—or may no longer be working on the project.

The goal is not bureaucracy.

The goal is to preserve context around important changes.

---

## 44. Git Does Not Automatically Make a Project Reproducible

Git provides history and change management, but reproducibility can depend on more than source code.

A model or analytical process may depend on:

- particular source data;
- package versions;
- configuration;
- environment settings;
- random seeds;
- external services.

Git is therefore one part of a reproducible analytical workflow.

It provides an essential foundation by allowing the team to identify exactly which version of tracked project files existed at a particular point in history.

---

## 45. Beginner Safety Rules

### 1. Check where you are before starting work

```bash
git status
```

Know which branch you are on and whether you already have changes.

### 2. Update `main` before beginning new organizational work

```bash
git switch main
git pull
git switch -c feature/my-new-work
```

This starts the new branch from the current accepted version.

### 3. Review before committing

Use:

```bash
git status
git diff
```

or inspect changes in VS Code.

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

Do not commit passwords, tokens, credentials, or other secrets.

### 6. Do not panic when Git reports something unfamiliar

Start with:

```bash
git status
```

Read what Git is reporting.

If you do not understand the repository's state, get assistance before using commands intended to force, reset, rewrite, or delete history.

---

## 46. The Most Important Takeaway

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

Without deliberate version control, the history and rationale for those changes become scattered across filenames, folders, email, chat messages, personal knowledge, and memory.

Git provides a structured record of change.

Branches allow proposed work to be developed without immediately disturbing the accepted version.

Commits record meaningful points in that development.

GitHub provides a shared repository and collaboration environment.

Pull requests make proposed changes visible and reviewable.

Merging turns accepted changes into the new source of truth.

The result is not simply better-organized code.

It is a more reliable way to develop, understand, review, and maintain analytical products.

---

# Appendix A — Git Command Quick Reference

This appendix is a reference, not a list you are expected to memorize.

## Initialize an existing local folder

```bash
git init
```

**Purpose:** Begin managing an existing local folder as a Git repository.

The self-study lab uses this command. Normal work with an existing organizational repository usually begins with `git clone`.

---

## Clone an existing repository

```bash
git clone <repository-url>
```

**Purpose:** Create a local working copy of an existing Git repository, including its history and remote connection.

---

## Check repository state

```bash
git status
```

**Purpose:** See the current branch and understand the state of working, staged, and untracked files.

---

## List local branches

```bash
git branch
```

**Purpose:** See local branches and identify the current branch.

---

## Create and switch to a branch

```bash
git switch -c feature/my-feature
```

**Purpose:** Create a separate line of development for proposed work.

---

## Switch branches

```bash
git switch main
```

**Purpose:** Change the branch represented by your working files.

---

## Inspect changes

```bash
git diff
```

**Purpose:** Review line-level working changes before recording them.

---

## Stage a file

```bash
git add src/example.py
```

**Purpose:** Select a change for the next commit.

To stage all appropriate changes beneath the current directory:

```bash
git add .
```

Review what will be staged before relying on this shortcut.

---

## Create a commit

```bash
git commit -m "Add customer input validation"
```

**Purpose:** Record the staged changes as a meaningful checkpoint in local history.

---

## View compact history

```bash
git log --oneline
```

**Purpose:** Inspect previous recorded changes.

---

## Push a new branch

```bash
git push -u origin feature/my-feature
```

**Purpose:** Publish the new local branch and its commits to the remote repository and establish its tracking relationship.

Subsequent pushes can normally use:

```bash
git push
```

---

## Pull remote changes

```bash
git pull
```

**Purpose:** Bring changes from the corresponding remote branch into the current local branch.

---

## Merge a branch locally

```bash
git merge feature/my-feature
```

**Purpose:** Incorporate another branch's changes into the current branch.

The self-study lab uses a local merge to demonstrate the mechanics. Organizational repositories will commonly use a pull request and the repository's configured merge process.

---

## Delete a completed local branch

```bash
git branch -d feature/my-feature
```

**Purpose:** Remove a local branch after its work has been merged and the branch is no longer needed.

---

# Appendix B — Glossary

### Branch

A separate line of development used to work on changes without immediately changing another branch such as `main`.

### Commit

A recorded checkpoint in Git history.

### Diff

A comparison showing what changed between versions or working states.

### Git

The version-control system that manages project history.

### GitHub Enterprise

The organization's shared platform for hosting Git repositories and supporting collaboration, pull requests, review, and access control.

### `main`

The primary branch, treated in this guide as the accepted source of truth.

### Merge

Incorporating changes from one line of development into another.

### Merge conflict

A situation in which Git cannot automatically determine how competing changes should be combined and requires a human decision.

### Local repository

The Git repository on your computer.

### `origin`

The conventional default name for the remote repository from which a repository was cloned.

### Pull

Bring remote changes into the current local branch.

### Pull request

A proposal to incorporate changes from one branch into another, providing a place for review and discussion.

### Push

Send local commits to a remote repository.

### Remote repository

A shared Git repository hosted elsewhere, such as GitHub Enterprise.

### Repository

A Git-managed project containing files and their recorded history.

### Stage / staging area

The set of changes selected for the next commit.

### Working files / working tree

The project files as they currently exist on your computer.

---

# One-Page Workflow Reminder

For normal work in an existing organizational repository:

```bash
# Start from accepted work
git switch main
git pull

# Create a safe place for your proposed change
git switch -c feature/my-feature

# Develop and test...

# Understand what changed
git status
git diff

# Select and record the intended change
git add src/example.py
git commit -m "Describe the change"

# Publish the proposed work
git push -u origin feature/my-feature

# GitHub Enterprise:
# Pull Request → Review → Approve → Merge

# Return to the accepted version and update it
git switch main
git pull
```

Remember the larger workflow:

```text
WHY
Understand the value of controlled history
      │
      ▼
MAIN
Accepted source of truth
      │
      ▼
BRANCH
Safe place for proposed work
      │
      ▼
COMMIT
Meaningful recorded change
      │
      ▼
PUSH
Share the proposed work
      │
      ▼
PULL REQUEST
Make the change visible and reviewable
      │
      ▼
MERGE
Accept the change
      │
      ▼
MAIN
New source of truth
```

**That is the core of Git-based collaborative development.**
