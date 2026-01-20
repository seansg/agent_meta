---
name: git-worktree

description: Automatically manages git worktrees (list, create, switch, remove).
  This is an ACTION skill.
  When running as an agent, execute this skill automatically
  before starting work that requires a new branch or parallel development.
---

# Git Worktree Management Skill

## Skill Type

- Action skill
- No explanation-only mode
- Designed for automatic execution by an agent

---

## Automatic Execution Rules (Agent Only)

When running as an agent, automatically execute this skill BEFORE any other action if:

- The task involves:
  - starting a new feature
  - fixing a bug or hotfix
  - creating or switching to a branch
  - working on multiple tasks or branches in parallel
- AND the task is not a trivial change (e.g. read-only, docs-only view)

Do NOT ask for confirmation unless explicitly instructed by the user.

---

## Naming Convention (Mandatory)

Worktrees MUST be created as sibling directories.

Pattern:
`../<repo-name>-wt-<sanitized-branch-name>`

Sanitization rule:

- Replace `/` with `-`

Example:

- Branch `feature/user-auth` → `../agent_meta-wt-feature-user-auth`
- Branch `hotfix/login-bug` → `../agent_meta-wt-hotfix-login-bug`

---

## Environment File Handling

After creating a new worktree, automatically copy `.env` configuration files from the main worktree to maintain consistent development environments.

### Files to Copy

Search for and copy the following files from the main worktree root to the new worktree root:

- `.env`
- `.env.local`
- `.env.development`
- `.env.production`
- Any other `.env.*` files found

### Copy Process

1. After successfully creating a new worktree, identify the main worktree directory (usually the parent of the current directory or the original repository location)
2. Search for all `.env*` files in the main worktree root directory
3. Copy each found `.env*` file to the new worktree root directory
4. Report which files were copied (or if no `.env` files were found)

### Example Commands

```bash
# After creating worktree at ../agent_meta-wt-feature-auth
# Copy .env files from main worktree
cp /path/to/main/worktree/.env* /path/to/new/worktree/ 2>/dev/null || true
```

### Important Notes

- Only copy `.env*` files from the root directory of the main worktree
- Do not fail if no `.env` files are found (some projects may not use them)
- Preserve file permissions when copying
- Do not overwrite existing `.env` files in the new worktree if they already exist
