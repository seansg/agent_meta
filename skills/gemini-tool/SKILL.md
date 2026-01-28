---
name: gemini-tool
description: 使用 Gemini 進行程式碼編寫或審核。
---

# Gemini Multi-Role Skill

## 指令規範

- **當作為 Coder 時**:
  `gemini "實作以下需求：{{task}} \n 原始碼：$(cat {{file}})" --raw`
- **當作為 Reviewer 時**:
  `gemini "請作為 Senior Reviewer 審核此程式碼並給出改進建議：$(cat {{file}})"`

## 使用指南

如果使用者要求「對等檢查」，請先用 Kiro 產生內容，再調用此 Skill 進行 Review。
