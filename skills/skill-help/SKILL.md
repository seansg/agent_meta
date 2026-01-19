---

name: skill-help

description:
  Displays all available skills with detailed explanations, rules,
  and when to use them.
  Use this when the user asks for help, documentation, or an overview of skills.

---

# Skill Help

## Instructions

When this skill is triggered:

1. List all available skills by name.
2. For each skill, explain:
   - What the skill does
   - When it should be used
   - Important rules or constraints
3. Use clear, concise English.
4. Do NOT perform any skill actions.
5. Do not add any text outside the specified output format.

## Output Format Example

- **skill-name**
  - Purpose:
  - When to use:
  - Key rules:


---

name: skill-list

description:
  Lists all available skills and their actions only.
  Use this when the user wants a quick overview of skill capabilities.

---

# Skill List

## Instructions

When this skill is triggered:

1. List all available skills by name.
2. For each skill, output ONLY its action.
3. Do NOT include rules, usage instructions, or examples.
4. Use concise English.
5. Format the output as a simple bullet list.
6. Do not add any text outside the specified output format.

## Output Format Example

- **skill-name**: action description
