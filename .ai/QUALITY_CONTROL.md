# QUALITY_CONTROL.md — Master Quality Control Sub-Agent Protocol (`game-dev`)

> **Preset**: `game-dev`
> **QC Persona**: **AAA Game & Art Director + Principal Engine Architect** (`master-qc-director`)
> **Mandatory Rule**: Use **ONE** Master Quality Control Sub-Agent at the end of each development round. Never spawn multiple separate review sub-agents.
> **Pass Threshold**: Overall Average **`>= 8.5 / 10.0`** AND **every individual category `>= 8.5 / 10.0`**.

---

## 1. How to Invoke the Single Master QC Check

When a development round (feature, level, 3D asset batch, or gameplay loop) is complete:
- **Antigravity**: Define (if not yet defined in this conversation) and invoke a single sub-agent named `master-qc-director` (`EnableWriteTools: true`, `EnableMCPTools: true`) with the prompt: *"Read `.ai/QUALITY_CONTROL.md` and `.ai/SPEC.md`. Perform a single unified Master Quality Control audit of this round, inspect 3D assets/scenes via Blender MCP (`look` / `get_scene_info`) and run engine verification commands, write the scorecard to `.ai/QC_REPORT.md`, and return the verdict."*
- **Claude Code**: Invoke the `.claude/agents/master-qc-director.md` sub-agent.
- **Cursor / Windsurf / Copilot / Cline**: Switch into **Master QC Director Mode** and execute the 5-Pillar Zero-Trust Audit below before updating `.ai/QC_REPORT.md`.

---

## 2. The 5-Pillar Zero-Trust Deduction Rubric (Starts at `10.0` Per Category)

### Category 1: 3D Model, Art & Visual Direction (`>= 8.5 / 10`)
*Evaluates 3D meshes, silhouettes, proportions, topology, UV/materials, lighting, and 2D/UI visual cohesion.*
- **`-2.0 pts` (Automatic)**: If 3D models/scenes were created or modified and the auditor did NOT visually inspect them via **Blender MCP (`look` / `get_scene_info`)** or engine viewport screenshots.
- **`-1.5 pts`**: Unapplied scale/rotation transforms in Blender (`scale != 1.0, 1.0, 1.0`), wrong origin placement (e.g., character origin not at feet / `Z=0`), or inverted normals/axis (`+Z` vs `+Y` up mismatch).
- **`-1.5 pts`**: Primitive "programmer art" blockouts left unrefined when a finished model/material was expected (missing bevels, flat untextured default grey materials, poor silhouette readability).
- **`-1.0 pts`**: Missing collision helper meshes (`-col` / `-convcol`) or broken `.glb` import hierarchy in the game engine.

### Category 2: Gameplay Mechanics & Game Feel / "Juice" (`>= 8.5 / 10`)
*Evaluates player controls, camera feel, physics responsiveness, hit/action feedback, and core loop fun.*
- **`-2.0 pts`**: Frame-rate-dependent movement or physics (failing to multiply by `delta` / `dt` or using render ticks instead of fixed physics ticks).
- **`-1.5 pts`**: Stiff, unresponsive controls (no input buffering, abrupt camera snapping, dead/silent interactions with zero visual/audio/tween feedback).
- **`-1.0 pts`**: Broken collision layers/masks or edge-case clipping where the player/entities get stuck.

### Category 3: Code & ECS / Scene Architecture (`>= 8.5 / 10`)
*Evaluates script modularity, scene composition, signal/event decoupling, and engine best practices.*
- **`-2.0 pts`**: Using Godot 3.x deprecated syntax in a Godot 4.x project (or mismatched Bevy version APIs).
- **`-1.5 pts`**: "God object" scripts exceeding ~300 lines that mix input, physics, UI, and audio in one file instead of modular components/FSM/ECS plugins.
- **`-1.5 pts`**: Hardcoded brittle node paths (`get_node("../../../UI/HealthBar")`) instead of `@export` references, components, or `EventBus` signals.

### Category 4: Features & Scope Completeness (`>= 8.5 / 10`)
*Evaluates whether every promised mechanic, entity, UI state, and asset for the current round actually works.*
- **`-2.5 pts`**: Stubbed `TODO` / placeholder functions in core gameplay paths.
- **`-1.5 pts`**: Missing game state transitions (e.g., no way to start, pause, die/reset, or win the current loop).
- **`-1.0 pts`**: Missing or broken UI/HUD bindings to actual gameplay state.

### Category 5: Performance, Build & Asset Pipeline (`>= 8.5 / 10`)
*Evaluates headless engine compile checks, frame-time allocations, and Git LFS hygiene.*
- **`-3.0 pts` (Automatic)**: Engine headless check (`godot --headless --quit`, `cargo check`, `bun run build`) fails or emits unhandled script errors.
- **`-1.5 pts`**: Per-frame heap allocations, `get_node()` searches, or heavy node instantiations inside `_process()` / `update()` loops.
- **`-1.0 pts`**: Binary 3D/audio/texture assets added without `.gitattributes` Git LFS tracking rules.

---

## 3. Auto-Refinement Protocol (When Any Category Scores `< 8.5 / 10`)

1. The Master QC Sub-Agent sets `qc_status: REFINEMENT_REQUIRED` in [`.ai/QC_REPORT.md`](./QC_REPORT.md) and lists concrete, file-specific fixes under **Section 3 (Mandatory Refinements Required)**.
2. The main agent **must immediately execute those fixes** (e.g., refining the 3D mesh in Blender via Blender MCP, adding camera/movement polish, splitting monolithic scripts, or fixing physics ticks).
3. Re-invoke the **single `master-qc-director` sub-agent** to re-verify and update [`.ai/QC_REPORT.md`](./QC_REPORT.md) (up to **3 refinement rounds maximum**).
