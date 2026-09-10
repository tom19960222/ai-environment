# Ai-environment

個人 AI agent 與工作流程設定。目前提供 OpenCode 的技術研究與獨立覆核 agents。

| Agent | 用途 |
|---|---|
| [researcher](./opencode/agents/researcher.md) | 查閱官方文件與目標版本原始碼，研究技術問題、比較方案、撰寫可支援決策的報告 |
| [research-reviewer](./opencode/agents/research-reviewer.md) | 獨立查核證據，並以新的讀者上下文測試報告是否容易理解 |

研究流程：確認讀者與大綱 → 查證與寫作 → 獨立技術查核及讀者測試 → 修訂與重審 → 交付。

將 `opencode/agents/` 中的兩份檔案放進目標專案的 `.opencode/agents/`，或全域的 `~/.config/opencode/agents/`。各 agent 的模型由使用者自行指定，預設不設定研究時間或成本上限。

[完整使用與設定說明](./opencode/README.md) · [驗收案例](./opencode/ACCEPTANCE.md) · [目前驗證範圍](./opencode/VALIDATION.md)
