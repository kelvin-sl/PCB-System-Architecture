# Workflow lessons / 通用驗證經驗

這份文件整理可公開分享的 PCB 修改與驗證經驗，供人閱讀與檢視工作成果。
This document summarizes general lessons for reviewing PCB changes and their verification.

它描述證據的解讀與限制，不提供可執行步驟，也不構成工具 API 或設計結果的保證。
It explains evidence and its limits; it provides no executable procedures or guarantees about tool APIs or design outcomes.

## 操作結果與儲存狀態 / Operation results and saved state

刪除佈線後的讀回，可支持指定佈線已移除的結論；板檔是否成功儲存，仍是另一項狀態。
Readback after route deletion can support that the specified routing was removed. Whether the board was saved successfully is a separate status.

每項結論應指明操作範圍與對應證據，將修改讀回、恢復驗證與儲存確認分別記錄。
Each conclusion should identify its scope and evidence, with change readback, recovery verification, and save confirmation recorded separately.

局部操作成功，不能擴大解讀為整項清理完成或整個設計已驗證。
Success for a local operation does not establish completion of the whole cleanup or verification of the entire design.

## 中間檢查與最終結果 / Intermediate checks and final outcomes

元件清理可能通過部分中間幾何檢查，之後仍然失敗並回復；中間 PASS 只適用於當時檢查的條件。
Component cleanup can pass some intermediate geometry checks and still fail and roll back. An intermediate PASS applies only to the condition checked at that stage.

只有完整操作及必要後置條件都有證據，才足以支持相應範圍的最終成功。
Final success within a stated scope requires evidence for the complete operation and its necessary postconditions.

若確切根因未被記錄，應明列為未知；最後通過的檢查不能用來推定失敗原因。
If the exact root cause was not recorded, state that it is unknown. The last passing check does not identify the cause of failure.

## 路徑物件與線段幾何 / Path objects and segment geometry

上層路徑拆分或合併可能改變物件數量，而經正規化的線段幾何仍可能保留。
Splitting or merging parent paths can change object totals while normalized segment geometry may be retained.

比較資料時，須保留網路、層、線寬、所屬關係、圓弧與導通孔語意，以及相同記錄的重複次數。
Comparisons must preserve net, layer, width, ownership, arc, and via semantics, as well as the multiplicity of matching records.

線段記錄與路徑物件應使用不同標籤；單靠物件數量相同或不同，都不足以判定幾何是否保留。
Segment records and path objects need different labels. Matching or differing object totals alone cannot establish whether geometry was retained.

幾何比對的結論限於其比較模型；它不能證明實際銅線連通或設計規則檢查通過。
A geometry comparison supports conclusions only within its comparison model. It does not prove physical copper connectivity or a passing design rule check (DRC).

## 錯誤證據與診斷 / Error evidence and diagnosis

圖形介面顯示的錯誤可能未寫入輸出檔；檔案沒有錯誤文字，不能證明執行沒有錯誤。
Errors displayed in the graphical interface may be absent from an output file. An error-free file does not prove an error-free execution.

診斷證據應包含失敗階段、失敗的斷言或物件細節，以及原始錯誤訊息。
Diagnostic evidence should include the failing phase, assertion or object details, and the original error message.

證據不足時，保留不確定性，並將已觀察到的失敗與推測原因清楚區分。
When evidence is incomplete, retain the uncertainty and distinguish the observed failure from any proposed explanation.

## 批次範圍與恢復基準 / Batch scope and recovery baselines

大型清理適合拆成可檢視的批次；各批次應有自己的基準、預覽、套用結果與讀回證據。
Large cleanup benefits from reviewable batches, each with its own baseline, preview, apply result, and readback evidence.

失敗批次的恢復，應以最近一次已接受的基準為依據，並驗證實際恢復範圍。
Recovery of a failed batch should use the latest accepted baseline and verify the scope actually restored.

回復呼叫成功與恢復讀回通過是不同證據；未經驗證的任意 API 修補，不能稱為已驗證的恢復方法。
A successful rollback call and passing recovery readback are different evidence. An arbitrary API repair cannot be called a verified recovery method without validation.

## 快照與結論的界線 / Snapshot and conclusion limits

Shape 快照只涵蓋當時擷取的描述欄位；它不自動涵蓋所有填銅、挖空區或約束。
Shape snapshots cover only the descriptors captured. They do not automatically cover every fill, void, or constraint.

結果說明應清楚區分已通過、失敗與尚未驗證，並讓每項成功聲明停留在證據涵蓋的範圍內。
Result summaries should distinguish passed, failed, and unverified items, keeping every success claim within the scope supported by evidence.

動態填銅可能使幾何與物件身份呈現不同結果。身份、網路、所屬關係、層與邊界角色通過核對時，仍應明列幾何是否相同，以及比較條件是否改變。將差異標為重新填銅是一項待支持的解釋，不能單靠相同網路或層就認定變更可接受；連通、設計規則與儲存仍須各自確認。

Dynamic copper fill can produce different results for geometry and object identity. When identity, net, ownership, layer and boundary role checks pass, state separately whether geometry matches and whether comparison criteria changed. Labeling a difference as repouring is an explanation requiring support. Matching net or layer alone does not establish that a change is acceptable; connectivity, design rules and saved state each need their own confirmation.
