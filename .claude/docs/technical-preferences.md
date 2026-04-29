# Technical Preferences

<!-- Populated by /setup-engine. Updated as the user makes decisions throughout development. -->
<!-- All agents reference this file for project-specific standards and conventions. -->

## Engine & Language

- **Engine**: Godot 4.6
- **Language**: GDScript
- **Rendering**: Godot Forward+ renderer (2D)
- **Physics**: Godot 2D physics（Jolt は3D用のため未使用）

## Input & Platform

- **Target Platforms**: PC (Steam), Web (Browser); Mobile post-launch
- **Input Methods**: Mouse/Keyboard, Touch (Web/Mobile)
- **Primary Input**: Mouse（駒のクリック・ドラッグ操作が中心）
- **Gamepad Support**: None（戦略ボードゲームのため不要）
- **Touch Support**: Full（Web版・将来のモバイル版で必須）
- **Platform Notes**: ホバー専用 UI は禁止（タッチ非対応のため）。全インタラクションはクリック/タップで完結。盤面のズーム・パンはマウスホイールとピンチの両方で動作させる。

## Naming Conventions

- **Classes**: PascalCase (e.g., `BoardCell`, `RoadNetwork`)
- **Variables**: snake_case (e.g., `current_supply`, `road_count`)
- **Signals/Events**: snake_case past tense (e.g., `road_created`, `fortress_collapsed`)
- **Files**: snake_case matching class (e.g., `board_cell.gd`, `road_network.gd`)
- **Scenes/Prefabs**: PascalCase matching root node (e.g., `BoardCell.tscn`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `MAX_BOARD_SIZE`, `COLLAPSE_TURN_COUNT`)

## Performance Budgets

- **Target Framerate**: 60 fps
- **Frame Budget**: 16.6 ms
- **Draw Calls**: 200 maximum per frame（2D ボードゲームとして十分余裕）
- **Memory Ceiling**: 500 MB（Web版を含むため抑制）

## Testing

- **Framework**: GUT (Godot Unit Test) — 導入は `/test-setup` で実施
- **Minimum Coverage**: 80%（バランス計算式・補給線判定・道生成ロジック）
- **Required Tests**: 補給線判定、道生成、陣地計算、勝利条件判定、マップ生成の公平性検証

## Forbidden Patterns

- [None configured yet — add as architectural decisions are made]

## Allowed Libraries / Addons

- [None configured yet — add as dependencies are approved]

## Architecture Decisions Log

- [No ADRs yet — use /architecture-decision to create one]

## Engine Specialists

- **Primary**: godot-specialist
- **Language/Code Specialist**: godot-gdscript-specialist (.gd ファイル)
- **Shader Specialist**: godot-shader-specialist (.gdshader, VisualShader)
- **UI Specialist**: godot-specialist（専任なし — primary が UI も担当）
- **Additional Specialists**: godot-gdextension-specialist（GDExtension/ネイティブC++ 必要時のみ）
- **Routing Notes**: アーキテクチャ判断・ADR検証・横断的レビューは primary。コード品質・signal設計・静的型付け・GDScriptイディオムは GDScript specialist。マテリアル/シェーダーは shader specialist。GDExtension は native 拡張時のみ。

### File Extension Routing

| File Extension / Type | Specialist to Spawn |
|-----------------------|---------------------|
| Game code (.gd files) | godot-gdscript-specialist |
| Shader / material files (.gdshader, VisualShader) | godot-shader-specialist |
| UI / screen files (Control nodes, CanvasLayer) | godot-specialist |
| Scene / prefab / level files (.tscn, .tres) | godot-specialist |
| Native extension / plugin files (.gdextension, C++) | godot-gdextension-specialist |
| General architecture review | godot-specialist |
