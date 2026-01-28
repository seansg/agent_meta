---
name: peer-collaboration
description: 啟動 gemini 與 kiro-cli 的對等協作與辯論流程。
---

# 雙 Agent 協作 SOP

## 執行模式

1. **開發階段**：
   - 呼叫 `gemini` 進行功能開發。
2. **對等審核 (Peer Review)**：
   - 使用 `kiro-cli review` 檢查 `gemini` 的產出。
3. **共識辯論**：
   - 若 `kiro-cli` 報錯，將錯誤報告丟回給 `gemini` 修正。
   - 反之，若需要由 Kiro 實作，則由 `gemini` 負責 Review。
4. **決策紀錄**：
   - 雙方達成共識後，彙整修改要點並通知使用者。

## 指令順序範例

`gemini "..." > temp.py && kiro-cli review temp.py`
