# Recipe: Godot 4.x (2D & 3D) Clean Game Architecture

## When to Use
Use this recipe when building a 2D or 3D indie game in **Godot 4.3+** (GDScript or C#). Godot 4 uses plain-text `.tscn` scenes and `.tres` resources that AI agents can read, generate, and verify headlessly.

## 1. Project Initialization & `.gitattributes` (Git LFS)
Create `project.godot` (`config_version=5`) and `.gitattributes`:
```gitattributes
*.glb filter=lfs diff=lfs merge=lfs -text
*.png filter=lfs diff=lfs merge=lfs -text
*.ogg filter=lfs diff=lfs merge=lfs -text
*.wav filter=lfs diff=lfs merge=lfs -text
```

## 2. Recommended Directory Layout
```text
├── project.godot
├── assets/
│   ├── models/           # .glb 3D assets exported via Blender MCP
│   ├── sprites/          # 2D textures & sprite sheets
│   ├── audio/            # sfx/ and music/ (.ogg, .wav)
│   └── shaders/          # .gdshader files
├── src/
│   ├── autoload/         # Global EventBus, AudioManager, SaveManager, GameState
│   ├── entities/         # Player, enemies, NPCs (self-contained scene + script + state machine)
│   ├── levels/           # Playable level scenes
│   ├── systems/          # Reusable components (HitboxComponent, HealthComponent, VelocityComponent)
│   └── ui/               # HUD, menus, dialogs
└── tests/                # Automated GDScript tests
```

## 3. Core Godot 4 Rules for AI Agents
- **Composition over Inheritance**: Build reusable `HealthComponent`, `HitboxComponent`, and `HurtboxComponent` nodes instead of deep class hierarchies.
- **Signal-Up, Call-Down**: Child nodes emit signals upward; parent nodes call methods downward; distant nodes communicate through an `Events` autoload singleton (`EventBus`).
- **Typed GDScript**: Always use static types (`var health: int = 100`, `func take_damage(amount: int) -> void:`) for better performance and compile-time error catching.

## 4. Headless CLI Verification Command
Verify that all GDScript files parse and compile without errors from the terminal:
```powershell
godot --headless --quit
```
