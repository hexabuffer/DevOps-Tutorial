

## Git & GitHub: The Complete Guide

### Introduction
- What you will learn
- Why Git and GitHub are essential
- Roadmap for this course


## What is Version Control?

Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.

### Key Benefits:
- **Track History:** See who changed what and when.
- **Collaboration:** Multiple people can work on the same project without overwriting each other's work.
- **Safety:** Revert files or projects back to a previous state if something goes wrong.

*Also known as Source Code Management (SCM) or Revision Control.*



## Why Do You Need Version Control?

Imagine working on a project without it:
- You'd have files like `final_v1.zip`, `final_v2_new.zip`, `FINAL_REALLY_FINAL.zip`.
- If you made a mistake two days ago, you'd have to remember what you changed.
- Sharing code with teammates would involve manual copy-pasting or emailing files.

### With Version Control:
- One clean working directory.
- Complete audit trail of every modification.
- Experiments can be done in isolation (branches).



## Centralized vs. Distributed VCS

### Centralized (e.g., SVN)
- One single server contains all versioned files.
- Problem: If the server goes down, no one can collaborate or save versions.

### Distributed (e.g., Git)
- Every user has a **full copy** of the repository including the entire history.
- If a server dies, any client repository can be used to restore it.
- Work offline and sync when convenient.



## Introduction to Git

Git is the world's most popular open-source distributed version control system.

### Key Facts:
- **Creator:** Linus Torvalds (the creator of Linux).
- **Core Focus:** Speed, data integrity, and support for non-linear workflows.
- **Performance:** Most operations are local, making them incredibly fast.
- **Safety:** Everything is checksummed to ensure nothing is lost or corrupted.



## Git vs. GitHub: What's the Difference?

Many beginners confuse the two, but they are very different tools.

### Git (The Tool)
- Command-line tool installed on your computer.
- Manages the local version history of your files.
- Works without an internet connection.

### GitHub (The Platform)
- A cloud-based hosting service for Git repositories.
- Provides a graphical interface and social features.
- Enables collaboration between developers around the world.
- Offers features like Pull Requests, Issues, and Actions.



## Installing Git

Before we start, you need the tool on your machine.

### Windows
- Download from `git-scm.com`.
- Includes "Git Bash" (a terminal emulator).

### macOS
- Run `git --version` in Terminal. If not installed, it will prompt you.
- Or use Homebrew: `brew install git`.

### Linux
- Use your package manager.
- Ubuntu/Debian: `sudo apt install git`.
- Fedora: `sudo dnf install git`.



## Initial Configuration

Once installed, you must introduce yourself to Git. This information is attached to every commit you make.

```bash
## Set your name
git config --global user.name "Your Name"

## Set your email
git config --global user.email "your.email@example.com"
```

### Why do this?
Git needs to know who authored each change so teams can track contributions and communicate about specific code updates.



## Checking Your Configuration

You can verify your settings at any time.

```bash
## List all current settings
git config --list
```

### Levels of Config:
1. **System:** Settings for every user on the computer.
2. **Global:** Settings for you as a specific user (most common).
3. **Local:** Settings specific to a single project/repository.

*Local settings always override Global, which override System.*



## Basic Shell Commands for Git

Git is primarily used via the terminal. Here are the essentials:

- `pwd`: Print Working Directory (where am I?).
- `ls`: List files in the current folder (`ls -a` shows hidden files).
- `cd folder_name`: Change Directory.
- `mkdir new_folder`: Create a new folder.
- `touch file.txt`: Create a new empty file.
- `rm file.txt`: Delete a file.

Mastering these basics makes using Git much smoother!



## Initializing a New Repository

Starting a project with Git is simple. Go to your project folder in the terminal and type:

```bash
git init
```

### What happens?
- Git creates a hidden folder named `.git`.
- This folder stores all the tracking information and history.
- **Do not delete the .git folder** unless you want to stop tracking the project history.



## The Three Stages of Git

Understanding these three areas is crucial for using Git correctly.

1. **Working Directory:** Where you modify your files.
2. **Staging Area (Index):** A "prep" area where you list the changes you want to include in the next save.
3. **Git Directory (Repository):** Where Git permanently stores the changes as snapshots (commits).

### The Flow:
Modify File → `git add` (Stage) → `git commit` (Save to Repository).



## Git Status: Your Best Friend

Not sure what's going on? Run:

```bash
git status
```

### It tells you:
- Which branch you are on.
- Files that have been modified but not yet staged.
- Files that are staged and ready to be committed.
- New files that Git isn't tracking yet (Untracked files).

*Run this command frequently! It prevents mistakes.*



## Git Add: Staging Your Work

Before you save, you must tell Git which changes to include.

```bash
## Stage a specific file
git add filename.txt

## Stage all changes in the current directory
git add .
```

### The Concept:
Think of `git add` like putting items into a box before sealing it. You can keep adding more items until you're ready to "ship" the commit.



## Git Commit: Saving Your Snapshot

Once your changes are staged, save them permanently with a commit.

```bash
git commit -m "Add a clear description of what you did"
```

### The `-m` Flag:
- This stands for "message". 
- Always write descriptive messages (e.g., "Fix login button bug" instead of "update" or "asdf").
- A commit is a snapshot of your project at that specific point in time.



## Git Log: Viewing History

Want to see what you've done in the past?

```bash
git log
```

### What you see:
- Commit ID (a unique hash code).
- Author info.
- Date and time.
- The commit message.

*To see a condensed version, use:* `git log --oneline`



## Git Diff: Seeing Exact Changes

Before you stage or commit, you might want to see the specific lines of code that changed.

```bash
## Compare working directory to staging area
git diff

## Compare staging area to last commit
git diff --staged
```

### Color Coding:
- **Red lines** (-) were removed.
- **Green lines** (+) were added.



## Gitignore: Keeping Projects Clean

Some files should never be tracked by Git (e.g., passwords, node_modules, temp files). Create a file named `.gitignore` in your project root.

### Example .gitignore content:
```text
## Ignore log files
*.log

## Ignore a specific folder
node_modules/

## Ignore sensitive environment files
.env
```

*Git will now ignore these files completely.*



## Setting Up GitHub

1. Create a free account at `github.com`.
2. Set up your profile (Avatar, Bio).
3. Secure your account using Two-Factor Authentication (2FA).

### Connecting to GitHub:
You can connect your local machine to GitHub using **HTTPS** (easier for beginners) or **SSH** (more secure, no password needed every time).



## SSH Keys: Secure Access

SSH (Secure Shell) keys allow you to connect to GitHub without typing your username and password every time.

### The Pair:
- **Private Key:** Stays on your computer (never share this!).
- **Public Key:** Uploaded to your GitHub settings.

### Generation:
`ssh-keygen -t ed25519 -C "email@example.com"`

*Follow the prompts, then copy the content of the `.pub` file to your GitHub account settings.*



## Remote Repositories

A "Remote" is a version of your project hosted on the internet (GitHub).

### Adding a Remote:
```bash
git remote add origin https://github.com/user/repo-name.git
```

- `origin` is the standard name for your primary remote repository.
- You can now sync your local work with this online version.



## Git Push: Sending Changes to GitHub

After committing locally, update the remote repository.

```bash
## Push the 'main' branch to 'origin'
git push -u origin main
```

### The `-u` Flag:
- Stands for "upstream".
- Remembers your preferences so next time you can just type `git push`.
- Your code is now live on GitHub!



## Git Pull: Getting Updates from Others

If your teammates pushed changes to GitHub, you need to bring them to your local machine.

```bash
git pull origin main
```

### What it does:
1. Downloads the new data from GitHub (**fetch**).
2. Merges it into your current local files (**merge**).

*Always pull before you start working to ensure you have the latest version.*



## Git Clone: Downloading Projects

Found a project on GitHub you want to work on?

```bash
git clone https://github.com/user/repo-name.git
```

### What happens?
- It creates a new folder.
- Downloads the entire project history.
- Sets up the remote connections automatically.
- You're ready to start working immediately!



## Understanding Branches

Branching is Git's most powerful feature. It allows you to "branch off" from the main code to work on a specific feature or fix.

### The Standard Approach:
- **Main Branch:** Holds the stable, production-ready code.
- **Feature Branches:** Work on new ideas here. If they work, merge them. If they fail, delete the branch without hurting the main project.

*Think of it as parallel universes for your code.*



## Creating and Switching Branches

```bash
## Create a new branch
git branch feature-name

## Switch to that branch
git checkout feature-name

## Shortcut: Create and switch at once
git checkout -b feature-name
```

### Visualizing:
Your project now has two separate paths of development. Changes on `feature-name` won't appear on `main` until you merge them.



## Managing Branches

```bash
## List all local branches
git branch

## Show which branch you're currently on
git status

## Delete a branch (after merging)
git branch -d branch-name
```

### Tip:
Keep branch names short and descriptive (e.g., `fix-header`, `user-auth`). Use hyphens instead of spaces.



## Git Merge: Combining Work

When your feature is finished and tested, bring it back into the main code.

1. Switch back to main: `git checkout main`
2. Update main: `git pull origin main`
3. Merge the feature: `git merge feature-name`

### Fast-Forward Merge:
If `main` hasn't changed since you branched off, Git simply moves the pointer forward. It's clean and instant.



## Handling Merge Conflicts

Sometimes two people change the **same line** in the same file. Git won't know which one to keep and will stop the merge.

### How to Fix:
1. Open the conflicted file in your code editor.
2. Look for the markers: `<<<<<<< HEAD`, `=======`, and `>>>>>>> branch-name`.
3. Choose the code you want to keep and remove the markers.
4. Stage (`git add`) and Commit to finish the merge.

*Conflicts are normal! Don't panic.*



## Forking: A GitHub Specific Tool

**Forking** is not a Git command, but a GitHub feature.

### The Workflow:
1. You find an open-source project you love.
2. Click "Fork" to create your own personal copy on your GitHub account.
3. Clone your fork to your computer.
4. Make changes and push back to your fork.

*This allows you to propose changes to projects you don't "own".*



## Pull Requests (PRs)

A Pull Request is a way to ask the owner of a project to "pull" your changes into their code.

### The Steps:
1. Push your feature branch to your GitHub fork.
2. Go to the original project page on GitHub.
3. Click "New Pull Request".
4. The maintainer reviews your code, suggests changes, and eventually "Merges" it.

*PRs are the heart of open-source collaboration.*



## Git Stash: Temporary Storage

Need to switch branches but your work isn't ready to commit yet?

```bash
## Save your messy changes for later
git stash

## Switch branches, do work, switch back...

## Bring your changes back
git stash pop
```

### Analogy:
It's like having a drawer where you can quickly throw your tools to clear the workbench, and then take them out exactly where you left off later.



## Git Rebase: Cleaning Up History

Rebasing is a way to move your entire branch so it starts from a newer commit on the main branch.

### Why Rebase?
- Keeps a linear project history.
- Avoids unnecessary "merge commits".
- Makes the history easier to read.

*Use with caution! Never rebase branches that you have shared with others on GitHub.*



## Git Reset: Fixing Mistakes

Accidentally committed something you didn't mean to?

```bash
## Undo the last commit but keep your changes in the folder
git reset --soft HEAD~1

## DELETE the last commit and all its changes (DANGEROUS)
git reset --hard HEAD~1
```

*Be very careful with `--hard`. It permanently deletes uncommitted work.*



## Git Checkouts: Time Travel

Git allows you to look at your project as it existed in the past.

```bash
## Temporarily look at a specific commit
git checkout commit-id-here

## Look at a specific file from a previous commit
git checkout commit-id-here filename.txt
```

### Going Back:
To return to the present, just type `git checkout main`.



## Git Aliases: Working Faster

Tired of typing long commands? Create shortcuts!

```bash
## 'git s' instead of 'git status'
git config --global alias.s status

## 'git co' instead of 'git checkout'
git config --global alias.co checkout
```

### Pro Tip:
Many developers have an alias like `gl` for a pretty, colorful, one-line graphical log history.



## Git and VS Code Integration

Visual Studio Code (VS Code) has built-in Git support that makes version control visual and easy.

### Features:
- **Source Control Tab:** See all modified files.
- **Side Highlights:** Blue bars for modified lines, green for new ones.
- **One-click Buttons:** Commit, Push, and Pull without using the terminal.
- **Timeline View:** See the history of a specific file.



## The GitHub CLI (gh)

GitHub also has a command-line tool (`gh`) that lets you manage GitHub features without leaving your terminal.

```bash
## Create a new repository on GitHub
gh repo create

## View open pull requests
gh pr list

## Create a new issue
gh issue create
```

*It's a huge time-saver for power users.*



## Markdown for GitHub

GitHub uses **Markdown** to format text in README files, issues, and pull requests.

### Basics:
- `# Heading 1`
- `**Bold Text**`, `*Italic Text*`
- `[Link Name](https://url.com)`
- `![Image Alt](image-url)`
- `- Bullet points`
- `1. Numbered lists`

*A great README.md file is the face of your project!*



## Essential Git Workflow (The Cycle)

Most days, your workflow will look like this:

1. `git pull origin main` (Get latest code)
2. `git checkout -b new-feature` (Start work)
3. *Write Code*
4. `git add .` (Stage)
5. `git commit -m "Work done"` (Save)
6. `git push origin new-feature` (Upload)
7. *Open Pull Request on GitHub* (Collaborate)



## Collaboration Best Practices (1/2)

To work successfully on a team:

- **Commit Small and Often:** Don't wait three days to commit. Small commits are easier to review and undo.
- **Write Good Commit Messages:** "Fix stuff" is bad. "Fix centering of the navbar logo" is good.
- **Sync Regularly:** Pull from the main branch daily to avoid massive merge conflicts later.



## Collaboration Best Practices (2/2)

- **Never Commit Sensitive Data:** No passwords, API keys, or personal details. Use `.env` files and `.gitignore`.
- **Review Each Other's Code:** Use Pull Requests to catch bugs before they reach the main branch.
- **Keep Your Main Branch Clean:** Only merge code that is tested and working.



## Removing Sensitive Data

If you accidentally commit a password:
1. **Change the password immediately.** Once it's in Git, it's in the history even if you delete the file later.
2. Use tools like `git-filter-repo` to scrub the history (advanced).
3. Contact GitHub support if the data is highly sensitive and you can't remove it yourself.

*Better safe than sorry: Always check `git status` before adding!*



## Git Tagging: Marking Versions

Tags are used to mark specific points in your project's history as being important, typically for releases (v1.0, v2.0).

```bash
## Create a tag
git tag -a v1.0 -m "Release version 1.0"

## Send tags to GitHub
git push origin --tags
```

### GitHub Releases:
GitHub can use these tags to create a "Release" page with downloadable ZIP files and release notes.



## GitHub Issues and Projects

GitHub isn't just for code; it's also for project management.

- **Issues:** Track bugs, feature requests, and tasks.
- **Labels:** Categorize issues (e.g., "bug", "help wanted").
- **Milestones:** Groups of issues for a specific deadline.
- **Project Boards:** Trello-like Kanban boards to track progress (To Do, In Progress, Done).



## GitHub Pages: Fast Hosting

You can host your website directly on GitHub for free!

1. Create a repository with your HTML/CSS/JS files.
2. Go to **Settings > Pages**.
3. Select your branch and click "Save".
4. GitHub gives you a URL (e.g., `username.github.io/repo-name`).

*Perfect for portfolios and project documentation.*



## Solving Common "Oops" Moments

- **"I committed to the wrong branch":** Use `git stash`, switch branches, then `git stash pop`.
- **"I made a typo in my last commit message":** Use `git commit --amend -m "New message"`.
- **"I deleted a file by mistake":** Use `git checkout filename.txt` to bring it back from the last commit.
- **"Git is acting weird":** Type `git status`. It usually tells you how to fix the problem!



## Git Cheat Sheet (The Vital Few)

Keep these commands handy:
- `git init`: Start a project.
- `git clone`: Copy a project.
- `git status`: See what's happening.
- `git add .`: Stage everything.
- `git commit -m ""`: Save.
- `git push`: Upload.
- `git pull`: Download.
- `git branch`: Manage branches.
- `git checkout`: Switch branches.
- `git merge`: Combine work.



## Summary: Your Git Journey Begins

### Remember:
- Git is a tool for **Local** version control.
- GitHub is a **Remote** platform for collaboration.
- Master the **Add-Commit-Push** cycle first.
- Branching is your secret weapon for safe experimentation.
- Mistakes are part of the learning process—Git is designed to help you fix them.



## Conclusion & Next Steps

### Where to go from here:
1. Practice by creating your first repository today.
2. Contribute to an open-source project (look for "good first issue").
3. Explore GitHub Actions for automation.
4. Keep learning and keep coding!

**Thank you for following this guide!**
*Git and GitHub Book - Steps to Success*
