---
name: git-worktree

description:
  Automatically manages git worktrees (list, create, switch, remove).
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
