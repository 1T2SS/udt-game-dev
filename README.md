# Universal 2D & 3D Game Development Template (`udt-game-dev`)

> **AI-First Vibe Coding Preset** for standalone 2D and 3D games (**Godot 4.x**, **Rust Bevy ECS**, **Unity 6**, **Unreal Engine 5**, **Raylib**, and **Web Games** with Kaplay / Phaser / Three.js).
> Includes built-in **Blender MCP 3D Modeling & Asset Pipeline** integration and automatic engine environment discovery/setup.

---

## Quick Start (30 Seconds)

### Option A: From GitHub
1. Click **"Use this template" → "Create a new repository"** on GitHub.
2. Clone your new repo and open it in any AI coding agent.
3. Describe the 2D or 3D game you want to create!

### Option B: From Local UDT Hub
```powershell
.\scripts\new-project.ps1 -Preset game-dev -Target "E:\Projects\MyGame" -InitGit
```

---

## Key Features
- **3D Modeling via Blender MCP**: When building 3D games or assets, your AI agent automatically checks if **Blender MCP** (`blender` MCP server) is available to create, inspect, texture, and export `.glb`/`.fbx` models directly into your game's asset folder — and if Blender or the Blender MCP addon isn't set up yet, the agent offers to install and configure it for you!
- **Proactive Engine Environment Doctor**: Automatically checks your local engine CLI (Godot, Cargo/Bevy fast-compile linkers, Unity/Unreal, Bun/Vite, Git LFS), helps set up missing tools, and records verified paths in [`.ai/ENVIRONMENT.md`](./.ai/ENVIRONMENT.md) so future game projects start ready to go.
