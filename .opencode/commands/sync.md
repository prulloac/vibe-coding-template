---
description: Sync working directory with remote repository - rebase, commit all changes, and push to same branch
agent: general
tools:
  - read
  - write
  - edit
  - glob
  - grep
  - bash
  - skill
  - task
subtask: true
---

## Sync Working Directory with Remote

This command synchronizes the local working directory with the remote repository using git.

### Step 1: Get Current Branch

Execute the following git command to determine the current branch:
!`git branch --show-current`

### Step 2: Rebase from Upstream

Fetch the latest changes from all remotes:
!`git fetch --all`

Rebase the current branch onto its upstream/remote branch:
!`git rebase`

If there are merge conflicts, stop and ask the user how to resolve them before proceeding.

### Step 3: Check for .gitignore Changes

Check if there are any changes to .gitignore:
!`git status --porcelain .gitignore`

**If .gitignore has changes:**
1. Stage the .gitignore changes: `!`git add .gitignore``
2. Use the **git-commit-workflow** skill to commit these changes with an appropriate message (e.g., "Update .gitignore")
3. After committing .gitignore changes, re-check the overall git status to identify all remaining files that need to be committed

### Step 4: Stage All Remaining Changes (Tracked and Untracked)

Stage all tracked files:
!`git add -u`

Stage all untracked files (including new files):
!`git add -A`

### Step 5: Check Staged Changes

View what will be committed:
!`git diff --cached --stat`

### Step 6: Commit All Changes

If there are staged changes, use the **git-commit-workflow** skill to commit all staged changes.

### Step 7: Push to Remote

Push the current branch to the remote repository with the same branch name:
!`git push origin <BRANCH_NAME>`

Replace `<BRANCH_NAME>` with the branch name obtained in Step 1.

If the push fails (e.g., due to remote changes), inform the user and ask how to proceed (force push, rebase again, etc.).
