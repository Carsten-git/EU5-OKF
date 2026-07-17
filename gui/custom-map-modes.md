---
type: Playbook
title: Custom map modes
description: Add heatmap map modes driven by script_values and location variables — MnT, Glorp, Construction Manager, and Zorange utility collection.
tags: [gui, map-modes, gfx, script-values]
timestamp: 2026-07-13T17:10:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Custom map modes make invisible systems (GDP proxies, automation state, building caps, geography scores) playable. Definitions live under `in_game/gfx/map/map_modes/`. **Additive** files work for small utility mods — no TC required (see ZMC below).

# Pattern

1. Decide [live vs cached metrics](/gui/live-vs-cached-mapmode-metrics.md).
2. Expose scales via `script_values` and/or location variables.
3. Define a map mode: `map_color`, legends, tooltips, refresh counters, `gradient_parameters`.
4. Ship icon + [localization / game concepts](/gui/mapmode-localization-and-concepts.md).

# Additive mode checklist (ZMC / utility mods)

From Zorange's Mapmode Collection (`3697317887`) — copy this skeleton per mode:

| Piece | Notes |
|-------|-------|
| Mode key | Prefixed id (`zmc_…`) = icon stem |
| `map_color` | Branch water → uncolonised → bands / `lerp` |
| `legend_key` | One per band color; `desc` → loc |
| `tooltip_key` | Land / water / uncolonised (and edge cases) |
| `*_map_names` / `*_tooltip_context` | Usually `location` |
| `fill_in_impassable` / `enable_snow` / `use_fow` | ZMC: fill yes, snow no, fow no |
| `flatmap_behaviour` | Often `always` for data heatmaps |
| `category` | `geography` \| `economy` \| `population` \| `debug` |
| `index` | Order within category |
| `map_markers = { all = no }` | Hide clutter on data modes |
| `gradient_parameters` | Copy `@data_*` / `@flatmap_far_*` block from vanilla/ZMC header |
| Refresh counters | `Month` for live economy; terrain DB update for static geography |
| Icon | `main_menu/gfx/interface/icons/map_modes/<mode_id>.dds` |

Header of `zmc_map_modes.txt` duplicates vanilla `@zoom_step_*` and `@data_*` / hollow / flatmap constants — same technique as Glorp #16 gradient retune.

# Typical fields (verify against vanilla + MnT)

- Color mode / gradient presets (`@data_*` style)
- `color_refresh_counters` + `color_and_names_refresh_counters`
- `flatmap_behaviour`
- Tooltip keys bound to the same vars / script values the heatmap uses

Public schema docs are thin — treat workshop + vanilla as truth.

# Gradient / parameter retune (Glorp #16)

UI overhauls may ship map mode files that duplicate vanilla `@` gradient constants and per-mode `gradient_parameters`. Technique = override rendering parameters; keep numbers as taste, not gospel.

# Construction Manager worked example (CM-7)

Workshop CM (`3736668860`) adds economy modes that **share scripted triggers / script_values with search filters**:

| Mode | Idea |
|------|------|
| `cm_breadbasket` | Highlight locations where auto-food is active |
| `cm_location_food_potential` | Lerp on food-potential script value |
| `cm_location_food_potential_hidden` | Hidden duplicate to force color refresh |

Extra CM techniques: `secondary_map_color`, `shader_id`, `refresh_colors_on_selection_change`, pair with [search filters](/gui/search-filters-for-lists.md), optional auto-`SetMapMode` from [off-screen widgets](/gui/offscreen-scripted-widget-drivers.md).

# Related

* [Live vs cached mapmode metrics](/gui/live-vs-cached-mapmode-metrics.md)
* [Mapmode localization and game concepts](/gui/mapmode-localization-and-concepts.md)
* [Zorange's Mapmode Collection as reference](/references/zorange-mapmode-collection-as-reference.md)
* [Mapmode hover preview](/gui/mapmode-hover-preview.md)
* [Search filters for lists](/gui/search-filters-for-lists.md)
* [Construction Manager as reference](/references/construction-manager-as-reference.md)
* [Glorp UI reference](/references/glorp-ui-as-reference.md)
* [Dynamic centers of importance](/map/dynamic-centers-of-importance.md)

# Citations

[1] MnT `in_game/gfx/map/map_modes/mnt_map_modes.txt`
[2] Glorp `in_game/gfx/map/map_modes/glorpUI_map_modes.txt`
[3] CM `in_game/gfx/map/map_modes/cm_map_modes.txt`
[4] ZMC `in_game/gfx/map/map_modes/zmc_map_modes.txt` (workshop `3697317887`)
