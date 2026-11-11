# MCP_AND_TOOLS.md — Proactive Environment, Engine & Blender MCP Setup (`game-dev`)

> **AI Agent Instruction**: On Day-0 Bootstrap (and whenever 3D assets or engine tools are needed), proactively check what is installed and configured. **For 3D games or assets, always check if Blender MCP is available — if it is, use it; if not, ask the user if you can set up Blender + Blender MCP for them!** Do the same for the game engine development environment and record everything in [`.ai/ENVIRONMENT.md`](./ENVIRONMENT.md) so future creations start with verified environment information.

---

## 1. Proactive MCP Server Checklist (Check & Offer to Set Up!)

| MCP Server / Tool | When to Suggest & Use | How to Check & Set Up |
| :--- | :--- | :--- |
| **Blender MCP** (`blender` MCP server: `get_addon_status`, `get_scene_info`, `execute_blender_code`, `look`, `generate_3d`, `search_assets`, `import_asset`) | **Mandatory to check for any 3D game, 3D level, or 3D sprite/model workflow!** Lets the AI model 3D meshes, apply materials, search/import 3D assets, generate 3D models, visually inspect the viewport, and export `.glb` directly into the game project. | 1. If `blender` MCP tools are in your tool list, run `get_addon_status` to check if Blender is running and connected.<br/>2. **If Blender MCP is not connected or not installed**: Ask the user: *"For 3D modeling and asset creation, we can use **Blender MCP** so I can build, inspect, and export 3D models directly into your game. Would you like me to check your Blender installation and set up Blender MCP for you?"* |
| **Chrome DevTools MCP** (`chrome-devtools`) | For WebGL / Three.js / Kaplay / Phaser web games or HTML5 game exports. | Check if `chrome-devtools` MCP is available to inspect WebGL FPS, canvas rendering, and console logs. |
| **`cargo-fallow` / `fallow`** | For Rust (Bevy) or TypeScript (Web Game) code health and dead-code detection. | Check `cargo fallow --version` or `npx fallow --version`. |

---

## 2. Game Dev Environment Doctor (Check What the Project Needs)

Run these non-destructive environment checks to see what engines and tools are already installed on the machine:

```powershell
# 1. Check Blender installation (for 3D modeling & Blender MCP)
where.exe blender 2>$null
Get-ChildItem "C:\Program Files\Blender Foundation\Blender*\blender.exe" -ErrorAction SilentlyContinue | Select-Object -ExpandProperty FullName

# 2. Check Game Engines & Runtimes
godot --version 2>$null
rustc --version 2>$null
cargo --version 2>$null
dotnet --version 2>$null
bun --version 2>$null

# 3. Check Git & Git LFS (essential for game binary assets!)
git --version
git lfs version 2>$null
```

### How to Set Up Missing Tools (Ask User Permission First!)
- **Install Blender (if missing for 3D work)**:
  - `winget install BlenderFoundation.Blender`
  - Once installed, guide or script enabling the Blender MCP addon and connecting the `blender` MCP server, then verify with `get_addon_status`.
- **Install Godot 4 Engine**:
  - Standard GDScript edition: `winget install GodotEngine.GodotEngine`
  - .NET (C#) edition: `winget install GodotEngine.GodotEngine.Mono`
- **Install Git LFS (for `.glb`, `.png`, `.ogg`, `.wav` assets)**:
  - `winget install GitHub.GitLFS && git lfs install`
- **Install Rust + Fast Linker for Bevy**:
  - `winget install Rustlang.Rustup`
  - Configure dynamic linking (`bevy/dynamic_linking`) in dev profile for 5x faster iterative compiles.

---

## 3. Give Future Creations Better Starting Information
Every time you verify an engine path (e.g., exact `blender.exe` or `godot.exe` path), configure Blender MCP, or solve a compiler/linker issue:
1. Save the exact paths, versions, and MCP connection status in [`.ai/ENVIRONMENT.md`](./ENVIRONMENT.md) (`Environment Check Status: VERIFIED`).
2. Append reusable setup discoveries to Section 5 of [`.ai/MEMORY.md`](./MEMORY.md) (`Template & Environment Learnings`) so running `scripts/harvest-learnings.ps1` updates the UDT Hub for all future game projects!
