---
name: kiro-tool
description: 使用 kiro-cli 進行代碼編寫或靜態掃描審核。
---

# Kiro Multi-Role Skill

## 指令規範

- **實作者角色 (Coder)**:
  `kiro-cli generate "{{task}}" --file {{file}}`
- **審核者角色 (Reviewer)**:
  `kiro-cli review {{file}}`

## 注意事項

- 在 Terminal 使用 `kiro-cli` 呼叫。
- Kiro 擅長語法規範與安全性檢查，適合對 Gemini 的產出進行「把關」。
