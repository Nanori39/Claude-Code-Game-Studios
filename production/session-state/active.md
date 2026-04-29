# Active Session State

*Last updated: 2026-04-29*

## Current Task
First MVP GDD complete: 入力ハンドリング (#11). Ready for design review or next system.

## Status
- [x] /start (Path C: 既存コンセプトの正式化)
- [x] /brainstorm — game-concept.md 作成
- [x] /setup-engine — Godot 4.6 / GDScript 設定
- [x] /art-bible — Core sections 1-4 完了
- [x] /map-systems — 24 システム特定、優先度割当、systems-index.md 作成
- [x] /design-system 入力ハンドリング — input-handling.md 作成 (Designed, pending review)

## Files Created This Project
- `production/review-mode.txt` — `lean`
- `design/gdd/game-concept.md` — 砦戦略ボードゲーム コンセプト
- `design/art/art-bible.md` — Visual Identity / Mood / Shape / Color (Core 1-4)
- `design/gdd/systems-index.md` — 24 システム索引、依存関係、優先度
- `CLAUDE.md` — Godot 4.6 / GDScript 設定済み
- `.claude/docs/technical-preferences.md` — 言語・入力・命名・パフォーマンス・テスト・ルーティング

## Key Decisions
- レビューモード: lean（director gates は /gate-check のみ）
- エンジン: Godot 4.6 / GDScript
- プラットフォーム: PC (Steam) + Web、Mobile はポストローンチ
- MVP: 17 システム、24×24 デフォルト + 9×9 クイックモード、hot-seat PvP
- 駒は MVP では 3-4 種に絞る（狼・熊・人 + 1種）
- 崩壊フェーズ・トラップ・鯨・AI は Vertical Slice (Tier 1) に後回し

## Next Recommended Action
`/design-system 入力ハンドリング` から MVP システムの GDD 作成開始
（または `/map-systems next` で順次自動選択）

## Open Questions
- HIGH-RISK システム（マップ生成、移動&道生成、補給線）は GDD 後すぐにプロトタイプ検証必要
- 24×24 でのパフォーマンス（補給線判定）は技術プロトタイプで早期測定すべき
