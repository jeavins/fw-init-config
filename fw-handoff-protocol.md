---
doc: fw-handoff-protocol
purpose: /fw-handoff short-trigger 的行為規範。CA fetch 本檔後進入 [ChatBriefing] 產出 binding 狀態。
audience: CA(本 chat 跟未來 chat 的 Claude)
created: 2026-05-15
updated: 2026-05-15
version: v1.0
related:
  - tools/handoff-snippet-for-other-chats.md (v1.1) — marker schema canonical
  - tools/conversation-archive-naming.md (v2.3-draft) — archive spec canonical
  - tools/chat-handoff-workflow.md (v2.0) — lifecycle 整體流程
mirror-status: synced to fw-init-config on each version bump
---

# /fw-handoff protocol

## Purpose

`/fw-handoff` 是 Jason 觸發「準備產 [ChatBriefing] / 進入歸檔流程」的
short-trigger。Fetch 本檔 = CA 進入 binding 狀態,行為受本檔規範。

設計動機:fetch 進 context 不等於 binding,中間隔一層「寫 deliverable
時要不要回查 spec」的紀律。本檔強制 readback,把紀律從內隱期待變顯式 token
產出。

## 啟動行為

Fetch 本檔成功後,**不可只 acknowledge**。CA 必須依序:

1. Inline 列出 §Mandatory readback 三條(完整文字,不可省略 / 不可摘要)
2. Readback 完才產 [ChatBriefing] block

跳過 readback 直接寫 briefing → 違規。Jason 一眼看 acknowledgement 有沒有
readback 三條,沒有立即 push back。

## Mandatory readback

每次 /fw-handoff 觸發 → CA inline 寫出下列三條:

### 1. Marker fence 規則

`[ChatBriefing]` / `[/ChatBriefing]` 必須單獨成行(line-anchored),不可嵌入
prose 或 code block 內部。`[Handoff]` / `[/Handoff]` 子區塊亦同。

### 2. [Handoff] 子區塊必含

`[ChatBriefing]` 內必含 `[Handoff]` 子區塊。子區塊欄位順序固定:

你接手的工作 → Context 摘要 → 必讀 → 關鍵 invariant

### 3. ca-briefing 四 section 順序

`ca-briefing` 內四 section 順序固定:

主題與目的 → 關鍵決策 → 未解 / 下個 chat 待辦 → 給未來 CA 的脈絡

`[Handoff]` 子區塊嵌於 ca-briefing 末尾。

## Revise 行為

Jason 對 briefing 提出修改後,**CA 不得**:

- 引用「上一輪 fetch 結果」省略 readback
- 假設「剛 fetch 過,還記得」就跳過

每次 Jason 重打 /fw-handoff → CA 重新 fetch 本檔 + 重新 inline readback +
重產 briefing。

理由:revise 階段 long-context drift 風險高於 first draft。Spec 必須 fresh,
不能延續上一輪記憶。

## 不在本檔的範疇

本檔**只規範 /fw-handoff trigger 行為**,不重複 marker schema 定義:

- `[ChatBriefing]` schema 完整內容 → handoff-snippet-for-other-chats.md §Snippet
- Marker semantics(ArchiveReady / ArchivePending / last-marker-wins)→ conversation-archive-naming.md
- Chat lifecycle 整體流程 → chat-handoff-workflow.md

## Changelog

- v1.0 (2026-05-15) — initial。設計動機:violating-CA case 揭示 fetch ≠ binding,需 readback 強制。
