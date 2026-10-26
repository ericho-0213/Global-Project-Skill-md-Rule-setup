---
name: workflow
description: AntiGravity 開工與收工標準作業流程。說「開工」「收工」時載入。
---

# 開工 / 收工標準流程

## 開工
1. 讀取 `AGENTS.md`（若有 `ANTIGRAVITY.md` 一併讀取）。
2. 檢查 `git status` 與最近 commit 紀錄。
3. 回報專案目前進度與建議的下一步。
4. 不自動執行未經確認的 pull/commit/push。

## 收工
1. 檢查有無敏感憑證（API key、token、密碼等）。
2. 更新專案筆記或交接進度。
3. 只在規則改變時更新 `AGENTS.md`。
4. 檢查 `git status` 與 `git diff`。
5. 只 stage 本次相關檔案（不用 `git add .`）。
6. 確認後 commit 與 push。
7. 回報同步結果。
