# OpenCode 技術研究與獨立覆核 agents

這兩份 agent 採用以下研究流程：官方證據優先、主動檢查反例、依讀者寫作，每份成果都經獨立技術查核與讀者測試。兩個 agent 的模型由你自行指定；檔案沒有填入模型或研究時間／成本上限。

| Agent | 檔案 | 職責 |
|---|---|---|
| `researcher` | [researcher.md](./agents/researcher.md) | 與你確認讀者、用途及大綱；查證、比較、寫作、補證與修訂 |
| `research-reviewer` | [research-reviewer.md](./agents/research-reviewer.md) | 分別執行技術查核與讀者測試，提出有依據的獨立意見 |

研究流程：**確認讀者與方向 → 官方文件／目標版本原始碼查證 → 寫作 → 技術查核＋讀者測試 → 修訂與必要重審 → 交付。**

同一個 reviewer 定義會以兩個不同的新子 session 呼叫。技術查核取得問題、範圍、報告及證據；讀者測試只取得交付報告、讀者設定與閱讀問題，並實際回答問題，避免用研究背景替正文補洞。

## 放進 OpenCode

在要使用的專案建立 `.opencode/agents/`，放入這兩個檔案：

```text
你的專案/
└── .opencode/
    └── agents/
        ├── researcher.md
        └── research-reviewer.md
```

若要所有專案共用，可改放 `~/.config/opencode/agents/`。檔名就是 agent 名稱；主研究 prompt 使用 `research-reviewer` 呼叫覆核者，改名時需一起改主 agent 的指令與 `permission.task`。這是 OpenCode V1 支援的 Markdown agent 方式。[官方設定文件](https://opencode.ai/docs/agents/#markdown)

要指定模型，在**每個檔案開頭的 YAML 區塊**各加一行，填入你實際使用的 provider／model ID：

```yaml
model: 你選定的-provider/你選定的-model
```

上例只是欄位示意，不能直接把示意文字當模型 ID。兩個 agent 可各自設定；若你已有同名 agent 的集中設定，合併至既有設定並核對最終生效值。[模型設定](https://opencode.ai/docs/agents/#model)

在 OpenCode 選擇 `researcher` 作為主 agent，即可開始提問。若要自行審查已有報告，可直接 `@research-reviewer`，並明確給 `mode: technical` 或 `mode: reader` 及對應輸入；兩種模式分開開新子 session。一般研究流程會由 `researcher` 自行安排。[子 agent 使用方式](https://opencode.ai/docs/agents/#subagents)

## 接上報告寫作 skill

`researcher` 會在撰寫或修訂正式技術報告時，以 `skill` 載入可用的 [writing-experiment-reports](../skills/writing-experiment-reports/SKILL.md)。範圍包括單次／多次／階段性實驗、純原始碼／文件研究與混合材料；技能統一寫作方式，再依證據選擇合適章節。沒有實測不填實驗表格，單一事實查詢也不硬套長報告。

把本 repository 的整個 `skills/writing-experiment-reports/` 目錄複製到下列任一位置，保留全部支援檔案：

```text
你的專案/.opencode/skills/writing-experiment-reports/
你的專案/.agents/skills/writing-experiment-reports/
~/.config/opencode/skills/writing-experiment-reports/
~/.agents/skills/writing-experiment-reports/
```

安裝後的目錄應包含：

```text
writing-experiment-reports/
├── SKILL.md
├── report-template.md
└── references/
    └── review.md
```

上列位置由 OpenCode 自動探索，選一個即可；只把原始檔留在本 repository 的 `skills/` 不代表已被探索。確認 `skill` 工具列出 `writing-experiment-reports`，並在實際報告任務檢查有呼叫 `skill({ name: "writing-experiment-reports" })`。若未出現，核對目錄、frontmatter 名稱、同名技能及生效權限；本套件 researcher 已明確允許這個技能。[官方技能文件](https://opencode.ai/docs/skills/)

共用 skill 不依賴這兩個 agents，也可在其他支援 `SKILL.md` 的工具使用。整合到 OpenCode 後，研究範圍、已定讀者、大綱、操作授權與兩種獨立覆核仍依 researcher 的契約；技能內的檢查不代替實際 reviewer 派工。

## 接上 doc-coauthoring

`researcher` 會在寫作階段另載入可用的 `doc-coauthoring`，協助脈絡整理與結構修訂，沿用已確認的讀者與大綱，不強制逐節做大量選項投票。任一寫作技能尚未可用時，會說明哪項未載入並提供接通方式，以已定寫作及獨立覆核流程繼續，不為可選依賴重新要求批准。

若該技能已在 OpenCode 可發現的位置，就不必另外設定，例如：

```text
~/.agents/skills/doc-coauthoring/SKILL.md
~/.config/opencode/skills/doc-coauthoring/SKILL.md
你的專案/.opencode/skills/doc-coauthoring/SKILL.md
```

如果你已將技能安裝在 `~/.codex/skills/doc-coauthoring/SKILL.md`，可把 [opencode-skills.example.json](./opencode-skills.example.json) 的 `skills.paths` **合併**進既有的 `opencode.json`／`opencode.jsonc`，保留其他設定及原有路徑。此範例只增加技能來源，不設定模型、不取代你的整份設定。在其他主機上，改成當地實際存在的技能路徑。[技能路徑設定](https://opencode.ai/docs/skills/)

## 預設行為

- 每個新研究確認讀者、用途與大綱；同題已回答的事沿用，不重複 grill。有實質缺口時集中提問，最多五題並附建議。
- 優先官方文件及目標版本原始碼，處理相反證據；版本、來源位置、數字與單位可核對。
- 報告適合獨立閱讀。正式報告先給約兩三分鐘的決策重點，後文提供足夠背景與證據；小問題按比例縮短。原始碼／文件支持、實測與推論分開交代，不從結果補寫不存在的事前預測。
- 修訂會整合回摘要、正文、表格及結論，消除受影響的舊說法；其他文件的連動修正遵守當次授權範圍。
- 每份研究都做兩種獨立覆核。實質修改後重新檢查受影響部分，舊稿通過不冒充新稿通過。
- 連續兩輪修訂與覆核沒有實質進展時，呈現分歧與影響，讓你決定是否繼續；保持未通過狀態。這不是總共只能 review 兩次。
- 不例行詢問預算或期限、不設研究 cap。證據足以支援目前判斷且覆核通過，就交付。
- 預設成果為 `research/<主題>/report.md`、`evidence.md`、`review.md`；可依你指定的交付目錄或格式調整。
- 第一版交付研究、判斷與驗證方法。實驗執行、修改目標系統與發布，仍依當次授權。

主 agent 沿用你的既有工具權限，允許呼叫指定 reviewer、`writing-experiment-reports` 及 `doc-coauthoring`。Reviewer 限讀取、搜尋與網頁查核，可讀本題相關的外部 corpus／repository，不能寫檔、執行 shell、再派 agent 或直接問你問題；讀者模式還會限制自己只讀交付成品。

## 驗證範圍

本套件採 OpenCode V1 的 Markdown agent 與 `permission` 設定格式，已對照 V1 原始碼核對欄位及 `task` 呼叫介面。使用其他版本時應核對該版本的相容性。[官方 agent 文件](https://opencode.ai/docs/agents/)

本機解析、設定查核與靜態覆核的實際完成範圍見 [VALIDATION.md](./VALIDATION.md)。另提供 [實際使用驗收案例](./ACCEPTANCE.md)，用來檢查技能載入、按證據寫作、模型是否真的依流程開啟 reviewer、隔離上下文及重新送審。

目前尚未執行 OpenCode 模型研究驗收。每次必審是 prompt 的工作流程要求；若模型未遵循，agent 設定本身不會以程式強制攔截。`ACCEPTANCE.md` 因此要求查看實際派工紀錄，不只看模型自稱通過。
