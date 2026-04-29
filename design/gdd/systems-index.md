# Systems Index: 砦戦略ボードゲーム（仮題）

> **Status**: Draft
> **Created**: 2026-04-29
> **Last Updated**: 2026-04-29
> **Source Concept**: design/gdd/game-concept.md

---

## Overview

ターン制 PvP 戦略ボードゲーム「砦戦略ボードゲーム」のシステム分解。コアループ「駒移動 → 道生成 → 陣地化 → 補給線判定 → 砦勝利」を成立させるために必要な全 24 システムを 5 層に分類した。

ピラー「道は意図を語る」「シンプルなルール、深い帰結」「頭脳戦の純粋性」「包囲は罠である」を満たすため、MVP は **24×24 をデフォルト、9×9 をクイックモード** とする hot-seat 対戦版を 17 システムで構築。Tier 1 で崩壊フェーズ・トラップ・鯨・全駒・AI を追加、Tier 2 でオンライン PvP を実装。

---

## Systems Enumeration

| # | System Name | Category | Priority | Status | Design Doc | Depends On |
|---|---|---|---|---|---|---|
| 1 | マップ生成 | Core | MVP | Not Started | — | #24 ボード幾何 |
| 2 | 駒システム | Gameplay | MVP | Not Started | — | — |
| 3 | 移動 & 道生成 🔴 | Gameplay | MVP | Not Started | — | #2, #1, #11, #10, #24 |
| 4 | 陣地化 | Gameplay | MVP | Not Started | — | #3, #24 |
| 5 | 補給線 🔴 | Gameplay | MVP | Not Started | — | #3, #1, #2, #24 |
| 6 | 砦崩壊フェーズ | Gameplay | Vertical Slice | Not Started | — | #5, #10 |
| 7 | トラップ | Gameplay | Vertical Slice | Not Started | — | #5, #2 |
| 8 | 鯨ユニット | Gameplay | Vertical Slice | Not Started | — | #2, #1, #3 |
| 9 | 勝利条件判定 | Gameplay | MVP | Not Started | — | #2, #1, #3 |
| 10 | ターン管理 | Core | MVP | Not Started | — | #14 |
| 11 | 入力ハンドリング | Core | MVP | Designed | [input-handling.md](input-handling.md) | — |
| 12 | 盤面レンダラ（ズーム/パン込） | UI | MVP | Not Started | — | #1, #17 |
| 13 | UI / HUD | UI | MVP | Not Started | — | #10, #5, #2 |
| 14 | ゲームステート FSM | Core | MVP | Not Started | — | — |
| 15 | メニュー / 画面遷移（最小） | UI | MVP | Not Started | — | #14, #16 |
| 16 | 設定（音量・色覚モード） | Persistence | MVP | Not Started | — | — |
| 17 | アセット管理 | Core | MVP | Not Started | — | — |
| 18 | オーディオ（最小） | Audio | MVP | Not Started | — | #17 |
| 19 | セーブ/ロード | Persistence | Vertical Slice | Not Started | — | 全 gameplay state |
| 20 | ローカル AI | Gameplay | Vertical Slice | Not Started | — | 全 gameplay |
| 21 | ネットコード | Core | Full Vision | Not Started | — | 全 gameplay state, #14 |
| 22 | マッチメイキング・認証 | Meta | Full Vision | Not Started | — | #21 |
| 23 | チュートリアル / オンボーディング | Meta | MVP | Not Started | — | 全主要 gameplay + #13 |
| 24 | ボード幾何（隣接・経路探索ヘルパー） | Core | MVP | Not Started | — | — |

🔴 = HIGH-RISK ボトルネックシステム（早期プロトタイプ推奨）

---

## Categories

| Category | Description |
|---|---|
| **Core** | Foundation systems everything depends on |
| **Gameplay** | The systems that make the game fun |
| **UI** | Player-facing information displays |
| **Audio** | Sound and music systems |
| **Persistence** | Save state and continuity |
| **Meta** | Systems outside the core game loop |

---

## Priority Tiers

| Tier | Definition | Target Milestone | Design Urgency |
|---|---|---|---|
| **MVP** | コアループ「道生成・補給線・砦勝利」を 24×24 + 9×9 hot-seat で成立させるために必須 | 2-3 ヶ月（最初のプレイ可能版） | Design FIRST |
| **Vertical Slice** | 崩壊フェーズ・トラップ・鯨を加えて完全な体験を提供 | +1-2 ヶ月 | Design SECOND |
| **Alpha** | 全駒・全盤面・地形拡張による完成版 gameplay | +2-3 ヶ月 | Design THIRD |
| **Full Vision** | オンライン PvP・モバイル・ランクマッチ | Post-launch | Design as needed |

---

## Dependency Map

### Foundation Layer（依存なし）
1. **#11 入力ハンドリング** — 生入力ラップ、エンジン非依存抽象
2. **#14 ゲームステート FSM** — 純粋状態マシン
3. **#16 設定システム** — 音量・色覚モード等のローカル設定
4. **#17 アセット管理** — 画像・音・データロード
5. **#18 オーディオ** — 基本再生（#17 のみに依存）
6. **#24 ボード幾何** — 隣接判定・経路探索ヘルパー（純粋ロジック）

### Core Layer（Foundation のみに依存）
1. **#1 マップ生成** — 地形・資源配置・公平性（24×24 + 9×9 対応必須）— depends on: #24
2. **#2 駒システム** — データ定義（能力・特性）
3. **#10 ターン管理** — depends on: #14
4. **#12 盤面レンダラ** — ズーム/パン UI 込 — depends on: #1, #17

### Feature Layer（Core に依存）
1. **#3 移動 & 道生成** 🔴 — depends on: #2, #1, #11, #10, #24
2. **#4 陣地化** — depends on: #3, #24
3. **#5 補給線** 🔴 — depends on: #3, #1, #2, #24
4. **#9 勝利条件判定** — depends on: #2, #1, #3
5. **#6 砦崩壊フェーズ** — depends on: #5, #10
6. **#7 トラップ** — depends on: #5, #2
7. **#8 鯨ユニット** — depends on: #2, #1, #3

### Presentation Layer（Feature に依存）
1. **#13 UI / HUD** — depends on: #10, #5, #2
2. **#15 メニュー / 画面遷移** — depends on: #14, #16
3. **#23 チュートリアル** — depends on: 全主要 gameplay + #13

### Polish Layer（Polish / Meta）
1. **#19 セーブ/ロード** — depends on: 全 gameplay state
2. **#20 ローカル AI** — depends on: 全 gameplay
3. **#21 ネットコード** — depends on: 全 gameplay state, #14
4. **#22 マッチメイキング・認証** — depends on: #21

---

## Recommended Design Order

| Order | System | Priority | Layer | Agent(s) | Est. Effort |
|---|---|---|---|---|---|
| 1 | #11 入力ハンドリング | MVP | Foundation | game-designer | S |
| 2 | #14 ゲームステート FSM | MVP | Foundation | game-designer + godot-specialist | S |
| 3 | #16 設定システム | MVP | Foundation | game-designer | S |
| 4 | #17 アセット管理 | MVP | Foundation | godot-specialist | S |
| 5 | #18 オーディオ（最小） | MVP | Foundation | sound-designer | S |
| 6 | #24 ボード幾何 | MVP | Foundation | game-designer + godot-gdscript-specialist | M |
| 7 | **#1 マップ生成** 🔴 | MVP | Core | game-designer + systems-designer | **L** |
| 8 | #2 駒システム | MVP | Core | game-designer + systems-designer | M |
| 9 | #10 ターン管理 | MVP | Core | game-designer | S |
| 10 | #12 盤面レンダラ | MVP | Core | godot-specialist + ux-designer | M |
| 11 | **#3 移動 & 道生成** 🔴 | MVP | Feature | game-designer + systems-designer | **L** |
| 12 | #4 陣地化 | MVP | Feature | systems-designer | M |
| 13 | **#5 補給線** 🔴 | MVP | Feature | systems-designer | **L** |
| 14 | #9 勝利条件判定 | MVP | Feature | game-designer | S |
| 15 | #13 UI / HUD | MVP | Presentation | ux-designer + ui-programmer | M |
| 16 | #15 メニュー（最小） | MVP | Presentation | ux-designer | S |
| 17 | #23 チュートリアル | MVP | Presentation | ux-designer + game-designer | M |
| 18 | #6 砦崩壊フェーズ | Vertical Slice | Feature | systems-designer | M |
| 19 | #7 トラップ | Vertical Slice | Feature | game-designer | S |
| 20 | #8 鯨ユニット | Vertical Slice | Feature | game-designer | S |
| 21 | #19 セーブ/ロード | Vertical Slice | Polish | godot-specialist | M |
| 22 | #20 ローカル AI | Vertical Slice | Polish | ai-programmer + game-designer | L |
| 23 | #21 ネットコード | Full Vision | Polish | network-programmer | L |
| 24 | #22 マッチメイキング・認証 | Full Vision | Polish | network-programmer + security-engineer | L |

> **Effort scale**: S = 1 セッション, M = 2-3 セッション, L = 4+ セッション

> **並行設計可能**: Order 1-5 は完全に独立（Foundation の中でも依存なし）。Order 7-9 や 11/12 など同層で依存しないものは並行設計可能。

---

## Circular Dependencies

なし（24 システム全てで循環なし）。

---

## High-Risk Systems

| System | Risk Type | Risk Description | Mitigation |
|---|---|---|---|
| **#1 マップ生成** | Technical + Design | 24×24 で公平性アルゴリズムを保証するのが難しい。両プレイヤーへの公平距離・地形配置のバランス検証が困難 | Order 7 で `/prototype` を回す前に、まず手動で 5-10 マップ生成して公平性チェック |
| **#3 移動 & 道生成** | Design | ボトルネック。下流(#4, #5, #9, #8) が依存。道の表現（タイルオーバーレイ）と上書きルールが下流の UX を決定する | Order 11 の前に game-concept.md の Visual Identity Anchor と art-bible §3.3 を再確認 |
| **#5 補給線** | Technical + Design | ボトルネック。24×24 で経路再計算が毎ターン発生、パフォーマンス懸念。崩壊フェーズ(Tier 1)の土台 | キャッシュ戦略を ADR で記録。`/prototype core-loop` で 24×24 の負荷を測定 |
| **#23 チュートリアル** | Design | 複雑ルール（道・補給・陣地・砦）を 10 分で伝える設計が難しい。チュートリアルでは 9×9 を使用 | Order 17 で ux-designer に委ね、複数オンボーディング案を比較 |
| **#1 マップ生成（パフォーマンス）** | Technical | 24×24 マップで補給線判定 7 倍負荷 | 早期に技術プロトタイプ。`technical-preferences.md` パフォーマンスバジェット再確認 |

---

## Progress Tracker

| Metric | Count |
|---|---|
| Total systems identified | 24 |
| Design docs started | 1 |
| Design docs reviewed | 0 |
| Design docs approved | 0 |
| MVP systems designed | 1 / 17 |
| Vertical Slice systems designed | 0 / 5 |
| Full Vision systems designed | 0 / 2 |

---

## Next Steps

- [ ] このシステム索引を承認
- [ ] `/design-system 入力ハンドリング` から MVP システムの GDD 作成開始
- [ ] または `/map-systems next` で次の未設計システムを自動選択
- [ ] 各 GDD 完成後 `/design-review design/gdd/[system].md` を実行
- [ ] HIGH-RISK システム（#1, #3, #5）は GDD 完成後すぐ `/prototype` で技術検証
- [ ] MVP 17 システム全て設計完了後 `/review-all-gdds` → `/gate-check pre-production`
