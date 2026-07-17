---
type: Playbook
title: Loading screen branding
description: Dual-priority loading GUI layers, ECS multi-layer scenes, and version/brand widgets in loading_screen/.
tags: [loading_screen, gui, cosmetics, branding]
timestamp: 2026-07-11T12:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1.6"
---

Boot branding lives under **`loading_screen/`**, separate from `main_menu` / `in_game`. Useful for any public mod, not only total conversions.

# Three-stack

| Layer | Role |
|-------|------|
| `loading_screen/gui/` | Priority layers + version/brand widgets |
| `loading_screen/gfx/scenes/` | ECS scene composition (meshes + image lists) |
| `loading_screen/gfx/images/` | Per-layer illustration textures |

# Dual GUI layers

```txt
layer loading_screen { priority = 20 }
layer custom_loading_screen { priority = 21 }  # overlays
```

# Version / brand widget

Use engine `GetCompleteVersionInfoString` → string-pair list for mod/DLC versions; add static brand rows (`raw_text = "My Mod"` / `"alpha"`). Anchor top-right.

# Multi-layer illustrations

Each parallax layer = image list entry + `pdxmesh` + shared anim name in a scene file. Optional `gfx_environment_file` + camera block.

# Related

* [Loading-screen defines](/total-conversion/loading-screen-defines.md)
* [Mod folder structure](/getting-started/mod-folder-structure.md)

# Citations

[1] MnT `loading_screen/gui/loading_screen.gui`, `custom_loading_screen.gui`
[2] MnT `loading_screen/gfx/images/00_modcon_loading_screen.txt`
[3] MnT `loading_screen/gfx/scenes/00_modcon_loading_screens.txt`
