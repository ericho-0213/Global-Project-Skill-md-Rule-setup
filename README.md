# AntiGravity 規則架構指南：全域 md / 專案 md / Skill.md

本專案彙整 Google DeepMind **AntiGravity (AGY)** 環境下的三層規則與技能架構設計指南，提供規範分流把關標準（Rule Triage SOP）與標準設定範本。

---

## 📖 目錄

1. [三層架構核心定位](#一三層架構核心定位)
2. [為什麼需要三層分流？](#二為什麼需要三層分流)
3. [規則分流把關機制 (Rule Triage SOP)](#三規則分流把關機制-rule-triage-sop)
4. [標準配置範本](#四標準配置範本)
   - [1. 全域 md (`~/.gemini/GEMINI.md`)](#1-全域-md-geminigeminimd)
   - [2. 專案 md (`AGENTS.md`)](#2-專案-md-agentsmd)
   - [3. 技能檔 (`SKILL.md`)](#3-技能檔-skillmd)
5. [常見問答 (FAQ)](#五常見問答-faq)

---

## 一、三層架構核心定位

在 AntiGravity 中，Agent 的行為受不同層級的設定與指令引導，各層級的職責劃分如下：

| 層級 | 核心檔案與位置 | 作用範圍 | 適用內容 | 載入機制 |
| :--- | :--- | :--- | :--- | :--- |
| **全域規則**<br>(Global md) | `~/.gemini/GEMINI.md`<br>`~/.gemini/config/GEMINI.md` | 本機所有專案 | 個人通用語系偏好、終端機工具偏好（如 PowerShell）、全域最高安全禁忌、規則把關機制。 | 每次啟動對話時<br>**無條件全域載入** |
| **專案規則**<br>(Project md) | 專案根目錄之 `AGENTS.md`<br>（或 `GEMINI.md`） | 當前專案與其子目錄 | 該專案之技術棧、Repo 資訊、專案工作規範、特定檔案忽略清單。 | 進入工作目錄時<br>**向上搜尋並載入** |
| **專案技能**<br>(Skill.md) | `.agents/skills/<name>/SKILL.md` | 當前專案特定任務 | 多步驟 SOP、特定任務腳本、MCP 工具使用指引（如生圖、Firebase、部署等）。 | **按需漸進載入**<br>(Progressive Disclosure) |

---

## 二、為什麼需要三層分流？

1. **避免 Context Window 浪費**：
   - 全域 md 與專案 md 會在每次對話啟動時載入。
   - 如果將厚重的操作手冊、部署腳本塞入 md，會無謂消耗 Context Token 並增加費用。
2. **避免專案間的污染與誤判**：
   - 若在全域 md 寫入特定專案的技術（例如 Next.js 規則），當您切換到 Python 專案時，Agent 可能會給出錯誤的架構建議。
3. **保持專案獨立性與可攜性**：
   - 專案特有的規則放在專案根目錄的 `AGENTS.md`，技能放在 `.agents/skills/`，可隨 Git 一起版控與團隊共享。

---

## 三、規則分流把關機制 (Rule Triage SOP)

當使用者提出：「*請幫我將這項設定 / 流程加入規則*」時，AI Agent 應主動依循以下決策樹進行評估與分流：

```
                    【新設定 / 流程需求】
                             │
         ┌───────────────────┼───────────────────┐
         ▼                   ▼                   ▼
  【加入 全域 md】       【加入 專案 md】         【封裝為 Skill】
   跨專案通用偏好         特定專案背景/架構       具體的多步驟 SOP
 (如繁中語系/安全底線)     (如 Repo/技術堆疊)      (需要時觸發的流程)
```

### 判斷標準：

1. **適合放「全域 md」**：
   - **個人回覆偏好**：例如「所有回答一律使用繁體中文（台灣）」、「代碼範例優先提供 PowerShell」。
   - **最高安全底線**：例如「禁止在任何輸出或 commit 中洩漏 API Key 與敏感憑證」。
   - **協作思維**：例如「當使用者要求新增設定時，主動協助進行分流把關」。

2. **適合放「專案 md (`AGENTS.md`)」**：
   - **技術堆疊**：例如本專案使用的 Python、Node.js 版本或框架。
   - **專案結構與路徑**：GitHub Repo 網址、專案資料夾結構、命名慣例。
   - **專案交付標準**：Commit message 規範、測試通過條件。

3. **適合封裝為「Skill (`SKILL.md`)」**：
   - **具體的多步驟作業流程**：如「開工/收工流程」、「Firebase 整合設定」、「生圖流程」。
   - **特定關鍵字觸發**：使用者說出「開工」或「部署」時才需叫出的手冊。

---

## 四、標準配置範本

### 1. 全域 md (`~/.gemini/GEMINI.md`)

放置於本機使用者目錄：
* Windows: `C:\Users\<使用者名稱>\.gemini\GEMINI.md` 與 `C:\Users\<使用者名稱>\.gemini\config\GEMINI.md`
* Mac / Linux: `~/.gemini/GEMINI.md`

```markdown
# 全域個人協作規範 (Global Rules)

## 通用行為準則
- 優先使用繁體中文（台灣）回應。
- Windows 環境下的終端機指令以 PowerShell 語法為主。
- 涉及檔案新增、修改或刪除時，回報完整路徑與說明。

## 規則分流與把關機制 (Rule Triage)
當使用者要求將某項設定、操作流程或約束加入規則時，主動協助評估並建議最合適的歸屬，避免造成全域規則臃腫或專案污染：
1. **全域 md (`~/.gemini/GEMINI.md`)**：僅收錄跨所有專案通用的個人偏好、溝通風格與全域安全底線。
2. **專案 md (`AGENTS.md` / `GEMINI.md`)**：收錄該專案專屬的技術架構、目錄慣例、特定 Repo 連結與專案 Git 規範。
3. **Skill (`.agents/skills/<name>/SKILL.md`)**：凡涉及多步驟的操作流程、SOP、工具調用腳本（如部署、測試、生圖、資料處理），應封裝為技能按需載入，不直接塞入規則 md。

## 安全底線
- 嚴格禁止 commit 或輸出任何 API Key、密碼、Token 與個人敏感憑證。
```

---

### 2. 專案 md (`AGENTS.md`)

放置於專案根目錄：`<專案根目錄>/AGENTS.md`

```markdown
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
- 遵守規則分流機制。

## 安全底線（不要做）
- 不要 commit API key、token、密碼與機密憑證。
- 不要自動納入無關 git 變更。
```

---

### 3. 技能檔 (`SKILL.md`)

放置於專案子目錄：`<專案根目錄>/.agents/skills/<技能名稱>/SKILL.md`

```markdown
---
name: my-workflow
description: 專案開工與收工標準流程。說「開工」「收工」時載入。
---

# 開工 / 收工標準流程

## 開工流程
1. 讀取 `AGENTS.md`。
2. 執行 `git status` 與檢查最近 commit。
3. 回報專案目前進度與建議的下一步。

## 收工流程
1. 檢查有無敏感憑證（API key、密碼）。
2. 檢查 `git status` 與 `git diff`。
3. 僅 stage 本次異動之相關檔案。
4. 撰寫清晰的 commit 訊息並 push 至遠端。
```

---

## 五、常見問答 (FAQ)

#### Q1：我剛開始使用 Antigravity，一定要同時建全域 md 與專案 md 嗎？
> **不用**。系統並無強制規定。推薦先在每個專案根目錄建立一份精簡的 `AGENTS.md`（專案 md）。當您有多個專案且希望統一語言偏好或安全規則時，再建立全域 md 即可。

#### Q2：如果全域 md 跟專案 md 的規則有衝突，誰優先？
> **專案 md 優先**。Antigravity 的載入優先權為：工作區專案規則 > 全域規則。專案設定會覆蓋全域設定。

#### Q3：技能 (Skill) 跟規則 (Rule) 的最大差別是什麼？
> **載入時機與成本**：
> * **Rule（規則）** 是「永遠在線 (Always-on)」，每次對話都會載入。
> * **Skill（技能）** 是「漸進揭露 (Progressive Disclosure)」，只有在觸發特定關鍵字或任務時才會完整載入內容，節省 Context Token。
