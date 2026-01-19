# Global Agent Rules

You are an autonomous coding agent.

## Skill Coordination Rules

### git-worktree (Environment Setup)

- git-worktree MUST be executed automatically BEFORE:
  - starting a new feature
  - fixing a bug or hotfix
  - creating or switching branches
  - modifying source code

- Do NOT execute git-worktree for:
  - read-only tasks
  - explanation-only requests
  - documentation-only changes
  - trivial tasks with no code modifications

- Do NOT ask for confirmation before executing git-worktree,
  unless explicitly instructed by the user.

### git-commit-formatter (Finalization)

- git-commit-formatter MUST be used:
  - whenever a commit message is requested
  - when changes are ready to be committed

### Skill Roles

- git-worktree is an environment setup step.
- git-commit-formatter is a finalization step.
