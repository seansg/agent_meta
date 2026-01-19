---
name: git-pr
description: Automates Pull Request workflow with optional ticket number prefix. Works seamlessly with git-worktree and git-commit-formatter. Can be triggered via natural language commands in Antigravity IDE.
---

# Git Pull Request Skill (Antigravity Agent)

This skill handles PR creation or updating automatically after all code changes are committed. It integrates with existing git-worktree and git-commit-formatter workflows, and optionally allows the user to prepend a ticket number or prefix to the PR title.

## Skill Triggers (Natural Language Examples)

- "Create a PR for branch feature/login"
- "Open a pull request for branch hotfix/crash-fix"
- "Update the PR for branch feature/signup"
- "Merge the PR for branch feature/payment"

> The Agent will ask: "PR title 是否需要加上 prefix?"  
> User can reply with the ticket number (e.g., `TICKET-123`) or `"No"`.

---

## Workflow

1. **git-worktree**
   - Agent ensures the correct worktree is checked out or automatically created if it does not exist.

2. **git-commit-formatter**
   - Agent ensures all changes are committed with Conventional Commit messages.

3. **Push**
   - Agent pushes changes to the remote branch (setting upstream if needed).

4. **PR Handling**
   - Agent prompts the user for an optional ticket number prefix.
   - Generates PR title:
     - With ticket: `<TICKET> <commit-title>`
     - Without ticket: `<commit-title>`
   - PR body: recent 5 commit messages.
   - Checks if PR already exists:
     - If yes: updates PR title/body.
     - If no: creates new PR.

---

## Skill Logic (Bash)

```bash
#!/bin/bash
# git-pr-agent.sh

BRANCH_NAME=$1           # branch name
BASE_BRANCH=${2:-main}   # default base branch
TICKET_PREFIX=$3         # optional ticket number

# Ensure worktree exists
REPO_NAME=$(basename "$(git rev-parse --show-toplevel)")
SANITIZED_BRANCH=$(echo "$BRANCH_NAME" | sed 's/\//-/g')
WORKTREE_PATH="../${REPO_NAME}-wt-${SANITIZED_BRANCH}"
git worktree add -b "$BRANCH_NAME" "$WORKTREE_PATH" "$BASE_BRANCH" 2>/dev/null || echo "Worktree exists"
cd "$WORKTREE_PATH"

# Commit changes if any
if [ -n "$(git status --porcelain)" ]; then
    git add .
    COMMIT_MSG=$(cat ../COMMIT_MSG.txt 2>/dev/null || echo "chore: auto commit changes")
    git commit -m "$COMMIT_MSG"
fi

# Push changes
git push origin "$BRANCH_NAME" || git push --set-upstream origin "$BRANCH_NAME"

# Prepare PR title
AUTO_TITLE=$(git log -1 --pretty=format:"%s")
if [ -n "$TICKET_PREFIX" ]; then
    PR_TITLE="$TICKET_PREFIX $AUTO_TITLE"
else
    PR_TITLE="$AUTO_TITLE"
fi
PR_BODY=$(git log -5 --pretty=format:"- %s")

# Create or update PR using GitHub CLI
EXISTING_PR=$(gh pr list --head "$BRANCH_NAME" --json number --jq '.[0].number')
if [ -z "$EXISTING_PR" ]; then
    gh pr create --title "$PR_TITLE" --body "$PR_BODY" --base "$BASE_BRANCH"
else
    gh pr edit "$EXISTING_PR" --title "$PR_TITLE" --body "$PR_BODY"
fi

echo "✅ PR workflow completed: $PR_TITLE"
```
