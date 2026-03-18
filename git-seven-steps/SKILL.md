---
name: Git 七招 (Git Seven Steps)
description: 一套標準化的 Git 工作流程，包含狀態檢查、差異分析、風格參考、同步拉取、暫存、提交及推送。
---

# 技能：Git 七招

## 觸發條件
當使用者提及「做 git 七招」或類似請求時。

## 執行流程 (第一階段：準備與確認)
1.  **第一招：檢查狀態 (git status)** - 執行 `git status` 確認哪些檔案有變動或未追蹤。
2.  **第二招：分析差異 (git diff)** - 執行 `git diff` 閱讀程式碼變更內容，並初步編寫一個 commit message 的草稿。
3.  **第三招：風格參考 (git log)** - 執行 `git log -n 5` 並參考先前專案的寫法風格，對第二招的 commit message 草稿進行修正，得到最終建議。
4.  **回報與等待** - 將最終擬定的 commit message 告訴使用者，並詢問是否 OK。
    - **關鍵點**：如果使用者不滿意，則與使用者討論並重複修正，直到使用者確認 OK。

## 執行流程 (第二階段：執行與推送)
使用者確認 OK 後，接續執行：
5.  **第四招：審查與拉取 (git pull)** - 執行 `git pull` 確保本地版本與遠端同步，避免直接推送造成衝突。
6.  **第五招：準備暫存 (git add)** - 將第一步看到的異動檔案進行 `git add`。
7.  **第六招：完成提交 (git commit)** - 使用被認可的訊息執行 `git commit -m "[Approved Message]"`。
8.  **第七招：推送遠端 (git push)** - 執行 `git push` 並回報成功。

## 規範與準則
- 始終在執行 `git add` 之前執行 `git pull` 作為審查機制。
- Commit message 應力求專業，並符合專案既有風格。
- 只有在使用者明確核准 commit message 後，才能接續執行後面的四招。
