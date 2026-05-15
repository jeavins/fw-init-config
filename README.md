# fw-init-config

Claude framework 的 distribution repo。存放透過短指令載入 Claude chat 的規範文件。

## 檔案

| 檔案 | 版本 | 載入指令 | 用途 |
|------|------|---------|------|
| `handoff-snippet-for-other-chats.md` | v1.1 | `/fw-init` | Base marker schema — ChatBriefing block、ArchiveReady/ArchivePending markers、預設 CA 行為 |
| `ca-advisor-mode.md` | v1.0-draft | `/fw-advisor` | Advisor 行為總綱 — briefing 寫作 guidelines、information flow discipline、reconnaissance discipline |

## 使用方式

新 chat 開始時,貼入對應短指令載入規範:

- `/fw-init` — 每個新 chat 的第一動作(default)
- `/fw-advisor` — opt-in,當 chat 需要主動寫 briefing / 判斷衝突 / 跨環境 ship 時使用

兩個指令不互斥;`/fw-advisor` 是 `/fw-init` 的 superset。

## Mirror 規則

本 repo 的文件與 `claude-dev-framework/tools/` 保持同步,每次 fw repo 版本號 bump 時更新。
