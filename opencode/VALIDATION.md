# 驗證範圍

初版查核日期：2026-09-10。

已完成：

- 本機 Ruby Psych 安全解析兩份 agent 的 YAML frontmatter。
- 對照 OpenCode V1 原始碼，核對 `description`、`mode`、`permission` 與 `task` 的 `subagent_type`／`task_id` 介面，以及 `skills.paths` 設定。
- 確認主 agent 的呼叫名稱與 reviewer 檔名一致；模型、`steps`、`maxSteps` 及研究時間／成本上限均未設定。
- 由未參與兩份 prompt 寫作的獨立 agent 進行靜態覆核；已修正技能缺失時不必要的再次批准，以及正式派工與引用內容指令界線的歧義。最終沒有未解的實質問題。

尚未執行 OpenCode runtime／模型行為測試，也未執行目標 fork 的完整 parser／schema 載入。上述 YAML 與欄位檢查不能證明模型實際遵循流程。

選定模型後，可用 [ACCEPTANCE.md](./ACCEPTANCE.md) 驗證實際工具呼叫、兩種 reviewer 的獨立上下文、修訂後重審與無進展收斂。
