# 驗證範圍

初版與報告 skill 整合查核日期：2026-09-10。

已完成：

- 本機 Ruby Psych 安全解析兩份 agent 的 YAML frontmatter。
- 對照 OpenCode V1 原始碼，核對 `description`、`mode`、`permission` 與 `task` 的 `subagent_type`／`task_id` 介面，以及 `skills.paths` 設定。
- 確認主 agent 的呼叫名稱與 reviewer 檔名一致；模型、`steps`、`maxSteps` 及研究時間／成本上限均未設定。
- 由未參與兩份 prompt 寫作的獨立 agent 進行靜態覆核；已修正技能缺失時不必要的再次批准，以及正式派工與引用內容指令界線的歧義。最終沒有未解的實質問題。

尚未執行 OpenCode runtime／模型行為測試，也未執行目標 fork 的完整 parser／schema 載入。上述 YAML 與欄位檢查不能證明模型實際遵循流程。

選定模型後，可用 [ACCEPTANCE.md](./ACCEPTANCE.md) 驗證實際工具呼叫、兩種 reviewer 的獨立上下文、修訂後重審與無進展收斂。

## 共用報告 skill 整合

新增 `skills/writing-experiment-reports/`，支援實驗、原始碼／文件與混合材料的報告；researcher 在正式報告寫作及修訂時載入可用技能。

已完成的套件檢查：

- 執行 skill-creator 的 `quick_validate.py`，檢查通過。驗證依賴放在該次工作區，未安裝或更新本機 skill。
- 用 Ruby Psych 安全解析 skill 與兩份 agent 的 YAML frontmatter；相對引用、支援檔案、JSON 設定與空白／衝突標記檢查通過。
- 獨立靜態 reviewer 查核適用範圍、依證據選用格式、技能載入與權限、可攜性及既有覆核政策，沒有未解的實質問題。
- 原有 `research-reviewer.md` 與 `opencode-skills.example.json` 保持原內容。既有八個驗收案例保留，新增五個案例並更新清單狀態說明。

另以假想程式與示意數據，在 Codex 的獨立 agent 中進行兩個寫作案例；這些不是任何實際產品的研究結果。作者使用共用 skill，技術 reviewer 取得題目、成品與原始材料，讀者 reviewer 各用全新上下文且只讀成品。實際結果列於下表。

| 案例 | 核對重點 | 結果 |
|---|---|---|
| 只有原碼的重試行為研究 | 區分三次嘗試與兩次重試、指定等待與總耗時；不虛構實測或 100 ms 成功保證 | 獨立技術查核與讀者測試通過 |
| 混合量測、原碼與未完成項目的階段報告 | 18 ms → 88 ms（增加 70 ms）、保留原統計判準、缺少事前預期、不同 IOPS 口徑、未測參數及未完成工作 | 獨立技術查核與讀者測試通過 |

上述試寫測試的是技能在 Codex agent 中的寫作行為與成品，未驗證 OpenCode 的技能探索、`skill`／`task` 實際呼叫或其執行期權限。試寫的獨立審查由驗收主代理安排，不能據此宣稱 OpenCode researcher 已能自行跑完整流程。一般修訂、技能缺失、停滯處理等其他情境仍依 [ACCEPTANCE.md](./ACCEPTANCE.md) 執行驗收。
