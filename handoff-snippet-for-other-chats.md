---
doc: handoff-snippet-for-other-chats
purpose: Verbatim snippet for Jason to paste into existing chats that predate v2.3 archive workflow. Communicates marker semantics to chat-internal CA without requiring full spec dump.
audience: Jason (operator)
created: 2026-05-12
updated: 2026-05-12
version: v1.1
changelog:
  - v1.1 (2026-05-12) — add [Handoff] sub-block to ChatBriefing schema for handoff-to-next-chat 精華
  - v1.0 (2026-05-12) — initial version
related:
  - tools/conversation-archive-naming.md (v2.3-draft) — canonical spec source
  - tools/chat-handoff-workflow.md (v1.1-draft) — overall handoff flow
  - tools/monthly-archive_v3_README.md (v3.0) — operator usage doc
  - tools/external-init-mechanism.md (v1.0) — distribution mechanism
mirror-status: synced to fw-init-config on each version bump
---

# Handoff snippet for other chats

## Purpose

當 Jason 想讓某個 existing chat(predates v2.3,或未引用 archive 規範)
走 v2.3 archive 流程時,複製本檔下方 §Snippet 段落貼到目標 chat 即可。
Chat-internal CA 看完即可配合,不需追問細節。

## When to paste

預設**不貼**。Marker 由 Jason 自己加(per spec §Retroactive marker
adoption),chat-internal CA 完全無感即可走流程。

需要貼的時機:

- Chat-internal CA 要幫忙寫 `[ChatBriefing]` 內容
- Chat-internal CA 要判斷 ready / pending 並建議
- Chat-internal CA 提到 archive 相關事但格式不對,需要校準

## Maintenance

本 snippet 是 spec 的 derived artifact,**spec 改版時要同步本檔**:

- Marker 名稱 / 格式變更 → 改 §Snippet
- Readiness gate 規則變更 → 改 §Snippet
- ChatBriefing schema 變更 → 改 §Snippet 內的 YAML 範例

每次改版同步把 frontmatter `version` bump、related cross-ref 對齊。
版本改動後 sync 到 fw-init-config(per `external-init-mechanism.md`
§Sync workflow)。

## Source authority

本檔**不是** spec source。Spec source 在
`tools/conversation-archive-naming.md` v2.3-draft。本 snippet 若與
spec 衝突,以 spec 為準,並開 issue 修本檔。

---

## Snippet

從下方 fenced block **內部**(不含 fence 本身)整段複製貼上到目標 chat。
為避免 nested fence 衝突,本檔用 5 個 backtick 包外層 fence。

`````
# Archive workflow handoff (fw v2.3-draft)

我在另一個 chat 建立了 conversation archive 規範,這個 chat 也會走那套
流程。Spec source 在 fw repo `tools/conversation-archive-naming.md`。

## Inline markers(三種,都必須單獨成行 / line-anchored)

### 1. `[ChatBriefing] ... [/ChatBriefing]` — 給未來 CA 的 briefing

Block 形式,內含 YAML(無 `---` fence)。Schema:

````
[ChatBriefing]
date: YYYY-MM-DD
products: [<product-code>]       # e.g. [fw] / [tw] / [csc]
submodules: []                    # 通常空,或如 [m3] / [m4-4]
topics:
  - <topic-slug>
  - <topic-slug>
related-archives:
  - YYYY-MM-DD-<slug>             # 相關歷史 archive 檔名(去 .md)
ca-briefing: |
  主題與目的
  <2-4 句:這個 chat 做什麼、為什麼>

  關鍵決策
  <分層列出,標代號方便 cross-ref,例如 D-alpha / D-beta>

  未解 / 下個 chat 待辦
  <優先序明確,P0 / P1 / P2 / P3>

  給未來 CA 的脈絡
  <必讀文件 + 關鍵心智模型 + invariant>

  [Handoff]
  你接手的工作
  <2-4 條,從 P0 / P1 抽,動詞起頭>

  Context 摘要
  <2-4 句 prose,從主題 + 決策結論抽,不含推導>

  必讀
  <檔案清單,bullet 即可>

  關鍵 invariant
  <1-3 條,違反會走樣的鐵律>
  [/Handoff]
[/ChatBriefing]
````

**`[Handoff]` 子區塊用途**:給 next chat CA 用的精華濃縮。Jason 開 new
chat 後**只貼這段**內容,不貼整個 ca-briefing。其他 section(主題與目的
/ 關鍵決策 / 未解待辦 / 給未來 CA 的脈絡)保留完整 audit 視角。

CA 寫 `[Handoff]` 時:精煉、無推導、目標 size 為完整 ca-briefing 的
1/3 ~ 1/2。內部欄位順序固定:你接手的工作 → Context 摘要 → 必讀 →
關鍵 invariant。

`[Handoff]` / `[/Handoff]` 都單獨成行(視覺定位明確,Jason 複製範圍
清楚)。不要在 `[Handoff]` 內加 markdown header 或 YAML sub-keys,
維持中文小標 + prose 風格。

### 2. `[ArchiveReady]` — 強制歸檔(覆蓋 7-day buffer)

單獨成行,後可接說明文字。例:

````
[ArchiveReady]
````

或

````
[ArchiveReady] deliverable shipped, no pending threads
````

### 3. `[ArchivePending]` — 強制跳過(還在進行中)

單獨成行,後可接說明文字。例:

````
[ArchivePending] soak in progress
````

## 規則

- Marker 必須**單獨成行**,prose 中提及不觸發(例如「我們用
  `[ArchiveReady]` marker」這種寫法不算)
- 多個 marker 取**最後一個**(last-marker-wins,支援狀態轉移如
  pending → ready)
- 不放 marker → 走 implicit readiness:`updated_at` 過 7 天 buffer 自動 ready
- ChatBriefing block 內**不要**加 `---` YAML fence

## 你在這個 chat 該做什麼

預設:不用做任何事,marker 由 Jason 自己加。

主動寫 `[ChatBriefing]` 的時機(無需 Jason 明確要求):

- Chat 進入自然收尾(deliverable 完成、決策收斂)
- Jason 說「準備歸檔 / 收尾 / handoff」等訊號詞
- Context window 接近上限,需要 fresh chat 接手

判斷 ready / pending:依據對話狀態給建議,Jason 決定是否加 marker。

不要做:

- 主動提 archive 流程,除非上面 trigger 條件達成
- 把 marker 寫進 code block 或 prose 中段(會破壞 line-anchored 解析)
- 在 ChatBriefing block 內加 `---` fence
- 在 `[Handoff]` 內加 markdown header 或 YAML sub-keys
`````
