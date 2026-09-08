# Introduction to Git Version Control

**Build the skills to review and record changes with confidence in your local development environment.**

This guide is based on [Gitバージョン管理_入門_公開用_v1.1.pptx](slides/Gitバージョン管理_入門_公開用_v1.1.pptx) and has been reorganized for programmers learning Git for the first time. It covers the main topics from the original slides, with local exercises and review habits that support professional development work.

Learning Git involves more than memorizing commands. You should be able to explain your changes and record only the changes that belong in each commit.

[日本語版](Git_バージョン管理_入門.md)

## Learning Objectives

By the end of this guide, you should be able to explain and perform the following in your own words:

- Distinguish the roles of Git and GitHub.
- Explain the relationship between the working tree, index, and repository.
- Review file changes and commit them in meaningful units.
- Create a branch and merge its changes into main.
- Read a conflict, resolve it, and complete the integration.

**Exercise environment: Git Bash on Windows.** Use a text editor you are comfortable with to edit files. All exercises run on your own PC, and no GitHub account is required. Run the commands in each code block from top to bottom. Angle brackets, such as `<filename>`, indicate a placeholder to replace with an actual value.

## 1. Version Control as a Foundation for Professional Work

![History icon from the original slides](images/history.png)

### 1.1 What Is Version Control?

Version control records changes to files so that you can review and compare them later. When you modify a program, it helps you trace what changed, who recorded it, and why the change was made.

For example, if another problem appears after a bug fix, comparing the states before and after the fix can help you investigate the cause. You can also retrieve a previously recorded state when needed.

Using filenames such as `report_v1`, `report_final`, and `report_final2` makes it increasingly difficult to identify the correct version. Git lets you keep the same filename while recording meaningful checkpoints in its history.

> **A professional habit:** Leave a history that your future self and your colleagues can understand. Git cannot determine why you made a change, so explain the reason in the commit message.

In general, the content you can reliably retrieve with Git is content you have recorded. Git does not automatically back up every saved edit or untracked file.

### 1.2 Git and GitHub

| Aspect | Git | GitHub |
| --- | --- | --- |
| Role | Software that manages file change history | A service that hosts and shares Git repositories and supports collaboration |
| Main location | Your PC or a server | An online service |
| Typical operations | Recording changes, reviewing differences, creating branches, integrating changes | Sharing repositories, reviewing changes, managing issues |
| Required for local learning | Yes | No |

Commits you create with Git are first recorded on your own PC. Sharing them through GitHub or another service requires a separate operation to send them.

### 1.3 Centralized and Distributed Version Control

| Model | Where history is stored | When offline | Examples |
| --- | --- | --- | --- |
| Centralized | A central server manages the history | You can edit files, but operations such as committing to the server are restricted | Subversion (SVN), CVS |
| Distributed | Each user also has a local repository | You can commit and inspect history locally | Git, Mercurial |

Git remains a distributed system even when one person uses it only on a local PC. A normal clone also retrieves history, although some cloning methods limit the history retrieved.

### 1.4 Git's Features

Git is an open-source version control system created in 2005 in the context of Linux kernel development. It is available on Windows, macOS, and Linux.

Many operations, including recording history and reviewing differences, run locally. Branches also let you separate work from the main development line and try changes independently.

Git treats the state of a set of files at a commit as a snapshot. This does not mean it simply duplicates every file each time. Internally, it manages storage efficiently, including reusing unchanged content. Reference: [Official Git book: What is Git?](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)

## 2. The Three Areas to Understand First

### 2.1 Edit, Select, and Record

| Area | Role | Example: changing README.md |
| --- | --- | --- |
| Working tree | Where you actually edit files | Add an explanation in your editor and save it |
| Index (staging area) | Where you prepare the state to include in the next commit | Register the content with `git add README.md` |
| Local repository | Where commits and history are stored | Record the content with `git commit` |

```text
Working tree          Index                  Local repository
Edit and save    →    Select content    →    Record in history
                 git add                git commit
```

In a typical repository, the `.git` directory inside the project stores history, configuration, and other Git data. Do not directly edit or delete its contents during these exercises.

### 2.2 Saving, Adding, and Committing Are Separate Operations

Saving in your editor updates the working file. `git add` registers its content at that moment for the next commit. `git commit` records the staged content in history.

**Edits made after add are not staged automatically.** A single file can have both staged and unstaged changes. To include the later edits in the next commit, review the differences and run add again.

### 2.3 Reading the State with status

```bash
git status
```

| Output | Meaning | What to consider next |
| --- | --- | --- |
| `Untracked files` | New files that Git is not yet tracking | Add them if they belong under version control |
| `Changes not staged for commit` | Tracked files have unstaged changes | Review the content with diff |
| `Changes to be committed` | Changes are prepared for the next commit | Review the staged differences |
| `nothing to commit, working tree clean` | No unrecorded changes are reported by normal status output | Continue to the next task |

`git status` shows the state at the moment you run it. It is not a command that stays open to continuously monitor changes.

### 2.4 What diff Compares

![Difference inspection icon from the original slides](images/diff.png)

| Command | Comparison | Main purpose |
| --- | --- | --- |
| `git diff` | Index and working tree | Review changes before add |
| `git diff --staged` | HEAD and index | Review changes going into the next commit |
| `git diff HEAD` | HEAD and working tree | Review working file changes since the last commit |
| `git diff <commitA> <commitB>` | Two specified commits | Compare recorded states |

`HEAD` normally points to the tip commit of the current branch. Before the first commit exists, there is no HEAD commit to compare against.

Normal `git diff` output does not show the contents of untracked files. Review a new file in your editor, then use `git diff --staged` after adding it. In a diff, a leading `-` marks a removed line and a leading `+` marks an added line.

## 3. Installing and Configuring Git

### 3.1 Check the Installation

Visit the [official Git website](https://git-scm.com/) for installation instructions for your operating system. On Windows, install Git for Windows and open Git Bash. On a company PC, follow any installation procedures and settings specified by your organization.

```bash
git --version
```

If the output begins with `git version ...`, the command is available. This guide assumes Git 2.28 or later, with support for `git switch`, `git restore`, and `git init -b main`.

### 3.2 Set Your Commit Identity

The following name and email address are for practice. For professional work, replace them with the identity specified by your organization.

```bash
git config --global user.name "Git Learner"
git config --global user.email "learner@example.com"
```

This information is recorded in commits. It is not a GitHub login setting. `--global` applies the setting to the current user on this PC.

Check the settings:

```bash
git config --global --get user.name
git config --global --get user.email
```

To use a different identity for a project, run the configuration commands inside its repository without `--global`. The local settings take precedence in that repository.

```bash
git config user.name "Project Developer"
git config user.email "developer@example.com"
```

The local configuration commands above are supplementary examples to use after creating a repository in the later exercise. You do not need to run every example.

On Windows, the global configuration file is usually at `C:\Users\YourUsername\.gitconfig`. Use `git config --list --show-origin` to see the settings and where they came from.

`git config --global --unset user.name` removes only the global `user.name` setting. It does not remove the entire configuration.

## 4. Exercise: Your First Commit

### 4.1 Create a Dedicated Practice Folder

Run the following in Git Bash. If a folder with this name already exists, choose a new name. Do not run these commands inside an existing work project.

```bash
cd ~
mkdir git-local-practice
cd git-local-practice
git init -b main
git status
```

`git init` initializes a new local repository. `-b main` sets the initial branch name to main. When the output says `No commits yet`, no commits have been created.

### 4.2 Create README.md

Open the practice folder in your editor and save `README.md` in UTF-8 with the following content:

```markdown
# Git Local Practice

This is a practice repository for learning local Git operations.
```

Return to Git Bash and check the state:

```bash
git status
```

If `README.md` appears under `Untracked files`, Git has detected the new file.

### 4.3 Stage the File and Review Its Contents

```bash
git add README.md
git status
git diff --staged
```

You should see `new file: README.md` under `Changes to be committed`. Check the diff to confirm that the text you wrote will be added.

The command `git add .`, shown in the original slides, stages changes under the current directory. Start by specifying filenames so that you practice choosing exactly what to stage.

### 4.4 Create the First Commit

```bash
git commit -m "Add practice README"
git status
git log --oneline
```

The `-m` option supplies the commit message. After a successful commit, the log shows a short commit ID and the message. Commit IDs will differ between environments.

Confirm that your message appears in the log and that status reports a clean working tree. You have now created history on your PC.

### 4.5 Make a Change and Create a Second Commit

Append the following content to README.md and save it:

```markdown

## Learning Topics

- Review changes and create commits
```

```bash
git status
git diff
git add README.md
git diff --staged
git commit -m "Add learning topics to README"
git log --oneline
```

The exercise is complete when the log shows two commits. If you cannot return from a log or diff display, a pager may be open. Press `q` to exit it.

### 4.6 Professional Commit Habits

Record changes in units that you can explain later. Combining a bug fix with unrelated appearance changes in one commit makes reviewing the work and investigating problems more difficult.

| Vague message | Informative message |
| --- | --- |
| Fix | Add startup instructions to README |
| Bug fix | Fix calculation stopping on empty input |
| Update | Add branch operations to learning topics |

Before committing, check the diff for accidental deletions and unnecessary files. If you changed program code, also check its behavior and run the necessary tests.

## 5. Exercise: Separate Your Work with a Branch

A branch gives a name to a line of development and lets you separate your work. In this guide, the main development branch is `main`, and the branch for adding documentation is `feature/readme-guide`.

### 5.1 Create and Switch to a Branch

After completing Chapter 4, confirm that your working tree is clean and run:

```bash
git status
git switch -c feature/readme-guide
git branch
```

`-c` creates the branch and switches to it. In `git branch` output, an asterisk (`*`) marks the current branch.

| Purpose | Command used in this guide | Equivalent command in the original slides |
| --- | --- | --- |
| Create a branch only | `git branch feature/readme-guide` | Same command |
| Switch to an existing branch | `git switch main` | `git checkout main` |
| Create and switch | `git switch -c feature/readme-guide` | `git checkout -b feature/readme-guide` |

This table explains equivalent operations. You do not need to create the same branch again. Uncommitted changes may carry over when switching branches or prevent the switch, so commit a meaningful unit of work before switching.

### 5.2 Record a Change on the Working Branch

Add the following item under Learning Topics in README.md:

```markdown
- Separate work with branches and merge changes
```

```bash
git diff
git add README.md
git diff --staged
git commit -m "Add branch operations to learning topics"
git switch main
```

After returning to main, reopen README.md in your editor. The new item is not there yet because the commit was recorded on the working branch.

### 5.3 Integrate the Change into main

```bash
git merge feature/readme-guide
git log --oneline --graph --all
git status
```

`git merge` incorporates changes from the specified branch **into your current branch**. Here, you switched to main first, so the changes are integrated into main.

Because no new commits have been added to main since the branch was created, the output normally says `Fast-forward`. Git moves main to the tip of the working branch without creating a new merge commit.

```text
Before: A──B  main
           └──C  feature/readme-guide

After:  A──B──C  main, feature/readme-guide
```

After checking the result, you can delete the merged working branch name:

```bash
git branch -d feature/readme-guide
```

The merged commits remain in main's history.

## 6. Understanding Merge and Rebase

### 6.1 Merging Diverged History

When main and the working branch both have their own new commits, a normal merge creates a merge commit connecting the two histories.

```text
A──B──C────M  main
    └──D──┘
```

`M` is the merge commit. The history preserves the branching and integration. A conflict can occur, for example, when the same part of a file was changed differently on the two branches.

### 6.2 Rebase Replays Commits on a Different Base

Rebase reapplies the working branch's changes in order, starting from another tip. The commands below illustrate the concept; they are not commands to run as a continuation of Chapter 5.

```bash
git switch feature/example
git rebase main
```

```text
Before: A──B──C  main
           └──D──E  feature/example

After:  A──B──C  main
              └──D'──E'  feature/example
```

`D'` and `E'` are commits created by reapplying the changes. Their commit IDs change. This operation alone does not advance main.

| Aspect | merge | rebase |
| --- | --- | --- |
| Original history | Integrates while preserving the original commits | Recreates the affected commits on another base |
| Resulting shape | May preserve branching | Can make the working history linear |
| Commits | Adds none for a fast-forward; creates a merge commit for a normal merge of diverged history | Creates replayed commits; some may be omitted depending on their content |
| Approach for beginners | Learn to use it reliably first | Understand its meaning and practice on an unshared working branch |

Rewriting shared history can make it inconsistent with your colleagues' histories. Check your team's workflow before rebasing a shared branch.

## 7. Exercise: Resolve a Conflict

A conflict occurs when Git cannot automatically decide how to combine changes. It can happen even when you work alone locally if you change the same content differently on separate branches.

### 7.1 Make Different Changes to the Same Line

In the practice repository from Chapter 5, confirm that you are on main and that the working tree is clean:

```bash
git switch main
git status
```

Create `message.txt` in your editor and save this single line:

```text
Hello
```

```bash
git add message.txt
git commit -m "Add greeting message"
git switch -c feature/greeting
```

On feature/greeting, replace the line in message.txt with the following and save it:

```text
Hello, let's learn Git.
```

```bash
git add message.txt
git commit -m "Add learning invitation to greeting"
git switch main
```

On main, replace the same line with the following and save it:

```text
Hello, welcome to the development team.
```

```bash
git add message.txt
git commit -m "Add team welcome to greeting"
git merge feature/greeting
```

This exercise produces `CONFLICT` output because the changes to the same line conflict.

### 7.2 Read the Markers and Decide the Final Content

```bash
git status
```

Open message.txt. It normally contains markers in the following format. Depending on your settings, the common original content may also appear.

```text
<<<<<<< HEAD
Hello, welcome to the development team.
=======
Hello, let's learn Git.
>>>>>>> feature/greeting
```

In this merge, the upper section is the content from the current main branch, and the lower section is from feature/greeting, the branch being merged. These sections do not mean "me" and "someone else."

Read the intent of both changes, replace the entire file with the following line, and save it. Remove the `<<<<<<<`, `=======`, and `>>>>>>>` markers as well.

```text
Hello, welcome to the development team. Let's learn Git.
```

### 7.3 Record the Resolution

```bash
git add message.txt
git diff --staged
git status
git commit -m "Resolve greeting conflict by combining welcome and learning invitation"
git log --oneline --graph --all
git status
```

Confirm that the final content is correct and that the working tree is clean. For conflicts in program code, check the integrated behavior as well as removing the markers.

To cancel a merge that is still in progress, use `git merge --abort`. This does not undo a merge that has already completed.

For a conflict during a rebase, edit the file and add it, then run `git rebase --continue`. To cancel the rebase, use `git rebase --abort`. Merging and rebasing require different commands to continue after resolving a conflict.

## 8. Basic Recovery from Mistakes

### 8.1 Unstage a File

This example applies when you accidentally stage README.md in a repository that already has its first commit:

```bash
git restore --staged README.md
```

Your edits in the working tree remain. Use this to take a file out of the next commit temporarily while you review it again.

### 8.2 Discard Unstaged Edits

```bash
git diff -- README.md
git restore README.md
```

`git restore README.md` resets the working file to the content in the index and discards unstaged edits. **Do not run it if you want to keep those edits.** Git may not be able to recover content that you have not committed.

### 8.3 Common Messages and What to Check

| Message or situation | What to check |
| --- | --- |
| `git: command not found` | Reopen Git Bash after installation and check the installation |
| `not a git repository` | Use `pwd` to check your location, then `cd` into the practice repository |
| `Author identity unknown` | Check user.name and user.email |
| `nothing added to commit` | Use status to check whether you saved and added the intended file |
| A new file is missing from `git diff` | Read the untracked file in your editor, then review the staged diff after add |
| You cannot switch branches | Inspect uncommitted changes and commit the changes you need |
| An editor opens during commit | Without `-m`, Git opens an editor for the message; this guide uses `-m` |

If you lose track of the current state, start by reading `git status`. Check what remains before attempting to move forward with forced deletion or history rewriting.

## 9. Professional Practice: Files to Exclude

`.gitignore` specifies untracked files that should be excluded from normal tracking. Create it in the project root.

```gitignore
# Local settings and secrets
.env

# Generated output and logs
build/
*.log
```

Adjust the exclusions to your programming language and team conventions. The `.gitignore` file itself is normally committed and shared.

Adding an already tracked file to `.gitignore` does not stop Git from tracking it. It also does not erase information from history. Check for files containing API keys or passwords before the first add.

## 10. An Introduction to Remote Collaboration

This chapter summarizes the original slides' remote collaboration material. Do not run these commands as part of the local exercises.

A remote repository is another repository registered for purposes such as sharing work. It can be hosted on services such as GitHub, GitLab, or Bitbucket.

| Example command | Purpose |
| --- | --- |
| `git clone <URL>` | Retrieve a repository and begin working locally |
| `git fetch origin` | Retrieve history information from the remote |
| `git push origin main` | Send local main's commits and update main on the remote |
| `git pull --ff-only` | Incorporate retrieved updates only when a fast-forward is possible |

`origin` is a commonly used name for a registered remote. Running these commands requires the appropriate remote, tracking, and authentication settings.

**push sends committed history.** It does not send changes that are only staged or edits that have not been committed. Reference: [Official git-push reference](https://git-scm.com/docs/git-push)

pull retrieves and integrates changes. The integration method depends on options and configuration and can involve merging or rebasing. `--ff-only` stops if the histories have diverged. If it stops, inspect the history and your team's policy. Reference: [Official git-pull reference](https://git-scm.com/docs/git-pull)

`checkout`, `switch`, and `diff` also work locally. You do not need to learn them as commands for communicating with a remote.

## 11. Tools That Help You Work with Git

The original slides introduce the following tools:

| Tool | Main purpose |
| --- | --- |
| Git Bash | An environment for running Git and shell commands on Windows |
| Git GUI | A graphical tool for operations such as staging and committing |
| Sourcetree | A Git client for viewing history and branches visually |
| TortoiseGit | A Git client for performing operations from Windows Explorer |

When using an editor or GUI, keep track of whether an action edits, stages, or commits your work. This guide uses Git Bash to help you understand and describe the underlying state.

## 12. Command Quick Reference

| Task | Command |
| --- | --- |
| Check the version | `git --version` |
| Create a new repository | `git init -b main` |
| Check the state | `git status` |
| View unstaged differences | `git diff` |
| View changes prepared for commit | `git diff --staged` |
| Stage a specific file | `git add <filename>` |
| Commit with a message | `git commit -m "Describe the change"` |
| Show a compact history | `git log --oneline` |
| Show history including branches | `git log --oneline --graph --all` |
| List branches | `git branch` |
| Create and switch to a branch | `git switch -c <branch-name>` |
| Switch to an existing branch | `git switch <branch-name>` |
| Merge into the current branch | `git merge <branch-name>` |
| Delete a merged branch | `git branch -d <branch-name>` |
| Unstage a file | `git restore --staged <filename>` |
| Cancel an in-progress merge | `git merge --abort` |

## 13. Check Your Understanding

1. Does saving a file alone record it in Git history?
2. If you edit a file again after add, which state goes into the commit?
3. Which command shows the changes that will go into the next commit?
4. Which branch should you be on when merging feature's changes into main?
5. Does a fast-forward create a new merge commit?
6. After removing conflict markers, what should you check before recording the result?
7. Can push share content that you have only added to the staging area?

### Suggested Answers

1. No. You need to stage and commit it.
2. The content from the last add. Run add again to include later edits.
3. `git diff --staged`.
4. Switch to main, then run `git merge <working-branch-name>`.
5. No. It moves the receiving branch forward.
6. Check that the combined content has the correct meaning and behavior, then add and commit it.
7. No. You must commit it first.

### Completion Checklist

- [ ] I can explain the three areas Git uses.
- [ ] I recorded the initial README and its subsequent change in separate commits.
- [ ] I integrated my working branch's change into main.
- [ ] I read a conflict, resolved it to the intended content, and completed the merge.
- [ ] I can explain why I review differences before committing.

In everyday work, use status after editing to check the state, read the changes with diff, and add the changes you need. Finally, review the staged differences and commit. Make this sequence a basic habit for taking responsibility for your changes.

## Appendix: Source Mapping and Terms of Use

### Mapping to the Original Slides

| Original slides | Coverage in this guide |
| --- | --- |
| 1–12 | Learning Objectives and Chapter 1: version control, Git and GitHub, centralized and distributed systems, Git's features |
| 13–18 | Chapters 2 and 4: the three areas, checking state, staging, committing |
| 19–22 | Chapters 2 and 10: reviewing differences, remote collaboration, checkout equivalents |
| 23–28 | Chapters 5–6: branches, fast-forward, merge, rebase |
| 29–30 | Chapter 7: conflicts |
| 31–32 | Chapter 11: tools and clients |
| 33–38 | Chapters 3–5 and 12–13: initial setup, practice, review |
| 39 | Copyright and terms of use below |

The steps for starting with an empty repository, exercise files, restore operations, .gitignore, and knowledge check are supplementary learning material. The original example of a distributed system, "GitHub," was corrected to "Git," and the push description was corrected to sending committed history. The explanations of pull integration methods and the commits involved in merge and rebase were also clarified to state the relevant conditions.

The images are embedded assets from the original slides. `images/history.png` corresponds to `image2.png`, used on slide 6 and elsewhere. `images/diff.png` corresponds to `image17.png` on slide 22. Keep the Markdown files alongside the shared images folder so that both language versions display the images.

### Copyright and Terms of Use (Translation of Public Edition Version 1.1)

Public Viewing and Non-Commercial Study License Version 1.1

Copyright © 2026 Shinsuke Oouchi. All rights reserved.

Established: August 30, 2026 / Revised: September 8, 2026

**Permitted Uses**

- Online publication of this public edition by the copyright holder or an authorized publisher, and viewing by the general public
- Private, non-commercial study by individuals, including viewing and saving copies from a public repository
- Modification and addition by instructors, and reproduction and distribution to instructors and participants, to the extent necessary for classes conducted by Heatwave Co., Ltd. (ヒートウェーブ株式会社)
- Projection and screen sharing within those classes, with this notice retained

**Prohibited Uses**

- Unauthorized reposting, republication, or public transmission, including posting on social media, video services, or public repositories
- Unauthorized publication of modified versions or redistribution to anyone other than class participants
- Sale, lending, sublicensing, or use for commercial, advertising, or promotional purposes
- Removal or modification of copyright notices, or extraction and reuse of third-party materials

**This public edition is made available to the general public by the copyright holder. Third parties must obtain permission to repost, redistribute, or publish modified versions.**

Third-party materials are subject to the terms of their respective rights holders. Uses beyond those listed above require prior written permission from the copyright holder.

This material is provided as is, without any guarantee of the accuracy or completeness of its content. The copyright holder accepts no liability for damages arising from its use.

These terms are governed by Japanese law. Permission to publish does not constitute a waiver of copyright or permission to use the included assets freely.
