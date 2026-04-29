# 入力ハンドリング (Input Handling)

> **Status**: Designed (pending review)
> **Author**: game-designer (lean mode)
> **Last Updated**: 2026-04-29
> **Last Verified**: 2026-04-29
> **Implements Pillar**: Pillar 3 (頭脳戦の純粋性) — 操作ミスが頭脳戦の純度を損なわない正確性
> **Layer**: Foundation · **Priority**: MVP

## Summary

入力ハンドリングは Mouse/Touch/Keyboard の物理入力を抽象化し、`cell_clicked`/`cell_drag_*`/`cancel`/`menu_toggle`/`camera_pan/zoom` などの **意味あるシグナル** を上位システムに発行する Foundation 基盤。Godot 4.6 のデュアルフォーカス仕様に対応し、ホバー専用 UI を禁じる方針を技術的に強制する（`cell_hovered` はタッチ環境では発行されない）。状態 FSM（Idle/PieceSelected/Dragging/MenuOpen/TutorialOverride）で入力ルーティングを制御し、メニュー中・相手ターン中・チュートリアル中の不適切な入力を本システム内でフィルタする。

> **Quick reference** — Layer: `Foundation` · Priority: `MVP` · Key deps: `None (depended on by #3, #10, #12, #13, #14, #15, #23)`

## Overview

入力ハンドリングは、プレイヤーからの全入力（マウス・タッチ・キーボード）を Godot のネイティブ入力イベントから抽象化し、ゲームロジック層が消費しやすい **意味のある入力アクション** に変換する基盤システム。物理デバイスの違い（マウス左クリック vs タッチタップ）を上位システムから隠蔽し、PC/Web/モバイル間での入力差を統一する役割を担う。

プレイヤーは直接このシステムを意識することはないが、ここで処理される全ての入力が「駒を選ぶ」「移動先を指定する」「メニューを開く」といった意味あるゲーム内行動に変換される。ホバー専用 UI を禁じる方針（タッチ非対応のため）と、`grab_focus()` がキーボードフォーカスのみに作用する Godot 4.6 のデュアルフォーカス仕様を考慮した設計とする。MVP では Mouse/Touch/Keyboard をサポート、Gamepad は対象外。

## Player Fantasy

このシステム自体にプレイヤーが意識的に関与する瞬間はなく、固有の Player Fantasy は存在しない。プレイヤーは入力ハンドリングを **存在に気づかないこと** によって正しく体験する — クリック/タップが常に「自分が押そうと思った場所」で「即座に」反応し、PC でもブラウザでもタッチでも同じ感覚で遊べることが達成されたとき、このシステムは成功している。

ただし上位システム（特に #3 移動&道生成、#13 UI/HUD）の Player Fantasy（「自分の意図が盤面に正確に反映される」）は本システムの精度に直接依存する。**入力の遅延・誤判定・反応漏れは Pillar 3「頭脳戦の純粋性」を直接損なう** ため、本システムは透明性と正確性を最優先する。

## Detailed Design

### Core Rules

入力ハンドリングは以下の **意味のあるアクション** をシグナルとして発行する。物理デバイス（マウス/タッチ/キーボード）の差異は本システム内で吸収する。

#### 入力アクション一覧

| Action | 物理入力（PC） | 物理入力（Web/Mobile） | 出力データ | 発行先 |
|---|---|---|---|---|
| `cell_clicked` | 左クリック | タップ | `Vector2i (col, row)` | #3 移動, #13 UI |
| `cell_hovered` | マウス移動 | (発行しない) | `Vector2i or null` | #13 UI（プレビュー表示） |
| `cell_drag_started` | 左ボタン押下 + 移動 | タッチ + 移動 | `Vector2i` 開始セル | #3 移動 |
| `cell_drag_released` | 左ボタン解放 | タッチ解放 | `Vector2i` 終了セル | #3 移動 |
| `cancel` | Esc / 右クリック | 二本指タップ | — | #3, #13, #15 |
| `confirm` | Enter / Space | (専用UIボタン経由) | — | #13 |
| `menu_toggle` | Esc (long) / メニューボタン | メニューボタン | — | #15 |
| `camera_pan` | 中ボタンドラッグ / 矢印キー | 二本指ドラッグ | `Vector2` 移動量 | #12 盤面レンダラ |
| `camera_zoom` | マウスホイール | ピンチ | `float` ズーム倍率 | #12 盤面レンダラ |

#### 入力ルール
1. **左クリック ≡ シングルタップ**：本システムはこれを `cell_clicked` として統一発行。受信側は物理デバイスを意識しない。
2. **ドラッグ開始判定**：ボタン/タッチ押下から **8 px 以上移動** したら drag。それ未満は click。
3. **`cell_hovered` は PC のみ**：タッチ環境では一切発行しない（ホバー専用 UI 禁止方針に対応）。受信側は hover が来ない前提で実装する。
4. **メニュー開放中は board アクションを発行しない**：本システム内部状態 `MenuOpen` でフィルタリング。
5. **チュートリアル中は限定的アクションのみ発行**：`TutorialOverride` 状態で、受信が許可されたアクション以外はブロック。

### States and Transitions

| State | 入場条件 | 退場条件 | 振る舞い |
|---|---|---|---|
| `Idle` | 初期状態、または PieceSelected → cancel | 駒タップ → PieceSelected | hover/click を board に発行 |
| `PieceSelected` | 自陣駒をタップ | 移動先タップ → MoveExecuted、別駒タップ → PieceSelected、cancel → Idle | drag 開始可能、有効移動先表示の通知 |
| `Dragging` | PieceSelected で 8px 以上移動 | リリース → MoveExecuted or Idle | drag_released 発行 |
| `MenuOpen` | menu_toggle | menu_toggle 再発行 / メニュー閉じる | board 入力ブロック |
| `TutorialOverride` | チュートリアル開始 | チュートリアル終了 | 許可アクション以外をブロック |

### Interactions with Other Systems

| System | 方向 | データ | 責務 |
|---|---|---|---|
| **#14 ゲームステート FSM** | ← 入力 | 現在のゲームフェーズ | 本システムは FSM 状態を読み、不適切な入力をフィルタ |
| **#3 移動 & 道生成** | → 出力 | `cell_clicked`, `cell_drag_*` | 移動要求の発行 |
| **#10 ターン管理** | ← 入力 | 自分のターン中か判定 | 相手ターンには board アクションを抑制 |
| **#12 盤面レンダラ** | → 出力 | `camera_pan/zoom` | カメラ制御 |
| **#13 UI / HUD** | ↔ 双方向 | `cell_clicked`, `cell_hovered`, `cancel` | UI クリックは UI 側で消費、未消費イベントが board に伝播 |
| **#15 メニュー** | ↔ 双方向 | `menu_toggle`, `cancel` | メニュー状態通知を本システムが受信し state 切替 |
| **#23 チュートリアル** | ↔ 双方向 | 許可アクションリスト | チュートリアルが本システム state を `TutorialOverride` にセット |

## Formulas

### Formula 1: スクリーン座標 → 盤面セル座標変換

`screen_to_cell(screen_pos, camera_pan, camera_zoom, cell_size, board_origin)` の定義は：

```
cell_x = floor((screen_pos.x - camera_pan.x - board_origin.x) / (cell_size * camera_zoom))
cell_y = floor((screen_pos.y - camera_pan.y - board_origin.y) / (cell_size * camera_zoom))
result = Vector2i(cell_x, cell_y) if (0 ≤ cell_x < board_width) and (0 ≤ cell_y < board_height) else null
```

**Variables:**
| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| screen_pos | `Vector2` | float×2 | (0,0)-(viewport_w, viewport_h) | クリック/タップのスクリーン座標 |
| camera_pan | `Vector2` | float×2 | -∞ to +∞ | カメラのパン量（px） |
| camera_zoom | `float` | float | 0.5 - 4.0 | ズーム倍率 |
| cell_size | `int` | int | 64 (固定) | 1セル基準ピクセル幅 |
| board_origin | `Vector2` | float×2 | 固定（盤面左上） | 盤面ワールド座標原点 |
| board_width / height | `int` | int | 9 or 24 | 盤面サイズ |

**Output Range**: `Vector2i(0..board_width-1, 0..board_height-1)` または `null`（盤外）
**Example**: 24×24 盤・cell_size=64・zoom=1.0・pan=(0,0)・origin=(0,0)・click=(640, 320) → cell=(10, 5)

### Formula 2: ドラッグ判定閾値

`is_drag(start_pos, current_pos, threshold) = distance(start_pos, current_pos) ≥ threshold`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| start_pos | `Vector2` | float×2 | スクリーン座標 | 押下開始位置 |
| current_pos | `Vector2` | float×2 | スクリーン座標 | 現在位置 |
| threshold | `int` | int | 8 (デフォルト、tunable 4-16) | ドラッグ判定px距離 |

**Output Range**: `bool`
**Example**: start=(100,100), current=(105,107), threshold=8 → distance=8.6 → drag=true

### Formula 3: ピンチズーム倍率変化

`new_zoom = clamp(current_zoom * (current_pinch_dist / start_pinch_dist), zoom_min, zoom_max)`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| current_zoom | `float` | float | 0.5-4.0 | 現在のズーム倍率 |
| start_pinch_dist | `float` | float | > 0 | ピンチ開始時の二指距離 |
| current_pinch_dist | `float` | float | > 0 | 現在の二指距離 |
| zoom_min / max | `float` | float | 0.5 / 4.0 | ズーム範囲 |

**Output Range**: `float` 0.5-4.0
**Example**: current_zoom=1.0, start_dist=200, current_dist=300 → new_zoom=1.5

### Formula 4: マウスホイールズーム倍率変化

`new_zoom = clamp(current_zoom * pow(zoom_step, wheel_delta), zoom_min, zoom_max)`

**Variables:**
| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| zoom_step | `float` | float | 1.1（デフォルト、tunable 1.05-1.2） | ホイール1ノッチあたりの倍率 |
| wheel_delta | `int` | int | -N to +N | ホイール回転量（+=拡大, -=縮小） |

**Output Range**: `float` 0.5-4.0
**Example**: current_zoom=1.0, zoom_step=1.1, wheel_delta=2 → new_zoom=1.21

## Edge Cases

- **If 入力が viewport 外で発生した（例：ウィンドウ端からのドラッグ抜け）**: `cell_drag_released` を最後の有効カーソル位置で発行し、状態を Idle に戻す。
- **If ウィンドウがフォーカス喪失した（タブ切替・Alt+Tab）**: 全 active state（PieceSelected, Dragging）を **強制 Idle に戻す**（途中ドラッグは破棄）。再フォーカス時は Idle から開始。
- **If 複数タッチが同時発生した（マルチタッチ）**: `Dragging` 状態でない限り **2本目以降のタッチは pinch zoom 入力としてのみ扱う**。3本以上は無視。
- **If 同フレーム内に click と drag 判定が両立した**: drag 判定（8px）を優先し click は破棄する。
- **If 駒タップ中にゲーム FSM がプレイヤー切替（hot-seat）した**: 強制 Idle に戻し、新プレイヤーの操作受付に切替。途中入力は無効。
- **If `screen_to_cell` が null を返した（盤外クリック）**: cancel として扱い、PieceSelected を解除。
- **If 入力デバイスがホットスワップされた（USB マウス抜き差し）**: Godot SDL3 が自動再認識。本システムは特別な処理なし。
- **If 同じ座標に複数のクリック対象（駒 + UI ボタン）がある**: UI を上層、Board を下層として **UI 側で event consumed なら board に発行しない**。Godot の `_unhandled_input` 経由でこれが自然に実現される。

## Dependencies

| Type | System | Interface |
|---|---|---|
| **Hard upstream** | （なし） | Foundation 層のため依存なし |
| **Hard downstream** | #3 移動&道生成 | `cell_clicked`, `cell_drag_started`, `cell_drag_released` を消費 |
| **Hard downstream** | #14 ゲームステート FSM | 本システムが FSM 状態を query して入力フィルタ判定 |
| **Hard downstream** | #10 ターン管理 | 自分のターンか query |
| **Hard downstream** | #12 盤面レンダラ | `camera_pan/zoom` 受信、座標変換のために pan/zoom を提供 |
| **Hard downstream** | #13 UI / HUD | `cell_clicked`, `cell_hovered`, `cancel`, UI クリック消費 |
| **Soft downstream** | #15 メニュー | `menu_toggle` 連携、メニュー状態通知 |
| **Soft downstream** | #23 チュートリアル | `TutorialOverride` state 制御権 |

**Provisional assumptions**（未設計依存先）：
- #3, #10, #12, #13, #14, #15, #23 全て未設計。インターフェース契約は本 GDD で先に定義し、各 GDD 設計時に再確認する。

## Tuning Knobs

| Knob | デフォルト | 範囲 | 影響 | 安全限界 |
|---|---|---|---|---|
| `drag_threshold_px` | 8 | 4-16 | ドラッグ判定の敏感さ | < 4 で誤判定多発、> 16 でドラッグ感が鈍い |
| `zoom_step` | 1.1 | 1.05-1.2 | ホイール1回の拡大量 | < 1.05 で遅い、> 1.2 で乱暴 |
| `zoom_min` | 0.5 | 0.25-0.75 | 最小ズーム（最大引き） | < 0.25 で 24×24 が小さすぎる |
| `zoom_max` | 4.0 | 2.0-8.0 | 最大ズーム（最大寄せ） | > 8.0 でぼやける |
| `cell_size_px` | 64 | 固定（変更不可） | セル基準サイズ | アセットと連動するため非変更 |
| `long_press_ms` | 600 | 400-1000 | (将来用) 長押しメニュー | 短いと誤発火 |
| `double_tap_window_ms` | 300 | 200-500 | (将来用) ダブルタップ | 短いと不便、長いと反応遅 |

## Visual/Audio Requirements

Foundation 層のため固有の Visual/Audio 要件はなし。ただし以下の **入力フィードバック** は本システムが発行するシグナルに対して上位システムが実装する責任：
- `cell_clicked` 受信時：上位 (#13 UI) で軽い SFX（クリック音）と視覚フィードバック
- `cell_drag_started` 受信時：上位 (#3, #13) でドラッグ視覚効果開始
- `cancel` 受信時：上位で軽い解除音と視覚効果解除

本 GDD では SFX 名・アセット仕様は規定しない。

## UI Requirements

本システムは固有 UI を持たない。ただし以下の **設定 UI 項目** が #16 設定システムから露出されること：
- `drag_threshold_px` スライダー（4-16）
- `zoom_step` スライダー（1.05-1.20）
- カメラ反転オプション（pan 方向逆転、好みで）

## Acceptance Criteria

- **GIVEN** プレイヤーが自分のターン中、状態 `Idle`、**WHEN** 自陣駒のセルをクリック/タップ、**THEN** `cell_clicked` シグナルが該当 `Vector2i(col, row)` で発行され、状態が `PieceSelected` に遷移する
- **GIVEN** 状態 `PieceSelected`、**WHEN** 8px 未満の小さなマウス移動の後ボタン解放、**THEN** drag ではなく click として扱い、`cell_clicked` を発行する
- **GIVEN** 状態 `PieceSelected`、**WHEN** 8px 以上ドラッグ後解放、**THEN** `cell_drag_started`（押下時）+ `cell_drag_released`（解放時）が発行され、`cell_clicked` は発行しない
- **GIVEN** 状態 `MenuOpen`、**WHEN** 盤面をクリック、**THEN** `cell_clicked` は発行されない（フィルタ）
- **GIVEN** 相手プレイヤーのターン中、**WHEN** 盤面をクリック、**THEN** `cell_clicked` は発行されない
- **GIVEN** タッチデバイス、**WHEN** 任意の操作、**THEN** `cell_hovered` シグナルは一切発行されない
- **GIVEN** 24×24 盤面・zoom=1.0・pan=(0,0)、**WHEN** スクリーン (640, 320) をクリック、**THEN** `screen_to_cell` は `Vector2i(10, 5)` を返す
- **GIVEN** スクリーン外（盤面外座標）をクリック、**WHEN** `screen_to_cell` 実行、**THEN** `null` を返し cancel として処理される
- **GIVEN** 状態 `Dragging`、**WHEN** ウィンドウがフォーカス喪失、**THEN** 強制 `Idle` に戻り途中ドラッグは破棄される
- **GIVEN** マウスホイール+1ノッチ・zoom=1.0・zoom_step=1.1、**WHEN** ホイールイベント受信、**THEN** new_zoom=1.1 で `camera_zoom` 発行
- **パフォーマンス**: 入力イベント受信から対応シグナル発行まで 1 フレーム以内（16.6ms 以下）
- **GIVEN** 同時に UI ボタンと board セルが重なるクリック、**WHEN** UI ボタンが上層、**THEN** UI ボタンのみ反応し `cell_clicked` は発行されない

## Open Questions

- **Q1**: Tier 1 でゲームパッド対応する場合、どのアクションマッピングにするか？ → owner: game-designer, target: Tier 1 設計時
- **Q2**: アンドゥ/待った機能を MVP に入れる場合、`undo` アクションを本システムが発行するか別システム経由か？ → owner: game-designer, target: 次回プロトタイプフィードバック後
- **Q3**: アクセシビリティ（キーボードのみで全操作可能にする）の優先度。MVP では Mouse/Touch 中心、キーボードはショートカットのみ。Tier 1+ で再検討。 → owner: ux-designer
