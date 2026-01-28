---
name: gemini-kiro-collaboration
description: 當需要修改、優化或重構程式碼時使用。此 Skill 會啟動一個雙 Agent 協作流程：由 Gemini 負責修改，並由 Kiro 負責審核與品質控管。
---

# Gemini-Kiro 協作開發 Skill

這是一個自動化的開發工作流，模擬了「開發者 + 資深審核者」的協作模式。當使用者要求對現有程式碼進行變更時，必須遵循此流程。

## 適用場景

- **程式碼重構**：優化結構但保持功能不變。
- **功能新增**：在現有檔案中添加新邏輯。
- **Bug 修復**：針對特定問題進行修正。
- **效能優化**：提高執行效率或節省資源。

## 執行流程與指令規範

Agent 應在 Local 環境中依序執行以下步驟：

### 1. 修改階段 (Gemini)

使用 `gemini-cli` 進行程式碼變更。

- **指令**: `gemini-cli "任務描述：{{task}} \n 原始程式碼：$(cat {{file_path}})" > {{file_path}}.tmp`
- **要求**: 必須確保輸出為純程式碼格式，並存入 `.tmp` 暫存檔。

### 2. 審核階段 (Kiro)

使用 `kiro-cli` 對產出的 `.tmp` 檔案進行嚴格評核。

- **指令**: `kiro-cli review {{file_path}}.tmp`
- **關鍵點**: 讀取 Kiro 的輸出報告。

### 3. 自動修正循環 (Self-Correction)

- **若 Kiro 報告包含錯誤或建議**：將該報告反饋給 Gemini，重複步驟 1，直到通過審核或達到 3 次嘗試。
- **指令範例**: `gemini-cli "請根據以下 Review 意見修正 {{file_path}}.tmp：{{kiro_feedback}}"`

### 4. 完成階段

- **指令**: 當審核通過後，執行 `mv {{file_path}}.tmp {{file_path}}`。

## 注意事項

- **不要直接覆蓋**：在 Kiro 審核通過前，禁止直接修改原始檔案。
- **Context 完整性**：在呼叫 CLI 前，務必確認已讀取目標檔案的最新內容。
- **報告反饋**：Agent 應將 Kiro 的最終 Review 報告摘要呈現給使用者。

## 範例範本

當使用者說：「優化這個 Function 的效能」

1. 調用 `gemini-cli` 產生優化版本。
2. 調用 `kiro-cli` 檢查安全性與效能。
3. 若 Kiro 滿意，則替換檔案並回報成功。
