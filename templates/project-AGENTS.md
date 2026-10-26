# 專案名稱 - AGENTS.md

## 專案入口
- 專案名稱：專案名稱
- 專案用途：專案主要功能與目標
- GitHub repo：https://github.com/<owner>/<repo>
- 預設 branch：main

## 工作規則
- 回應使用繁體中文（台灣）。
- 涉及檔案操作時回報完整產出位置與連結。
- 使用 PowerShell 語法。
- 開工時讀本檔、讀專案最新進度、檢查 Git 狀態。
- 收工時檢查 diff 後只提交本次相關檔案，不無差別 `git add .`。
- 不把每日流水帳寫進本檔。
- 遵守規則分流機制。

## 規則分流與把關機制 (Rule Triage)
- 當要求新增設定或流程時，主動協助評估歸屬：
  1. **全域 md (`~/.gemini/GEMINI.md`)**：通用習慣、全域安全底線。
  2. **專案 md (`AGENTS.md`)**：專案技術架構、目錄規範、專案邊界。
  3. **技能 (`.agents/skills/<name>/SKILL.md`)**：多步驟 SOP 與工具呼叫流程。

## 不要做
- 不要 commit API key、token、密碼與個人敏感憑證。
- 不要自動納入無關 git 變更。
