# PCB System Architecture — Cadence Connectivity

**繁體中文** | [English](README.en.md)

**以可追溯的證據，逐步完成 schematic 與 PCB 接線協作。**

[MIT 授權](LICENSE) · [安裝與使用](#快速開始) · [貢獻指南](CONTRIBUTING.md)

`cadence-connectivity` 是供 Codex 使用的作業流程 skill，涵蓋 OrCAD Capture Tcl 與 Allegro PCB Editor SKILL。內容以繁體中文撰寫，協助使用者從現況盤點、配對與修改預覽，走到執行讀回及驗證。

An instruction-based Codex skill for evidence-driven schematic and PCB collaboration: connectivity, placement, incremental routing, execution readback, and recovery. The workflow guides are written in Traditional Chinese.

## 適合處理什麼

| 工作 | Skill 提供的引導 |
| --- | --- |
| Schematic 接線 | 固定端 pinout、訊號配對、off-page 附著、極性、既有連線替換與顯示修正 |
| 舊板沿用 | RefDes、netlist 與既有 placement／routing 的盤點及變更紀錄 |
| Placement | 共同基準版本、座標轉換、footprint origin、Pin 1、板框與 Keepin |
| 逐段佈線 | 端點、周邊障礙、有效間距規則、舊線／via 處理及分階段驗收 |
| 執行與恢復 | 腳本／報告版本、手動或授權入口、讀回、失敗停止及 rollback 證據 |

本倉庫提供指引與參考文件；不附帶 MCP server、CAD 執行代理、通用 autorouter 或專案設計檔。腳本須依實際設計與工具版本產生及驗證。

## 快速開始

### 1. 安裝 skill

在支援 `$skill-installer` 的 Codex 環境中貼上：

```text
$skill-installer
請從 https://github.com/kelvin-sl/PCB-System-Architecture 安裝 skill。
SKILL.md 位於 repository 根目錄（path 為 .），安裝名稱為 cadence-connectivity。
若已有同名 skill，先比較版本與本機修改，不要直接覆蓋。
```

也可以下載 GitHub 的 Code → Download ZIP，解壓後將含有 `SKILL.md`、`agents/` 和 `references/` 的完整資料夾命名為 `cadence-connectivity`，放入目前 Codex 環境的 skill 目錄。依官方文件，個人可使用 `~/.agents/skills/cadence-connectivity`；既有環境若使用不同的安裝位置，請沿用該環境的 installer。避免同時安裝多個同名副本，並保留本倉庫的授權與著作權文件。

安裝後，在下一輪對話確認可選取 `$cadence-connectivity`；若未出現，重啟 Codex 後再確認。Skill 的格式與使用方式可參考 [OpenAI 官方文件](https://learn.chatgpt.com/docs/build-skills)。

### 2. 提供現況與固定條件

至少說明使用哪個 CAD 工具與版本、要處理的設計範圍、哪些 pinout／bank／極性不可改，以及目前可取得的報告。專案資料放在自己的專案目錄。

```text
$cadence-connectivity
我要將既有 connector 訊號接到指定 FPGA bank。
外部 connector 的 pinout 固定；bank 內部可調整，但差分 P/N 不可互換。
先依目前資料產生唯讀盤點，列出既有占用與證據缺口，再給我第一版 mapping。
這一輪先做分析與預覽。
```

接續既有任務時可使用：

```text
$cadence-connectivity
這是上一輪腳本的完整 output。請確認它對應目前設計與腳本版本，
區分已通過、失敗及尚未驗證項目，接續已授權的下一步。
```

### 3. 按證據推進

```text
讀取現況 → 固定條件與配對 → 修改預覽 → 執行 → 後置條件讀回
                                            ↓
                             失敗：停止並確認恢復範圍
                                            ↓
                        電氣／佈線／機構驗證與存檔紀錄
```

工具回傳成功只支持該操作的結果。Pin/net 配對、實際銅線連通、DRC、SI、機構及儲存狀態需分別留下證據。

## 文件導覽

- [SKILL.md](SKILL.md)：供 agent 使用的入口與標準循環。
- [Capture](references/capture.md)：Tcl、物件定位、off-page 與文字顯示。
- [Allegro](references/allegro.md)：實體 pinout 匯出及 placement 解讀。
- [配對規劃](references/planning.md)：固定端、既有占用與修改清單。
- [Placement 與邊界](references/placement.md)：座標、對接、共同版本、板框／Keepin。
- [逐段佈線](references/routing.md)：幾何、規則、舊路徑與驗收。
- [執行通道](references/execution.md)：本機／遠端往返、觸發、結果辨識。
- [驗證與恢復](references/verification.md)：輸出核對、失敗與恢復界線。
- [舊板沿用](references/board-reuse.md)：重用設計的修改流程。
- [專案紀錄](references/project-data.md)：私人資料、修改與交付紀錄。

[通用驗證經驗（中英對照）](docs/workflow-lessons.md)：持續整理操作結果、幾何比對、錯誤證據與批次恢復的判讀方法。

## 相容性與驗證範圍

閱讀與安裝本 skill 不需 Cadence runtime。執行產生的 Tcl／SKILL 時，需使用者具備相應軟體、授權、設計存取及可用的執行入口。

目前已進行文件格式、引用、公開內容檢查，以及 placement、routing、rollback、檔案往返與發布邊界五類合成情境的流程評估。這些檢查未構成跨 Cadence 版本的 API 相容性認證，也不保證特定設計可通過 ERC／DRC、訊號完整性或製造驗證。

## 貢獻與問題回報

請先讀 [CONTRIBUTING.md](CONTRIBUTING.md)。歡迎提交可重現的通用流程改善；公開 issue／PR 僅使用合成或經審查的去識別資料，避免上傳真實板檔、接腳表、完整 log、內部路徑與憑證。

[更新紀錄](CHANGELOG.md) 記錄公開 skill 的變更。實際板級進度保存在各自的私人專案。

## 授權與關係聲明

本倉庫內容採 [MIT License](LICENSE)，著作權聲明為 `Copyright (c) 2026 kelvin-sl`。使用、修改或再散布時，請保留 LICENSE 中的著作權與授權聲明。完整條款以 LICENSE 為準；標準授權文本可參考 [Open Source Initiative](https://opensource.org/license/mit)。

此授權適用於本倉庫的內容，不授予 Cadence 軟體、第三方函式庫或使用者私人設計的使用權。

本專案為獨立社群工作流程，未宣稱獲 Cadence 或 OpenAI 官方認證、贊助或背書。產品名稱用於指出適用工具；相關軟體、商標及第三方材料仍由各權利人管理。
