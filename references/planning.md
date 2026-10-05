# 配對與變更計畫

## 先建立約束

從使用者需求整理：固定 connector pinout、固定訊號群與 bank 對應、極性政策、允許交換範圍、保留功能、電氣條件。把本次策略和不可變更要求分開；不把過往案例的保留選擇當通用規則。

固定端使用實體 connector 與 pin number，保留 schematic section 作定位。重複身份要先釐清，不用座標或 pin name 取代腳號。

## 配對順序

1. 固定 assignment、群組／bank 與極性。
2. 電氣可行性：元件完整型號、package、clock pin 能力、clock region、I/O standard、bank 電壓、專用功能、No Connect。
3. 既有占用與本次允許替換範圍。
4. 當前板的 connector placement、方向、pad 鄰接及差動對完整性。
5. 比較可行候選的交叉、繞行與線長條件。

舊 net 名包含 clock 用語不是時鐘能力證據。資料不足時可提供 provisional proposal，將假設與待核對條件跟著 mapping 保存，不標成電氣驗證通過。

## 每條線的變更

記錄 signal、兩端 page／ref／pin、bank alias、old net、existing off-page、new net、action、reason、pending、source evidence。

- KEEP：現況已符合，不重複新增。
- RENAME：確定可取代的舊用途，改名後讀回 pin net。
- CREATE：確定所需物件不存在才新增。
- MOVE：遷移指定端點，保留固定端並核對搬離端。
- REMOVE：只解除此次明確需要移除的物件；同名遠端不自動連帶刪除。

檢查同 wire 的多個 alias／off-page、branch 與 junction，避免只改文字仍保留不應存在的連通。預覽列出範圍內的殘留舊端點及預期影響。

## 執行邊界

以穩定身份保存計畫，每次 apply 重新定位。位置變了可更新幾何；assignment、占用或數量不同就停止更新計畫。保存 before／after 及部分成功項目。

新 symbol、方向或顯示布局可先做代表性小範圍驗證；已實測模式可批次。依使用者要求的逐步節點進行，不把每條線都設定成必須重複確認。
