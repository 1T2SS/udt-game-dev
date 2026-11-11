# Recipe: Blender MCP 3D Modeling & Game Asset Pipeline

## When to Use
Use this recipe alongside **any** 3D game stack (**Godot 4**, **Bevy**, **Three.js**, **Unity**, **Unreal**) whenever the project requires creating, modifying, kitbashing, rigging, or exporting 3D models, props, characters, or environments.

---

## 1. Step-by-Step Blender MCP Discovery & Setup Flow

1. **Check Blender MCP Status**:
   - Call `blender:get_addon_status` (or check if `blender` MCP tools are registered in your agent environment).
2. **If Blender MCP Is Connected**:
   - Call `blender:get_scene_info` to inspect the active Blender scene.
   - Use `blender:search_assets` / `blender:import_asset` or `blender:generate_3d` or `blender:execute_blender_code` (Python `bpy`) to model and material meshes.
   - Always call `blender:look` after modeling steps to visually verify geometry, proportions, and materials!
3. **If Blender or Blender MCP Is Not Set Up Yet**:
   - Check if Blender is installed locally:
     ```powershell
     Get-ChildItem "C:\Program Files\Blender Foundation\Blender*\blender.exe" -ErrorAction SilentlyContinue
     ```
   - Ask the user: *"I can create and export 3D models directly into your game using **Blender MCP**. Would you like me to install/launch Blender and help connect the Blender MCP addon right now?"*
   - Once verified, log the exact Blender version, executable path, and MCP status in [`.ai/ENVIRONMENT.md`](../ENVIRONMENT.md).

---

## 2. Standard Game-Ready `.glb` Export Script (`bpy`)

When exporting a 3D asset from Blender into the game repository (`assets/models/<name>.glb`), enforce these game-ready rules via `execute_blender_code`:
1. **Apply Scale & Rotation** (`Ctrl+A` equivalent: `bpy.ops.object.transform_apply(location=False, rotation=True, scale=True)`) so the game engine receives clean `1.0, 1.0, 1.0` scale transforms.
2. **Origin Placement**: Set object origin to the bottom-center (`Z = 0`) for characters/props that rest on the ground.
3. **Export to glTF 2.0 Binary (`.glb`)**:
   ```python
   import bpy
   bpy.ops.export_scene.gltf(
       filepath=r"E:\Path\To\Project\assets\models\prop_name.glb",
       export_format='GLB',
       use_selection=True,
       export_yup=True,
       export_apply=True
   )
   ```

## 3. Collision Suffix Conventions (Godot 4 & Bevy)
- Name collision helper meshes in Blender with `-col` (trimesh static collision) or `-convcol` (convex collision) so Godot 4 automatically generates physics collision shapes on `.glb` import!
