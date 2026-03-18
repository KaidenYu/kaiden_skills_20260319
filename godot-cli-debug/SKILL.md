---
name: Godot CLI Debugger
description: (v4.x/v3.x) 一套用於自動化驗證 Godot 專案改動、檢查腳本語法與場景完整性的除錯工具與工作流。支援版本偵測、--help 指令診斷與穩定性強化。
---

# 技能：Godot CLI 除錯助手 (Godot 4.x / 3.x)

## 觸發情境
當對 Godot 專案進行邏輯、場景或資源修改後，需要自動驗證變更是否正確，或者在回報任務完成前作為最後的品質保證。

## 0. 核心配置與診斷 (Phase 0)
在執行任何除錯工作前，應先確認環境路徑並利用 Godot 內建指令進行「能力診斷」：
- **Godot 控制台路徑 (`[GODOT_EXE]`)**: `C:\kaiden\software\Godot_v4.6.1-stable_win64.exe\Godot_v4.6.1-stable_win64_console.exe`
- **能力診斷 (Diagnostic)**:
  1. **版本檢查**: `& "[GODOT_EXE]" --version` (獲取大版本號以選用工作流)。
  2. **參數驗證 (最保險)**: `& "[GODOT_EXE]" --help` (直接檢查該執行檔支援哪些 Flag，例如是否有 `--headless` 或 `--quit-after`)。

---

## 核心工作流

### 1. 腳本語法與靜態檢查 (`--check-only`)
檢查單個 `.gd` 腳本是否存在語法錯誤或編譯問題。
- **Godot 4.x 指令**:
  ```powershell
  & "[GODOT_EXE]" --headless --check-only -s "res://path/to/script.gd"
  ```
- **Godot 3.x / 舊版**: 若不支援 `--headless`，請改用 `--no-window`。
- **進階檢查 (包含警告訊息)**:
  加上 `--debug` 捕捉潛在邏輯陷阱：
  ```powershell
  & "[GODOT_EXE]" --headless --debug --check-only -s "res://path/to/script.gd"
  ```

### 2. 場景與資源完整性檢查
驗證 `.tscn` 文件是否能成功載入，主要用於檢查是否出現「丟失的 Node」或「損壞的外部資源路徑」。
- **Godot 4.x 指令**:
  ```powershell
  & "[GODOT_EXE]" --headless --path "[PROJECT_PATH]" --quit-after 2 "res://path/to/scene.tscn"
  ```
- **Godot 3.x 轉譯建議**: 使用 `--no-window -q` 替代 `--headless --quit-after 2`。
- **參數說明**: `[PROJECT_PATH]` 為包含 `project.godot` 的專案目錄絕對路徑。

### 3. 全專案初始化與匯入 (Editor 模式)
當修改了 Autoload (單例) 或大批全域資源時，使用編輯器模式強迫 Godot 進行完整的專案掃描與資源重匯入。
- **通用指令**:
  ```powershell
  & "[GODOT_EXE]" --headless --path "[PROJECT_PATH]" --editor --quit-after 2
  ```

---

## 專家提示與穩定性強化 (Expert Tips)

### 參數相容性診斷
若執行失敗並報出 `Unknown argument` 錯誤：
1. 請查看 Phase 0 的 `--help` 輸出。
2. 搜尋相似功能的替代方案（例如搜尋 "quit", "window", "display" 等關鍵字）。

### 提高 Headless 模式穩定性
在某些大型專案中，立即退出 (`--quit`) 可能會導致資源匯入中斷。
- **建議**: 使用 **`--quit-after 2`** (Godot 4.x) 或 **`--quit-after 5`** 確保引擎有足夠幀數處理快取。

### 錯誤修復協議 (Feedback Loop)
1.  **擷取除錯訊息**: 執行上述指令並讀取輸出。
2.  **定位問題與行號**: 
    - **GDScript**: 輸出會精確定位出錯誤行號。
    - **TSCN/資源**: 報出的行號通常是損壞區塊的結尾，建議檢查該行號及其鄰近的配置內容。
3.  **執行修正**: 修正檔案後，**務必再次執行檢查**直到達成 `Exit code 0`。

## 技巧與限制
- **Autoload 依賴**: 單獨檢查腳本時若引用 Autoload 可能會誤報 identifier 不存在，此時請改用工作流 3 進行全專案掃描。
- **環境感知**: Windows 平台請務必確認路徑結尾為 **`..._console.exe`**，否則 Stdout/Stderr 輸出會被系統吞掉。
