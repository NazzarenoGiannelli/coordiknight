# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

CoordiKnight is a Blender addon (Python) that copies transform data (location, rotation, scale) from selected Blender objects to the clipboard in a format that can be pasted directly into Unreal Engine 5 as placeholder static meshes.

**Target platforms**: Blender 2.83+, Unreal Engine 5.1+

## Architecture

The entire addon is contained in `__init__.py` (~100 lines):

- **`OBJECT_OT_coordiknight`**: Main operator class that handles the transform export
- **`menu_func`**: Adds operator to View3D > Object menu
- **`register`/`unregister`**: Standard Blender addon lifecycle hooks

### Transform Conversion Logic

The addon bridges Blender's and UE5's coordinate systems:
- **Location**: Converts meters to centimeters (×100), negates Y axis
- **Rotation**: Converts radians to degrees, negates Y and Z axes; maps Blender (X,Y,Z) to UE5 (Roll, Pitch, Yaw)
- **Scale**: Direct 1:1 mapping

### Output Format

Generates UE5 T3D text format (`Begin Map`/`Begin Actor`/`End Actor`/`End Map`) using default Cube as placeholder mesh. Object names from Blender are preserved in `ActorLabel`.

## Development

No build system, tests, or external dependencies. To test changes:
1. Edit `__init__.py`
2. In Blender: Edit > Preferences > Add-ons > disable then re-enable addon (or restart Blender)
3. Select objects and run operator from Object menu

Uses Blender's built-in clipboard (`bpy.context.window_manager.clipboard`) - no external libraries needed.
