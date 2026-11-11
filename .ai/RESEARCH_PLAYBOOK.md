# RESEARCH_PLAYBOOK.md — 2D & 3D Game Dev Pre-Coding Research Protocol

> **Preset**: `game-dev`
> **Rule**: Verify exact engine version APIs, coordinate system conventions, and 3D asset pipeline tools before writing game code.

---

## 1. Engine Version & API Verification

1. **Godot 4.x Verification**:
   - Check the installed Godot version (`godot --version`) and ensure `project.godot` uses `config_version=5`.
   - Verify Godot 4 annotations (`@export`, `@onready`, `@rpc`) and typed GDScript (`var speed: float = 200.0`, `func move(delta: float) -> void:`).
   - Run `godot --headless --quit --check-only` to validate script syntax from the CLI without opening a GUI window!

2. **Bevy (Rust) Version Verification**:
   - Bevy releases major API updates every few months (e.g., Bevy 0.14 vs 0.15+ required components vs bundles).
   - **Mandatory Check**: Run `cargo info bevy` and verify whether your installed version uses **Required Components** (`#[require(Transform, Visibility)]`) or legacy `Bundle` structs before writing ECS spawn code.

3. **Coordinate System & Scale Alignment (Blender ↔ Game Engine)**:
   - **Blender** uses Right-Handed **+Z Up** (`1 unit = 1 meter`).
   - **Godot 4 / Bevy / Three.js** use Right-Handed **+Y Up** (`-Z Forward`, `1 unit = 1 meter`).
   - When exporting `.glb` from Blender (via **Blender MCP** `bpy.ops.export_scene.gltf`), ensure `+Y Up` conversion (`export_yup=True`, which is Blender's default for glTF 2.0) is enabled so models import upright and at 1:1 meter scale.

## 2. 3D Asset & Modeling Research (Blender MCP First)
- Whenever the game needs 3D models, props, characters, environments, or animations:
  1. Check if **Blender MCP** (`blender` MCP server) is active via [`.ai/MCP_AND_TOOLS.md`](./MCP_AND_TOOLS.md).
  2. If active, call `get_addon_status` and `get_scene_info`, search or generate assets (`search_assets`, `generate_3d`, or procedural `execute_blender_code`), visually inspect with `look`, and export directly into `assets/models/*.glb`.
  3. Record working scale factors, material conventions, and collision naming suffixes (e.g., `-col`, `-colonly` in Godot) in `.ai/research/asset-pipeline.md`.
