# LLM Wiki 學員範本（Template）

用 n8n + LLM 自動建立你自己的知識庫（Wiki）。
這是課程的**學員起始範本**：按幾個鈕就能得到一個乾淨的 wiki repo，
再把 `n8n/` 裡的 workflow 匯入你的 n8n，就能開始上傳文件、自動長出 wiki。

> 講師的 demo repo（有實際範例內容）在課堂上會展示；
> **請不要複製 demo repo 的內容**——裡面的 index 指向講師的 repo，複製過來會指錯位置。
> 一律從這個 template 開始。

## 這套系統在做什麼

1. 你在 n8n 的表單上傳一份檔案（PDF 或 Markdown）。
2. n8n 呼叫 LLM 分析、摘要、萃取實體與知識三元組。
3. n8n 把結果寫成一篇 Markdown，commit 到你 repo 的 `wiki/` 資料夾。
4. n8n 同時更新 `index.md`，把新文章的摘要與更新時間加進目錄。
5. 另有一個 chat 介面，可以用問答的方式查詢整個 wiki。

`index.md` 不只是目錄，而是**整個 wiki 的摘要層**：LLM 只要讀這一個檔案，
就知道有哪些文章、各篇重點是什麼、資料是新是舊，需要細節才去開 `wiki/` 的完整內容。

```
上傳檔案 ──▶ n8n workflow ──▶ LLM 分析 ──▶ wiki/xxx.md
                                    └────▶ index.md（更新目錄）
提問 ──▶ chat trigger ──▶ LLM 讀 index ──▶ 需要時開 wiki/ 細讀 ──▶ 回答
```

## 使用步驟（總覽）

完整圖文教學見 [docs/01-課前準備.md](docs/01-課前準備.md)，這裡是速覽：

1. **用這個 template 建立你自己的 repo**
   在本頁右上角點綠色的 **Use this template** → **Create a new repository**，
   Repository name 填 **`llm-wiki`**（請照這個名字，之後 workflow 設定會對上），
   選 **Private**，按 **Create repository**。
   完成後你就有一個乾淨的 repo，內含 `index.md` 和 `wiki/`，不需要手動建任何檔案。

2. **產生 GitHub Personal Access Token**
   n8n 要能讀寫你的 repo 需要這把鑰匙。做法見 docs/01 的步驟 3。

3. **下載並修改 n8n workflow**
   下載 [n8n/llm-wiki-workflow.json](n8n/llm-wiki-workflow.json)，
   用任何文字編輯器打開，**全部取代一次**：

   | 找 | 取代成 |
   | --- | --- |
   | `YOUR_GITHUB_USERNAME` | 你的 GitHub 帳號名稱 |

   一次「全部取代」會把檔案裡所有位置一起改好。
   （如果第 1 步你的 repo 沒取名 `llm-wiki`，把 `llm-wiki` 也全部取代成你的 repo 名。）

4. **匯入 n8n 並掛上憑證**
   在 n8n 建新 workflow → 右上 ⋯ → **Import from File** → 選改好的 json。
   點開每個有紅色驚嘆號的節點，在 Credential 欄選（或建立）你的憑證：
   - GitHub 節點 → GitHub API 憑證（貼你的 token）
   - M1 / M2 節點 → Mistral 憑證（課堂提供的 API key）

5. **測試**
   用表單上傳一份小的 PDF，跑完後回你的 repo 看：
   `wiki/` 裡多了一篇文章、`index.md` 的文章清單多了一個區塊 → 成功。

## 目錄結構

| 路徑 | 說明 |
| --- | --- |
| [README.md](README.md) | 本說明 |
| [index.md](index.md) | Wiki 目錄＋摘要層，由 n8n 自動維護 |
| [wiki/](wiki/) | 所有 LLM 產生的知識文章 |
| [n8n/llm-wiki-workflow.json](n8n/llm-wiki-workflow.json) | n8n workflow（改好帳號後匯入） |
| [docs/](docs/) | 圖文教學 |

## 文章格式約定

每篇寫入 `wiki/` 的文章維持以下格式（workflow 裡的 AI 已被教好會照做）：

```markdown
---
title: 文章標題
source: 原始檔案名稱
created: 2026-07-22
updated: 2026-07-22
tags: [標籤1, 標籤2]
summary: 兩到三句的摘要，這一段會被複製到 index.md 當作摘要層。
---

## 摘要
## 正文
## 待議與備註
## 實體卡片
## 知識三元組
## 原文出處
```

檔名使用 `YYYY-MM-DD-主題.md`，例如 `wiki/2026-07-22-rag-基礎.md`。

## ⚠️ 使用須知

- **token 絕對不能放進 repo 或截圖**，只貼在 n8n 憑證欄。外洩就到
  GitHub Settings → Developer settings 刪掉重發。
- **不要短時間反覆執行**：一次一份文件，失敗先看錯誤訊息再重試，間隔至少一分鐘。
- **不要批次上傳或掛排程**：這是教學流程，設計上一次處理一份。
- **用自己的 token，不要共用**。
- 練習用 repo，不要放公司機密或個人隱私。

## 授權

教學用途，內容以 CC BY 4.0 釋出。
