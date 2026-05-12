# Archive workflow handoff (fw v2.3-draft)

我在另一個 chat 建立了 conversation archive 規範,這個 chat 也會走那套
流程。Spec source 在 fw repo `tools/conversation-archive-naming.md`。

## Inline markers(三種,都必須單獨成行 / line-anchored)

### 1. `[ChatBriefing] ... [/ChatBriefing]` — 給未來 CA 的 briefing

Block 形式,內含 YAML frontmatter(無 `---` fence),schema:

```
[ChatBriefing]
date: YYYY-MM-DD
products: [<product-code>]      # e.g. [fw] / [tw] / [csc]
submodules: []                   # 通常空,或如 [m3] / [m4-4]
topics:
  - <topic-slug>
  - <topic-slug>
related-archives:
  - YYYY-MM-DD-<slug>            # 相關歷史 archive 檔名(去 .md)
ca-briefing: |
  主題與目的
  <2-4 句:這個 chat 做什麼、為什麼>

  關鍵決策
  <分層列出,標代號方便 cross-ref,例如 D-alpha / D-beta>

  未解 / 下個 chat 待辦
  <優先序明確,P0 / P1 / P2 / P3>

  給未來 CA 的脈絡
  <必讀文件 + 關鍵心智模型 + invariant>
[/ChatBriefing]
```

### 2. `[ArchiveReady]` — 強制歸檔(覆蓋 7-day buffer)

單獨成行,後可接說明文字。例:

```
[ArchiveReady]
```

或

```
[ArchiveReady] deliverable shipped, no pending threads
```

### 3. `[ArchivePending]` — 強制跳過(還在進行中)

單獨成行,後可接說明文字。例:

```
[ArchivePending] soak in progress
```

## 規則

- Marker 必須**單獨成行**,prose 中提及不觸發(例如「我們用 `[ArchiveReady]`
  marker」這種寫法不算)
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