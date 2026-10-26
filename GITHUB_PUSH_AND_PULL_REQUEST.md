# Pushing Source Code to GitHub and Creating a Pull Request

This guide explains what GitHub is, why it is useful, and how to publish source
code and propose changes with a pull request. The commands use Git Bash, PowerShell,
or another terminal with Git installed. Replace values in `<angle brackets>` with
your own values; do not type the angle brackets.

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

Set your commit author information once on this computer. Use the name and email
you want recorded in your commits:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Check the settings:

```bash
git config --global --get user.name
git config --global --get user.email
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

Check the current directory and files:

```bash
git status
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

## 6. Connect the project to GitHub and push it

Replace the URL below with the URL copied from the newly created GitHub repository:

```bash
git remote add origin https://github.com/<username>/<repository>.git
git remote -v
git push -u origin main
```

`git remote add origin` names the GitHub repository as `origin`. The first push
uploads the `main` branch. `-u` records the upstream branch, so future pushes
from `main` can usually use the shorter command:

```bash
git push
```

If `origin` is already configured, inspect it instead of adding it again:

```bash
git remote -v
```

To correct an existing remote URL:

```bash
git remote set-url origin https://github.com/<username>/<repository>.git
```

Refresh the GitHub repository page to confirm that the files and commit appear.

## 7. Make a change on a feature branch

Create a separate branch for a change rather than committing directly to `main`:

```bash
git switch main
git pull
git switch -c docs/update-project-guide
```

Edit or add the relevant project files. Then review and commit the change:

```bash
git status
git diff
git add <file-or-folder>
git diff --cached
git commit -m "Document project setup"
```

Push the feature branch to GitHub:

```bash
git push -u origin docs/update-project-guide
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

### Optional: using GitHub CLI

Install and authenticate the GitHub CLI (`gh`) before using these commands:

```bash
gh auth login
gh pr create --base main --head docs/update-project-guide \
  --title "Document project setup" \
  --body "Explain setup and contribution steps."
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

The feature branch can be deleted after merging if it is no longer needed.

## 10. Common problems

### `remote origin already exists`

Inspect it and update the URL if needed:

```bash
git remote -v
git remote set-url origin https://github.com/<username>/<repository>.git
```

### `src refspec main does not match any`

There may not be a commit yet, or the branch may have a different name. Check:

```bash
git status
git branch --show-current
git log --oneline -1
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
