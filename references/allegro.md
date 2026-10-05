# Allegro PCB 作業指引

## 唯讀匯出

核對 PCB Editor 版本、板檔身份及本次需要的範圍。依可用 SKILL API 讀取 design、components、placed symbols、pins，輸出：

- 實體 refdes、footprint、放置狀態。
- 元件 origin、rotation、mirror／side。
- 每個 pin number、pad XY、net。
- 座標單位、精度、匯出範圍與缺失欄位。

API 名稱與欄位需依安裝版本驗證。未放置元件、未知值及空 net 分開表示。名稱欄位取得失敗不必然表示全部資料失敗，但不可自行補猜板檔身份。

載入 `.il` 只定義函式，再明確呼叫匯出。一般 Command 與 SKILL 提示符的呼叫方式不同，提供符合使用者環境的指令。完整括號、字串引號與正斜線路徑都應檢查。

## Physical pinout 解讀

- component origin 與 pad XY 不同。
- top／bottom、rotation、mirror、mating view 會影響觀察方向。
- 不同板的全域座標不能直接當相同 placement。
- 舊板可作固定 pinout 與 pad 鄰接參考，但須核對 footprint、pin numbering 與方向。
- PCB 座標與 schematic 頁面座標屬於不同空間。

## 能提出的結論

只有相對位置圖時，提供有條件的方向性 proposal。要比較實際線長與交叉，需要當前板的端點位置、朝向與可用佈線條件。pad 間距不是已佈線長度，也不能證明 skew 符合要求。

本流程的 PCB 步驟以讀取和分析為主。若要求修改 placement、pin assignment 或 routing，需要另訂具體變更計畫與驗證範圍。
