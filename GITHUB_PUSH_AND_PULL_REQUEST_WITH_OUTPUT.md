# Pushing Source Code to GitHub and Creating a Pull Request

This guide explains what GitHub is, why it is useful, and how to publish source
code and propose changes with a pull request. The commands use Git Bash, PowerShell,
or another terminal with Git installed. Replace values in `<angle brackets>` with
your own values; do not type the angle brackets.

Expected outputs below are examples. Git versions, operating systems, repository
contents, and GitHub account names can change the exact wording and identifiers.

## 1. What are Git and GitHub?

- **Git** is a version control system. It records changes to files as commits,
  lets you compare versions, and supports working on separate branches.
- **GitHub** is a website and collaboration platform for hosting Git repositories.
  It adds code review, pull requests, issue tracking, project collaboration, and
  automation around Git.
- A **repository (repo)** is a project's files and their Git history.
- A **commit** is a named snapshot of changes, usually accompanied by a short
  explanation.
- A **branch** is an independent line of work. The default branch is commonly
  named `main`.
- A **remote** is a named URL for another copy of a repository. `origin` is the
  conventional name for the GitHub remote.
- A **pull request (PR)** proposes merging changes from one branch into another.
  Reviewers can discuss changes and checks can run before the PR is merged.

## 2. Why use GitHub?

GitHub makes it easier to:

1. Keep an off-computer copy of source code and its history.
2. Collaborate without overwriting one another's work.
3. Review and discuss changes before they reach the default branch.
4. Track bugs and requested work with issues.
5. Run automated checks, such as tests, through GitHub Actions.
6. Share a project using a repository URL and document it with a README.

Git and GitHub are related but different: Git handles version history locally;
GitHub hosts a remote repository and provides collaboration features.

## 3. Prerequisites

- A GitHub account.
- Git installed (`git --version` should print a version).
- A source-code project in a local folder.
- Permission to create a repository in the GitHub account or organization.

Check the installed Git version:

```bash
git --version
```

Example output:

```text
git version 2.45.1.windows.1
```

Set your commit author information once on this computer. Use the name and email
you want recorded in your commits:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

These configuration commands normally complete silently. Verify the saved values:

```bash
git config --global --get user.name
git config --global --get user.email
```

Example output:

```text
Your Name
you@example.com
```

## 4. Create an empty GitHub repository

1. Sign in at <https://github.com>.
2. Select **New repository**.
3. Enter a repository name, such as `my-project`.
4. Choose whether it should be public or private.
5. If the project already exists on your computer, leave **Add a README file**,
   **Add .gitignore**, and **Choose a license** unchecked. This avoids creating
   a separate initial commit on GitHub that can complicate the first push.
6. Select **Create repository**.
7. Copy the repository URL. It will look like one of these:

```text
https://github.com/<username>/<repository>.git
git@github.com:<username>/<repository>.git
```

HTTPS may ask you to authenticate through Git Credential Manager or a browser.
For SSH, an SSH key must already be configured with GitHub. Do not put a password
or personal access token directly in a Git command or save it in source code.

## 5. Prepare the local project

Open a terminal in the project directory. For example, in PowerShell:

```powershell
Set-Location "C:\path\to\my-project"
```

Expected output: none. PowerShell changes the current directory.

Check the current directory and files:

```bash
git status
```

Example output before Git is initialized:

```text
fatal: not a git repository (or any of the parent directories): .git
```

If this folder is not already a Git repository, initialize it and create the
first commit:

```bash
git init
git branch -M main
git add .
git status
git commit -m "Add initial project files"
```

Example output (the commit hash, file count, and branch path will differ):

```text
Initialized empty Git repository in C:/path/to/my-project/.git/
On branch main
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.md
        new file:   src/app.js

[main (root-commit) a1b2c3d] Add initial project files
 2 files changed, 20 insertions(+)
 create mode 100644 README.md
 create mode 100644 src/app.js
```

Review `git status` before committing. Do not add secrets, credentials, API keys,
private configuration, build output, or large generated files. Add appropriate
entries to `.gitignore` before `git add .`.

If the project is already a Git repository with commits, do not run `git init`
or create an unnecessary initial commit. Check the current branch and history:

```bash
git branch --show-current
git log --oneline -5
git status
```

Example output:

```text
main
a1b2c3d Add initial project files
On branch main
nothing to commit, working tree clean
```

## 6. Connect the project to GitHub and push it

Replace the URL below with the URL copied from the newly created GitHub repository:

```bash
git remote add origin https://github.com/<username>/<repository>.git
git remote -v
git push -u origin main
```

Example output:

```text
origin  https://github.com/octocat/my-project.git (fetch)
origin  https://github.com/octocat/my-project.git (push)
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (5/5), done.
To https://github.com/octocat/my-project.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

`git remote add origin` names the GitHub repository as `origin`. The first push
uploads the `main` branch. `-u` records the upstream branch, so future pushes
from `main` can usually use the shorter command:

```bash
git push
```

Example output on a later push with no new commits:

```text
Everything up-to-date
```

If `origin` is already configured, inspect it instead of adding it again:

```bash
git remote -v
```

Example output:

```text
origin  https://github.com/octocat/my-project.git (fetch)
origin  https://github.com/octocat/my-project.git (push)
```

To correct an existing remote URL:

```bash
git remote set-url origin https://github.com/<username>/<repository>.git
```

Expected output: none. Confirm the new URL with `git remote -v`.

Refresh the GitHub repository page to confirm that the files and commit appear.

## 7. Make a change on a feature branch

Create a separate branch for a change rather than committing directly to `main`:

```bash
git switch main
git pull
git switch -c docs/update-project-guide
```

Example output:

```text
Already on 'main'
Already up to date.
Switched to a new branch 'docs/update-project-guide'
```

Edit or add the relevant project files. Then review and commit the change:

```bash
git status
git diff
git add <file-or-folder>
git diff --cached
git commit -m "Document project setup"
```

Example output (the changed file list and numbers depend on your edits):

```text
On branch docs/update-project-guide
Changes not staged for commit:
        modified:   README.md

[docs/update-project-guide d4e5f6a] Document project setup
 1 file changed, 8 insertions(+), 2 deletions(-)
```

Push the feature branch to GitHub:

```bash
git push -u origin docs/update-project-guide
```

Example output:

```text
To https://github.com/octocat/my-project.git
 * [new branch]      docs/update-project-guide -> docs/update-project-guide
branch 'docs/update-project-guide' set up to track 'origin/docs/update-project-guide'.
```

## 8. Open a pull request

### Using the GitHub website

1. Open the repository on GitHub.
2. Select **Compare & pull request** if GitHub displays it, or open **Pull
   requests** and select **New pull request**.
3. Select the target branch (`base`, usually `main`) and the source branch
   (`compare`, here `docs/update-project-guide`).
4. Review the displayed diff. Confirm that only the intended changes are included.
5. Enter a descriptive title and summary. Mention relevant tests or checks.
6. Select **Create pull request**.
7. Address review comments or failing checks by editing the branch, then commit
   and push again. The PR updates automatically.

Expected result in the browser: GitHub opens the new PR page with its title,
number, source and target branches, change summary, and review/check status.

### Optional: using GitHub CLI

Install and authenticate the GitHub CLI (`gh`) before using these commands:

```bash
gh auth login
gh pr create --base main --head docs/update-project-guide \
  --title "Document project setup" \
  --body "Explain setup and contribution steps."
```

Example output (the PR number and URL will differ):

```text
? Where do you use GitHub? GitHub.com
? What is your preferred protocol for Git operations? HTTPS
✓ Logged in as octocat

https://github.com/octocat/my-project/pull/1
```

The GitHub website route does not require the GitHub CLI.

## 9. Review and merge the pull request

Before merging:

- Read the PR diff and description.
- Resolve review feedback.
- Wait for required automated checks to pass.
- Confirm the base and compare branches are correct.

When approved and ready, select **Merge pull request** on GitHub and confirm.
The repository's rules may require approval or a specific merge method. After
merging, GitHub shows the PR as **Merged**. To update the local default branch:

```bash
git switch main
git pull
```

Example output after a successful merge:

```text
Switched to branch 'main'
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
Updating a1b2c3d..d4e5f6a
Fast-forward
 README.md | 10 ++++++++--
 1 file changed, 8 insertions(+), 2 deletions(-)
```

Expected result in the browser: the PR page displays a **Merged** status and
the merge commit or squash commit.

The feature branch can be deleted after merging if it is no longer needed.

## 10. Common problems

### `remote origin already exists`

Inspect it and update the URL if needed:

```bash
git remote -v
git remote set-url origin https://github.com/<username>/<repository>.git
```

Expected output: the first command prints the current remote URL; the second
normally completes silently. Run `git remote -v` again to confirm the update.

### `src refspec main does not match any`

There may not be a commit yet, or the branch may have a different name. Check:

```bash
git status
git branch --show-current
git log --oneline -1
```

Example output:

```text
On branch main
nothing to commit, working tree clean
main
a1b2c3d Add initial project files
```

Create a commit if necessary, then push the actual branch name.

### Authentication failed

Use the GitHub authentication flow for HTTPS or configure an SSH key. GitHub
account passwords are not accepted as Git HTTPS passwords. Never paste a token
into a shared command, screenshot, repository, or this document.

### Push rejected because the remote contains commits

First inspect the remote and local history. If the remote contains a README or
other commit, fetch and integrate it intentionally; do not overwrite remote
history with a force push:

```bash
git fetch origin
git log --oneline --graph --decorate --all
```

Example output:

```text
From https://github.com/octocat/my-project
 * [new branch]      main       -> origin/main
* a1b2c3d (HEAD -> main) Add initial project files
* 9f8e7d6 (origin/main) Add a README file
```

Resolve the histories according to the project state, then push again.

## 11. Requested deliverables

The repository URL, pull request link, and merged-PR screenshot depend on actually
creating and merging a PR in a GitHub account. This guide does not create or push
a repository, open/merge a PR, or capture a screenshot. Fill in these fields
after completing the steps above:

- **GitHub repository URL:** `<paste the repository page URL>`
- **Pull request link:** `<paste the merged pull request URL>`
- **Merged PR screenshot:** `<save and attach a screenshot of the GitHub PR page showing Merged>`

For a useful screenshot, open the completed PR on GitHub and capture the page
showing its title, PR number, repository, and **Merged** status. Avoid including
access tokens, private data, or unrelated browser tabs. Save the image alongside
your submission or attach it where the deliverables are being submitted.
