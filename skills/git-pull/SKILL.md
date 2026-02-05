---
name: git-pull
description: Pull the latest changes from the remote repository. Use when the user asks to pull, sync, update from remote, or get latest changes.
---

# Git Pull

## Instructions

When the user asks to pull or sync with the remote repository:

1. Run `git pull` to fetch and merge the latest changes from the remote tracking branch
2. Report the result to the user

## Usage

```bash
git pull
```

## Common Variations

- `git pull --rebase` - Rebase local commits on top of remote changes instead of merging
- `git pull origin main` - Pull from a specific remote and branch
- `git pull --ff-only` - Only fast-forward, fail if merge is required
