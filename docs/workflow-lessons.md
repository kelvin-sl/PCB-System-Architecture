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

## 約束指派與驗證範圍 / Constraint assignment and verification scope

指派既有共用約束集與修改該約束集的定義，是影響範圍不同的操作。結果應說明實際執行哪一項，並以目標讀回及非目標指派比對支持相應範圍；指派比對本身不能證明共用定義未變。
Assigning an existing shared constraint set and editing its definition have different scopes. State which operation occurred and support the stated scope with target readback and comparison of non-target assignments. Assignment comparisons alone do not establish that the shared definition was unchanged.

差分對的存在及成員，應與訊號命名和約束指派分別核對。名稱看似成對或共用約束集，不能單獨支持差分對已建立的結論。
Verify differential pair existence and membership separately from signal names and constraint assignments. Paired-looking names or a shared constraint set alone do not establish that a differential pair was created.

對內間距與異網間距是不同的規則，即使顯示數值相同，也須保留各自的規則種類、適用範圍與單位。設定讀回只支持相應設定的狀態，佈線、連通、設計規則及電氣結果仍需各自驗證。
Intra-pair gap and clearance to other nets are different rules. Even when displayed values match, retain each rule's type, scope and units. Configuration readback supports the corresponding configuration state; routing, connectivity, design rules and electrical results each require their own verification.

資料庫可能同時以一般屬性與原生物件關聯表示設計資訊。單一屬性查詢沒有值，只能支持該查詢未讀到值；在判定關聯不存在前，應核對原生關聯及成員資料。若新證據推翻先前結論，應修正結論並保留尚未核對的成員與極性範圍。

A database may expose design information through both ordinary properties and native object relationships. An empty property query establishes only that the query returned no value. Before concluding that a relationship is absent, inspect the native relationship and its members. When new evidence overturns an earlier conclusion, revise it while retaining any unverified membership and polarity scope.

## 比較基準與例外條件 / Comparison baselines and exceptions

零差異的結論取決於預期基準與比較條件。若驗證把操作後觀察到的結果寫回預期值，或排除失去網路歸屬的銅線、容許網路重新歸屬，結果應明列這些調整及其影響範圍；它不能單獨證明原有幾何與網路歸屬完整保留。例外需有獨立依據，並另行核對受影響的連通與修改授權。

A zero-difference result depends on the expected baseline and comparison criteria. If verification replaces expected values with observed results after the operation, excludes copper that lost its net association, or permits net reassignment, state those adjustments and their scope. The result alone does not establish full preservation of the original geometry and net ownership. Exceptions require independent support, with affected connectivity and authorization checked separately.

## 設計規則差異的辨識範圍 / Scope of design rule comparisons

設計規則違規總數相同，仍可能包含不同違規；只用座標與規則名稱作索引，也可能把同位置同名稱的多筆違規合併。結果應說明比較鍵、重複項處理及檢查時點，並保留可區分違規的層、涉及物件與規則內容。交易內的差異比較不能自動延伸為提交後的完整驗證。

Equal design rule violation totals can contain different violations. Indexing only by location and rule name can also merge multiple violations with the same key. State the comparison key, handling of duplicates and time of inspection, and retain distinguishing layer, involved objects and rule details. A comparison within a transaction does not automatically establish complete verification after commit.

## 觀察結果與提交條件 / Observations and commit conditions

報告記錄後置條件的值，與程式以該條件決定是否提交，是不同的證據。審查時應核對失敗條件是否真的阻止提交，並保留提交後的讀回；成功案例本身不能證明失敗分支已實測。

Logging a postcondition and using it to decide whether to commit provide different evidence. Review whether a failed condition actually prevents commit, and retain readback after commit. A successful run alone does not establish that the failure branch was tested.

非目標網路的未連接總數沒有增加，只支持該總數的結論；固定端點是否仍經銅線互通，需以指定端點另行核對。

An unchanged unconnected total for a non-target net supports only that aggregate result. Preserved copper connectivity between fixed endpoints requires a separate check of those endpoints.

## 採樣幾何與精確保留 / Sampled geometry and exact preservation

將幾何轉成經過取整的離散點集合，可協助比較拆分或合併後的路徑；但取樣間隔、取整與集合去重都會縮減可辨識的資訊。若圓弧只以端點間的線性取樣表示，曲率資訊也未被保留。採樣比對通過，只支持該採樣模型內的相符，不能單獨證明曲線、重複幾何或既有蛇線完整保留。

Converting geometry to a rounded set of sampled points can help compare paths after splitting or merging. Sampling intervals, rounding and set deduplication reduce the information that the comparison can distinguish. Representing an arc by linear samples between its endpoints also omits curvature. A passing comparison establishes agreement within that sampling model; it does not by itself establish complete preservation of curves, duplicate geometry or existing meanders.

審查時應說明哪些幾何使用精確記錄、哪些使用近似採樣，並分別保留必要的曲線語意、重複次數及固定端點連通證據。近似比對與端點連通各自通過，仍不會自動補足對方未檢查的範圍。

State which geometry uses exact records and which uses approximate samples, and retain the required curve semantics, multiplicity and fixed endpoint connectivity evidence separately. Passing approximate geometry and endpoint connectivity checks does not automatically cover what either check omits.
