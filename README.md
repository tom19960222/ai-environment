# Ai-environment

個人 AI agent 與工作流程設定。提供共用技術報告寫作 skill，以及 OpenCode 的技術研究與獨立覆核 agents。

## 共用報告寫作 skill

[writing-experiment-reports](./skills/writing-experiment-reports/SKILL.md) 用於撰寫與修訂技術報告：單次／多次／階段性實驗、純原始碼／文件研究，以及混合材料都可使用。開頭讓讀者快速掌握結論、限制與行動，後文保留可回查的證據；依材料選用格式，沒有實測也能用，不補造量測或事前預測。

將整個 `skills/writing-experiment-reports/` 目錄複製到使用工具支援的 skill 目錄，保留 `SKILL.md`、`report-template.md` 與 `references/`。例如 OpenCode 可使用專案的 `.agents/skills/`、`.opencode/skills/`，或全域的 `~/.agents/skills/`、`~/.config/opencode/skills/`；選一個位置，避免同名技能重複。這份 skill 可獨立使用，不依賴 OpenCode agent 或 learning-k8s 的目錄慣例。[OpenCode 技能探索說明](https://opencode.ai/docs/skills/)

## OpenCode 研究流程

| Agent | 用途 |
|---|---|
| [researcher](./opencode/agents/researcher.md) | 查閱官方文件與目標版本原始碼，研究技術問題、比較方案、撰寫可支援決策的報告 |
| [research-reviewer](./opencode/agents/research-reviewer.md) | 獨立查核證據，並以新的讀者上下文測試報告是否容易理解 |

研究流程：確認讀者與大綱 → 查證與寫作 → 獨立技術查核及讀者測試 → 修訂與重審 → 交付。

將 `opencode/agents/` 中的兩份檔案放進目標專案的 `.opencode/agents/`，或全域的 `~/.config/opencode/agents/`。`researcher` 會在正式報告寫作或修訂時載入可用的共用 skill；各 agent 的模型由使用者自行指定，預設不設定研究時間或成本上限。

[完整使用與設定說明](./opencode/README.md) · [驗收案例](./opencode/ACCEPTANCE.md) · [目前驗證範圍](./opencode/VALIDATION.md)
