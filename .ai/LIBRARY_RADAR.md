# LIBRARY_RADAR.md — 2D & 3D Game Development Engine & Library Guide

> **Preset**: `game-dev`
> **Rule for AI Agents**: Recommend engines and libraries from the **Approved Radar** below based on whether the game is 2D, 3D, desktop/console, or web-first. For **any 3D modeling or 3D asset generation**, always check and suggest **Blender MCP** via [`.ai/MCP_AND_TOOLS.md`](./MCP_AND_TOOLS.md).

---

## 1. Engine & Stack Decision Tree

| Game Type & Goal | Recommended Engine / Stack | Why It Excels for AI Vibe Coding | Matching Recipe |
| :--- | :--- | :--- | :--- |
| **General 2D or 3D Indie Game (Fastest Iteration + Lightweight Text Scenes)** | **Godot 4.3+** (GDScript or C#) | `.tscn` and `.tres` files are human- & AI-readable plain text; CLI headless mode verifies scripts instantly | [`.ai/recipes/godot-4-2d-3d-architecture.md`](./recipes/godot-4-2d-3d-architecture.md) |
| **Code-First Systems / Simulation / ECS Game in Rust** | **Bevy 0.15+** (Rust ECS) or **Macroquad / Raylib** | 100% code-driven (zero GUI editor clicks needed); strong compiler guarantees | [`.ai/recipes/rust-bevy-ecs-fast-compile.md`](./recipes/rust-bevy-ecs-fast-compile.md) |
| **Instant-Play Browser / WebGL / Multiplayer Web Game** | **Kaplay (2D)**, **Phaser 3 (2D)**, or **Three.js + Rapier3D (3D)** with Vite + TypeScript | Instant HMR in browser, trivial web deployment, works with Chrome DevTools MCP | [`.ai/recipes/web-game-kaplay-threejs.md`](./recipes/web-game-kaplay-threejs.md) |
| **3D Asset Creation, Level Kitbashing & Procedural Modeling** | **Blender + Blender MCP** (`get_scene_info`, `execute_blender_code`, `generate_3d`, `search_assets`, `.glb` export) | Lets AI build, modify, rig, and export `.glb`/`.gltf` 3D assets directly into Godot/Bevy/Three.js/Unity | [`.ai/recipes/blender-mcp-3d-asset-pipeline.md`](./recipes/blender-mcp-3d-asset-pipeline.md) |
| **High-Fidelity 3D / Commercial Ecosystem** | **Unity 6 (C#)** or **Unreal Engine 5 (C++)** | Huge asset & rendering ecosystem; keep core logic in decoupled C#/C++ classes outside heavy binary scenes | [`.ai/recipes/godot-4-2d-3d-architecture.md`](./recipes/godot-4-2d-3d-architecture.md) |

---

## 2. Approved Library & Addon Radar

- **3D Asset Format**: **glTF 2.0 (`.glb` / `.gltf`)** is the universal standard across Blender, Godot 4, Bevy, and Three.js. Prefer `.glb` over legacy `.fbx` or `.obj` whenever possible.
- **Godot 4 Ecosystem**:
  - **LimboAI** or **Beehave** (behavior trees & state machines), **PhantomCamera** (smooth 2D/3D camera follow/tweening), **GdUnit4** or **GUT** (automated testing).
- **Rust / Bevy Ecosystem**:
  - **Physics**: `avian2d` / `avian3d` (formerly `bevy_xpbd`) or `bevy_rapier2d` / `bevy_rapier3d`.
  - **Input**: `leafwing-input-manager`.
  - **Audio**: `bevy_kira_audio`.
  - **UI / Debug**: `bevy_egui` + `bevy-inspector-egui`.
- **Web Game Ecosystem**:
  - **2D**: `kaplay` (modern successor to Kaboom.js) or `phaser`.
  - **3D**: `three`, `@types/three`, `@dimforge/rapier3d-compat` (WASM physics), `gsap` (tweens), `howler` (audio).

---

## 3. Banned / Anti-Patterns by Default (DO NOT USE)

| Banned Pattern / Tool | Why It Breaks Games | Use This Instead |
| :--- | :--- | :--- |
| **Godot 3.x syntax in Godot 4.x projects** (`KinematicBody2D`, `yield()`, `connect("sig", self, "fn")`) | Breaks immediately in Godot 4 | Use **Godot 4.x syntax**: `CharacterBody2D`, `await`, `signal.connect(callable)`, `@export`, `@onready` |
| **Frame-dependent movement** (`position.x += 5` without `delta`) | Game speed breaks on 144Hz/240Hz monitors vs 60Hz | Always multiply movement/forces by **`delta`** (`dt`) and use fixed physics ticks (`_physics_process` / `FixedUpdate`) |
| **Per-frame heap allocations or `get_node()` / `find()` inside `_process()` / `update()`** | Causes GC spikes and frame stutters | Cache node/component references on ready/startup or use ECS queries |
| **Monolithic 2,000-line Player controller script** | Impossible to debug or extend cleanly | Use a **Finite State Machine (FSM)** or **ECS Components** (Input, Movement, Combat, Health, Animation separated) |
| **Committing raw multi-GB binary assets without Git LFS** | Bloats git history and blocks GitHub pushes | Configure `.gitattributes` with `git lfs track "*.glb" "*.png" "*.ogg" "*.wav"` |
