# Recipe: Web Game (2D Kaplay / Phaser 3 or 3D Three.js + Rapier Physics)

## When to Use
Use this recipe when building an instant-play browser game (itch.io, CrazyGames, Poki, Discord Activity, or standalone desktop wrapper via Tauri v2).

## 1. Official Scaffold Commands

### Option A: 2D Arcade / Action Game (Kaplay + Vite + TypeScript)
```powershell
bun create vite . --template vanilla-ts
bun install
bun add kaplay howler
```

### Option B: 3D WebGL / WebGPU Game (Three.js + Rapier3D Physics + Vite + TypeScript)
```powershell
bun create vite . --template vanilla-ts
bun install
bun add three @dimforge/rapier3d-compat gsap howler
bun add -D @types/three
```

## 2. Recommended Directory Layout
```text
├── public/
│   └── assets/           # .glb 3D models (from Blender MCP), sprites, audio
├── src/
│   ├── main.ts           # Game loop & canvas initialization
│   ├── core/             # InputManager, AssetLoader, AudioController
│   ├── entities/         # Player, Enemies, Projectiles
│   ├── scenes/           # MenuScene, GameplayScene, GameOverScene
│   └── ui/               # HTML/CSS HUD overlay
└── index.html
```

## 3. Verification Commands
- **Typecheck**: `bun run tsc --noEmit`
- **Build**: `bun run build`
- **Visual & FPS Check**: Start `bun run dev` and inspect via Chrome DevTools MCP or browser subagent to verify 60fps canvas rendering and zero console errors.
