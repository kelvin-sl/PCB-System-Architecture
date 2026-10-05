# Capture 作業指引

## 產生唯讀盤點

先核對 Capture 版本與使用者當前設計，依任務選擇 schematic、頁面或階層 instance。root schematic 的盤點不等於所有階層實例；輸出要寫清楚範圍。

收集：頁面、元件及 section、實體 pin number、symbol pin name、pin net、No Connect、wire 端點、alias、off-page 名稱與附著 wire、符號方向及顯示属性。關鍵欄位缺失時標記並說明哪些結論受影響。

### 身份與座標

- 實體 connector／pin number 是配對依據。symbol pin name 可能不同。
- 多個 schematic section 可能屬於同一實體 connector；核對 reference 關係後合併，不靠名稱截斷猜測。
- 迭代序號與 Dbo pointer 只能作該次讀取的線索，不作跨執行的持久識別。
- 分清 page XY、local XY、symbol origin、pin connection point，並保留單位。
- 位置重疊不等於電氣連通。wire 幾何命中與 pin net 分別核對。

### API 使用方式

採用當前版本確認的 Dbo API。名稱讀取可能需要 CString buffer；座標可能回傳 CPoint；iterator 與狀態物件按對應 API 使用與釋放。

可查找的 API 線索：GetActivePMDesign、GetRootSchematic、NewPagesIter、NewWiresIter、NewAliasesIter、NewOffPageConnectorsIter、GetWire、NewPinsIter、GetPinNumber、GetNetName、GetIsNoConnect、GetOffsetHotSpot、GetHotSpot。

以上是探索線索，不是跨版本 signature 保證。優先讀當前環境文件、已驗證的通用程式或最小唯讀 probe，確認參數與回傳值後才生成修改程式。

## 定位與修改

一條 wire 可能有 alias、off-page 或兩者。不同定位來源必須核對一致；查不到 alias 不代表 wire 不存在。查到多個候選時列出差異，不任選第一個。

複製既有 off-page template 前核對 source symbol、orientation、mirror、接點偏移及顯示属性。template 位置不能直接当新目標位置；每次執行從 live pin／wire 重新計算。

使用經驗證的狀態判讀及 readback 確認寫入。catch 沒有 Tcl error 不足以證明 Dbo 操作完成。尚不清楚 API 時先 probe，避免猜測修改呼叫。

## Name／IREF

- 符號、Name、IREF 分別處理；鏡像符號不保證文字排列同步正確。
- 依文字 bounds、方向及適當間距排版，不固定使用某次名稱的數字座標。
- 頁碼來源與頁面名稱分開；依真正頁碼屬性及連線 scope 產生對應頁碼集合。
- refresh／重新產生 intersheet references 可能改變顯示，需讀回並檢查畫面。
- 修正外觀後檢查符號仍附著預期 wire。

## 執行交付

產生可下載的純 `.tcl` 檔，提供以大括號包住實際路徑的 source 指令。採正斜線避免 Windows 路徑跳脫；不要放入已過期的個人路徑。

修改腳本分開定義、預覽與 apply。只載入檔案不應意外改線。未在 Capture 執行的新腳本標示待實測。
