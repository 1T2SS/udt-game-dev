# Recipe: Rust + Bevy ECS (2D/3D) with Fast-Compile Configuration

## When to Use
Use this recipe when building a code-first 2D or 3D game, simulation, or procedural world in **Rust** using the **Bevy Engine**.

## 1. Official Scaffold Commands
```powershell
cargo init --bin
cargo add bevy
# Enable dynamic_linking during dev for dramatically faster iterative compile times:
cargo add bevy --features dynamic_linking
```

## 2. Mandatory Fast-Compile `.cargo/config.toml`
Bevy is slow to compile without optimization settings in `.cargo/config.toml`. Always create this file on Day 0:
```toml
[profile.dev]
opt-level = 1

[profile.dev.package."*"]
opt-level = 3
```
*(In `Cargo.toml`, keep `dynamic_linking` in a `dev` feature or remove it only for final release builds).*

## 3. Recommended Modular Plugin Architecture (`src/`)
Split game domains into Bevy `Plugin` modules so `main.rs` stays under 50 lines:
```text
src/
├── main.rs               # App::new().add_plugins((DefaultPlugins, GamePlugin)).run()
├── state.rs              # States enum (Menu, Playing, Paused, GameOver)
├── player/               # PlayerPlugin (spawning, input, movement, camera)
├── world/                # WorldPlugin (level generation, lighting, .glb scene loading)
├── combat/               # CombatPlugin (collisions, health, damage events)
└── ui/                   # UiPlugin (HUD, menus)
```

## 4. Verification Commands
- **Fast Compile Check**: `cargo check`
- **Lint**: `cargo clippy`
- **Run Tests**: `cargo test`
- **Run Game**: `cargo run`
