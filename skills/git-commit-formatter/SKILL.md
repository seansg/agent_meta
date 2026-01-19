---
name: git-commit-formatter
description: Formats git commit messages according to Conventional Commits specification. Use this when the user asks to commit changes or write a commit message.
---

# Git Commit Formatter Skill

When writing a git commit message, you MUST follow the Conventional Commits specification.

## Format
`<type>[optional scope]: <description>`

## Allowed Types
- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation only changes
- **style**: Changes that do not affect the meaning of the code (white-space, formatting, etc)
- **refactor**: A code change that neither fixes a bug nor adds a feature
- **perf**: A code change that improves performance
- **test**: Adding missing tests or correcting existing tests
- **chore**: Changes to the build process or auxiliary tools and libraries such as documentation generation

## Instructions

0. **Pre-commit Check**: Before analyzing changes or generating the message, check for lint or format scripts (e.g., `npm run lint`, `npm run format`) in the project. Run them if they exist.

1. The commit message MUST be written in English only.
   Even if the user provides information in Chinese, translate it and output English.
   Do NOT include any non-English text.

2. Analyze the changes to determine the primary `type`.

3. Identify the `scope` if applicable (e.g., specific component or file).

4. Write a concise `description` in imperative mood (imperative form).

5. If there are breaking changes, add a footer starting with `BREAKING CHANGE:`.


## Example
`feat(auth): implement login with google`


