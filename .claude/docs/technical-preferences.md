# Technical Preferences

<!-- Populated by /setup-engine. Updated as the user makes decisions throughout development. -->
<!-- All agents reference this file for project-specific standards and conventions. -->

## Engine & Language

- **Engine**: Godot 4.6
- **Language**: GDScript (statically typed)
- **Rendering**: Compatibility (Web and mobile support)
- **Physics**: Jolt (Godot 4.6 default)

## Input & Platform

<!-- Written by /setup-engine. Read by /ux-design, /ux-review, /test-setup, /team-ui, and /dev-story -->
<!-- to scope interaction specs, test helpers, and implementation to the correct input methods. -->

- **Target Platforms**: PC, Web, Mobile (iOS / Android)
- **Input Methods**: Mouse/Keyboard, Touch
- **Primary Input**: Mouse Click (grid cell selection)
- **Gamepad Support**: Partial
- **Touch Support**: Full (required for mobile)
- **Platform Notes**: All UI must support both mouse click and touch. Hover-only interactions are forbidden. Board grid cells must have minimum touch target size of 44×44px.

## Naming Conventions

- **Classes**: PascalCase (e.g., `PlayerController`, `FortressBoard`)
- **Variables/Functions**: snake_case (e.g., `move_speed`, `get_cell_at()`)
- **Signals**: snake_case, past tense (e.g., `piece_moved`, `fortress_captured`)
- **Files**: snake_case matching class (e.g., `fortress_board.gd`)
- **Scenes**: PascalCase matching root node (e.g., `FortressBoard.tscn`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `BOARD_SIZE`, `MAX_PIECES`)

## Performance Budgets

- **Target Framerate**: 60 fps
- **Frame Budget**: 16.6 ms
- **Draw Calls**: ≤ 500 per frame
- **Memory Ceiling**: 512 MB

## Testing

- **Framework**: GUT (Godot Unit Test)
- **Minimum Coverage**: 70% for gameplay logic systems
- **Required Tests**: Board state logic, movement validation, win condition checks, piece rule calculations

## Forbidden Patterns

<!-- Add patterns that should never appear in this project's codebase -->
- [None configured yet — add as architectural decisions are made]

## Allowed Libraries / Addons

<!-- Add approved third-party dependencies here — only add when actively integrating, not speculatively -->
- GUT (Godot Unit Test) — test framework

## Architecture Decisions Log

<!-- Quick reference linking to full ADRs in docs/architecture/ -->
- [No ADRs yet — use /architecture-decision to create one]

## Engine Specialists

<!-- Written by /setup-engine when engine is configured. -->
<!-- Read by /code-review, /architecture-decision, /architecture-review, and team skills -->
<!-- to know which specialist to spawn for engine-specific validation. -->

- **Primary**: godot-specialist
- **Language/Code Specialist**: godot-gdscript-specialist (all .gd files)
- **Shader Specialist**: godot-shader-specialist (.gdshader files, VisualShader resources)
- **UI Specialist**: godot-specialist (no dedicated UI specialist — primary covers all UI)
- **Additional Specialists**: godot-gdextension-specialist (GDExtension / native C++ bindings only)
- **Routing Notes**: Invoke primary for architecture decisions, ADR validation, and cross-cutting code review. Invoke GDScript specialist for code quality, signal architecture, static typing enforcement, and GDScript idioms. Invoke shader specialist for material design and shader code. Invoke GDExtension specialist only when native extensions are involved.

### File Extension Routing

<!-- Skills use this table to select the right specialist per file type. -->

| File Extension / Type | Specialist to Spawn |
|-----------------------|---------------------|
| Game code (.gd files) | godot-gdscript-specialist |
| Shader / material files (.gdshader, VisualShader) | godot-shader-specialist |
| UI / screen files (Control nodes, CanvasLayer) | godot-specialist |
| Scene / prefab / level files (.tscn, .tres) | godot-specialist |
| Native extension / plugin files (.gdextension, C++) | godot-gdextension-specialist |
| General architecture review | godot-specialist |
