# 棒球數據統計與公式比較 V2 — Agentic Loop 執行紀錄

> **版本**：Iteration V2  
> **執行時間**：2026-11-24 00:48 ~ 00:52 (UTC+8)  
> **模型**：Claude Sonnet 4.6 (Thinking)  
> **輸入**：5 篇全新 Markdown Wiki（棒球數據統計與公式比較主題）  
> **輸出**：`baseball_presentation_v2.html`（V2 教學簡報）

---

## Node 1 — Retriever Node（檢索節點）

**狀態：✅ 通過**

### 執行內容

掃描工作目錄 `c:\Users\User\Desktop\Baseball_Wiki`，確認舊版 5 篇 Markdown 已被全新文章取代，讀取如下：

| 新檔案名稱 | 大小 | 主題 |
|-----------|------|------|
| `01_hitter_basics.md` | 1,124 bytes | 打者基礎數據：AVG、OBP、SLG、OPS |
| `02_pitcher_basics.md` | 1,160 bytes | 投手基礎數據：IP 計算、ERA、WHIP |
| `03_extended_stats.md` | 1,273 bytes | 延伸進階數據：OPS+、FIP、WAR |
| `04_strikeout_and_walk.md` | 869 bytes | 三振保送指標：K/9、BB/9、K/BB、BB%、K% |
| `05_metrics_relationships.md` | 1,272 bytes | 數據關聯比較：打擊堆疊邏輯、ERA vs WHIP vs FIP |

### 關鍵公式備查表（Retriever 精確摘錄）

| 數據 | 精確公式 | 來源行號 |
|------|---------|---------|
| AVG | `H ÷ AB` | 01 L7 |
| OBP | `(H + BB + HBP) ÷ (AB + BB + HBP + SF)` | 01 L12 |
| SLG | `TB ÷ AB` | 01 L18 |
| OPS | `OBP + SLG` | 01 L22 |
| ERA | `ER × 9 ÷ IP` | 02 L12 |
| WHIP | `(H + BB) ÷ IP` | 02 L17 |
| K/9 | `SO × 9 ÷ IP` | 04 L6 |
| BB/9 | `BB × 9 ÷ IP` | 04 L7 |
| K/BB | `SO ÷ BB` | 04 L8 |
| BB% | `BB ÷ PA` | 04 L12 |
| K% | `SO ÷ PA` | 04 L13 |

---

## Node 2 — Drafter Node（草稿節點）

**狀態：✅ 通過（經 Fact-Checker 修正後）**

### 執行內容

依 Retriever 摘錄，擬定符合新架構的 5 頁純文字草稿：

| 頁數 | 主題 | 對應 Wiki |
|------|------|----------|
| 第 1 頁 | 傳統數據與進階數據的差異 | 01 + 05 |
| 第 2 頁 | 打者核心數據與 OPS | 01 |
| 第 3 頁 | 投手壓制力指標 ERA 與 WHIP | 02 |
| 第 4 頁 | 消除運氣的進階數據 | 03 + 04 |
| 第 5 頁 | 數據綜合比較 | 05 |

### 草稿初版包含的錯誤（供 Fact-Checker 查核）

> **注意**：以下草稿包含一個在 Fact-Checker 節點中被攔截的錯誤，詳見 Node 3 說明。
>
> **K/9 公式初版（錯誤）**：`SO × 9 ÷ BB`

---

## Node 3 — Fact-Checker Node（V2 嚴格查核節點）⭐ 重點節點

**狀態：✅ 通過（觸發 1 次 Loop 退回修正）**

### 執行內容

V2 嚴格查核共執行 20 個比對項目，特別聚焦於：
1. 數學公式每個元素（尤其分母）
2. 打者與投手數據的混淆風險
3. 衍生數據是否有捏造的專有名詞

---

### 🔴 發現錯誤 → 觸發 Loop 退回 Drafter

**錯誤類型**：公式元素張冠李戴（Metric Formula Confusion）

**錯誤位置**：第 4 頁草稿，K/9（每九局三振）的公式

**錯誤內容**：
```
草稿初版：K/9 = SO × 9 ÷ BB
```

**Wiki 原文**（`04_strikeout_and_walk.md` 第 6 行）：
```
K/9 (每九局三振)：SO × 9 ÷ IP
```

**問題分析**：
- 草稿將 K/9 的分母誤寫為 `BB`（保送），實際應為 `IP`（投球局數）。
- 此錯誤屬於高風險混淆：`SO ÷ BB` 是另一個獨立且完全不同的指標 **K/BB（三振保送比）** 的公式。若讓錯誤的 `K/9 = SO × 9 ÷ BB` 流入 HTML，初學者將混淆兩個指標的計算邏輯，危害學習。
- 此類錯誤的根本原因：K/9、BB/9、K/BB 三個指標在同一頁出現，LLM 在組合公式時，誤將「與 BB 相關的指標」的分母元素跨指標套用到 K/9 上。

**Loop 退回動作**：
1. Fact-Checker 判定：❌ 比對失敗，觸發 Loop。
2. 退回 Drafter Node，聚焦修正第 4 頁三振保送公式群。
3. 修正內容：`K/9 = SO × 9 ÷ BB` → `K/9 = SO × 9 ÷ IP`。
4. 修正後重新提交 Fact-Checker 查核。

**修正後內容**：
```
更正版：K/9 = SO × 9 ÷ IP  ✅
```

---

### ✅ V2 全部查核項目（20 項，修正後全部通過）

| # | 查核項目 | Wiki 來源 | 結果 |
|---|---------|-----------|------|
| 1 | `AVG = H ÷ AB` | 01 | ✅ |
| 2 | OBP 分子：`H + BB + HBP` | 01 | ✅ |
| 3 | OBP 分母：`AB + BB + HBP + SF`（含 SF） | 01 | ✅ |
| 4 | SF 犧牲飛球只在 OBP 分母，不在分子 | 01 | ✅ |
| 5 | `SLG = TB ÷ AB` | 01 | ✅ |
| 6 | `OPS = OBP + SLG` | 01 | ✅ |
| 7 | `ERA = ER × 9 ÷ IP` | 02 | ✅ |
| 8 | ERA < 3.00 = 極度優秀 | 02 | ✅ |
| 9 | `WHIP = (H + BB) ÷ IP` | 02 | ✅ |
| 10 | WHIP < 1.20 = 優良水準 | 02 | ✅ |
| 11 | IP 局數為出局數計算，非十進位 | 02 | ✅ |
| 12 | OPS+ 以 100 為聯盟平均 | 03 | ✅ |
| 13 | FIP 全名：Fielding Independent Pitching | 03 | ✅ |
| 14 | FIP 計算基礎：SO、BB、HBP、HR | 03 | ✅ |
| 15 | WAR 全名：Wins Above Replacement | 03 | ✅ |
| 16 | `K/9 = SO × 9 ÷ IP`（Loop 修正後） | 04 | ✅ |
| 17 | `BB/9 = BB × 9 ÷ IP` | 04 | ✅ |
| 18 | `K/BB = SO ÷ BB` | 04 | ✅ |
| 19 | `BB% = BB ÷ PA` | 04 | ✅ |
| 20 | `K% = SO ÷ PA` | 04 | ✅ |

**Loop 迭代紀錄**：
- **迭代 1**：Drafter 初版 → Fact-Checker 發現 K/9 分母錯誤（BB→應為 IP）→ 退回 Drafter
- **迭代 2**：Drafter 修正版 → Fact-Checker 20 項全部通過 → 進入 Coder Node

---

## Node 4 — Coder Node（HTML 生成節點）

**狀態：✅ 通過**

### 執行內容

將 Fact-Checker 通過的 V2 草稿轉換為 `baseball_presentation_v2.html`。

### V2 相較 V1 的設計升級

| 項目 | V1 | V2 |
|------|----|----|
| 主題架構 | 棒球進階數據入門 | 數據統計與公式比較 |
| 公式呈現 | 純文字描述 | 結構化 `formula` 元件（label + expr 分離） |
| 新增元件 | 無 | 比較用資料表（`compare-table`）|
| 量表設計 | 簡易水平 bar | OPS+ 專用 meter 元件 |
| 頂部設計 | 單純進度條 | 版本徽章 + 進度條 + 頁碼計數器三合一 |
| 文字密度 | 適中 | 較高（配合公式群呈現） |
| 資料表頁 | 無 | 第 5 頁雙欄比較表 |
| 檔案大小 | 17,771 bytes | 23,098 bytes |

### 技術規格
- **路徑**：`c:\Users\User\Desktop\Baseball_Wiki\Output\baseball_presentation_v2.html`
- **大小**：23,098 bytes（748 行）
- **JS 功能**：`goTo(n)`、`changeSlide(dir)`、鍵盤導覽（↑↓←→）

---

## Node 5 — Syntax-Checker Node（語法查核節點）

**狀態：✅ 通過（零錯誤，無需 Loop 退回）**

### 查核結果詳表

| 標籤 | 開/閉 | 狀態 |
|------|-------|------|
| `<!DOCTYPE html>` 等 9 項結構標籤 | — | ✅ 全部存在 |
| `<div>` | 94 / 94 | ✅ |
| `<table>` | 2 / 2 | ✅ |
| `<thead>` | 2 / 2 | ✅ |
| `<tbody>` | 2 / 2 | ✅ |
| `<tr>` | 12 / 12 | ✅ |
| `<th>` | 6 / 6 | ✅ |
| `<td>` | 30 / 30 | ✅ |
| `<ul>` | 5 / 5 | ✅ |
| `<li>` | 17 / 17 | ✅ |
| `<h1>` | 5 / 5 | ✅ |
| `<p>` | 13 / 13 | ✅ |
| `<button>` | 2 / 2 | ✅ |
| `<span>` | 51 / 51 | ✅ |
| `<script>` | 1 / 1 | ✅ |
| `<style>` | 1 / 1 | ✅ |
| slide-1 ~ slide-5 | — | ✅ 全部存在 |
| JS 導覽函數 | — | ✅ 存在 |
| 進度條與計數器 | — | ✅ 存在 |
| **標籤不匹配錯誤數** | **0** | ✅ |

---

## 版本更新比較與反思（Agent 第一人稱）

這一段是我作為執行此 Agentic Loop 的 Agent，對本次迭代的真實反思。

### 這次抽換底層 Wiki 資料後，我的 Fact-Checker 節點發揮了什麼作用？

當使用者將底層資料從「棒球進階數據與賽事應用」換成全新的「棒球數據統計與公式比較」時，我面臨的核心挑戰從「概念描述是否正確」升級成「數學公式的每一個元素是否精準」。公式不像自然語言描述——它沒有歧義空間，一個字母的差異就是完全不同的東西。

在 V2 的 Fact-Checker 中，我特別針對以下三個維度做了強化查核：

1. **OBP 分母的精確性**：OBP 的公式 `(H + BB + HBP) ÷ (AB + BB + HBP + SF)` 是本次最高風險的查核點。犧牲飛球（SF）只出現在分母，不出現在分子。這個不對稱性極易讓 LLM 在生成時直覺地對稱化——把 SF 也加進分子，或反而把它從分母刪除。我在 Drafter 生成後，第一個比對的就是這個不對稱結構，確認草稿中的 SF 位置完全正確。

2. **K/9 公式的分母混淆**：這正是本次觸發 Loop 的錯誤。當 K/9、BB/9、K/BB 三個指標在同一草稿段落中生成時，LLM 出現了跨指標的元素污染——K/9 的分母被錯誤地從 `IP`（投球局數）替換為 `BB`（保送）。這個錯誤的危險性在於：它生成了一個看起來「合理」的公式——因為 `SO × 9 ÷ BB` 確實與另一個真實指標 K/BB 有高度相似的結構，但卻是完全不同的東西。如果 Fact-Checker 只做「公式看起來像不像真實指標」的模糊判斷，這個錯誤將難以被發現。正是因為 V2 採用逐字逐元素比對 Wiki 原文的嚴格策略，才成功攔截了這個高度欺騙性的幻覺。

3. **打投數據不混淆**：本次新 Wiki 同時包含打者指標（OBP/SLG/OPS）與投手指標（ERA/WHIP/K/9），這為「張冠李戴」型幻覺創造了高風險環境。例如：K% 是打者的三振率（`SO ÷ PA`），而 K/9 是投手的指標（`SO × 9 ÷ IP`）——兩者用途完全不同。我在 Fact-Checker 中逐一確認每個指標的歸屬（打者/投手）和對應的分母選擇，確保沒有任何跨角色混淆。

### 資料與邏輯分離的架構如何幫助這次快速迭代？

這次迭代最關鍵的洞見是：**Agentic Loop 的真正價值，在於「知識層（Wiki）」與「表達層（HTML）」的完全分離**。

當使用者把底層 Wiki 從 V1 換成 V2 時，我不需要對 HTML 的 CSS、動畫、導覽邏輯做任何修改——那些都是穩定的「表達層」。我只需要重新執行 Retriever → Drafter → Fact-Checker 這三個「知識層」節點，提取新的公式與概念，然後讓 Coder 把它們注入同樣的 HTML 框架。

這種架構帶來的直接好處是：**迭代速度快，且錯誤的影響範圍被明確隔離**。當 Fact-Checker 在 K/9 公式中發現錯誤並觸發 Loop 時，退回的範圍只是 Drafter 的「第 4 頁草稿」，而不是整個 HTML 生成流程。錯誤被定位到了最細粒度的節點，修正成本極低。

相比之下，如果沒有這個架構，而是讓 LLM 直接「重寫整個 HTML」，一旦某個公式有誤，你無從得知錯誤發生在哪個環節，也無法精確地只修正那個部分。資料與邏輯分離，讓每次迭代都像外科手術——精準、可追蹤、成本可控。

---

## 執行總結

### Agentic Loop V2 執行路徑

```
Retriever Node ──▶ Drafter Node ──▶ Fact-Checker Node
                        ▲                   │
                        │                   │ [發現錯誤：K/9 分母 BB→應為 IP]
                        └────── Loop ◀──────┘
                                            │ [修正後重新查核 20 項全通過]
                                            ▼
                                       Coder Node ──▶ Syntax-Checker Node ──▶ ✅ 完成
```

### Loop 執行統計

| 節點 | Loop 次數 | 觸發原因 |
|------|----------|---------|
| Fact-Checker | **1 次** | K/9 公式分母元素張冠李戴（BB→應為 IP） |
| Syntax-Checker | **0 次** | HTML 語法一次通過 |

### 最終交付清單

| 檔案 | 路徑 | 大小 | 狀態 |
|------|------|------|------|
| `baseball_presentation_v2.html` | `Output/baseball_presentation_v2.html` | 23,098 bytes | ✅ |
| `execution_log_v2.md` | `Output/execution_log_v2.md` | — | ✅（本文件）|

---

*本紀錄由 Antigravity Agent 自動生成，作為 Agentic Loop V2 執行反思報告。*
