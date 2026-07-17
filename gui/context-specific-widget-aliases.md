---
type: Playbook
title: Context-specific widget type aliases
description: Define thin wrapper widget types per panel (production list, building view, location window) that share one core control but fix size, datacontext, and visibility.
tags: [gui, widgets, types, rgo, construction-manager]
timestamp: 2026-07-13T17:00:00+10:00
status: complete
source_mod: romaimperator.construction_manager
source_version: "2.2.11"
---

Construction Manager pattern **CM-3**: one **core** button type, many **context aliases** so the same control drops cleanly into differently sized vanilla slots.

# Problem

Vanilla panels reserve different slot sizes and provide different datacontexts (`BuildingCandidate`, `Location`, `RawGoodLocationItem`). Copy-pasting the full button into every REPLACE file duplicates bugs.

# Pattern

1. Implement **core** type once (checkbox, glow, scripted_gui execute / IsShown, right-click settings).
2. Add **alias types** that only set `size`, `visible`, `datacontext`, and nest the core widget.
3. In overridden vanilla files, instantiate the alias (`cm_auto_expand_rgo_widget_location_window = {}`).

```txt
# Core (simplified)
type cm_auto_expand_existing_building_button = … { /* toggle chrome */ }

# Production lateral list wrapper
type cm_auto_expand_existing_building_button_pl = widget {
	size = { 20 44 }
	visible = "[ObjectsEqual(BuildingCandidate.GetBuilding.GetOwner, Player.Self)]"
	datacontext = "[BuildingCandidate.GetBuilding]"
	widget = { cm_auto_expand_existing_building_button = {} }
}
```

# CM suffix convention

| Suffix | Panel |
|--------|-------|
| `_pl` | Production lateral view |
| `_bv` | Building view (often self-drawn background) |
| `_bll` | Build-location lateral view |
| `_location_window` | Location window RGO header |
| `_expand_raw_goods_lateralview` | Expand raw goods row |
| `_food_production_lateralview` | Food production row |

# Coexistence with vanilla / Glorp

Keep vanilla checkbox **beside** the CM widget; hide vanilla when peer active:

```txt
visible = "[Not(GetScriptedGui('glorpui_is_cm_active').IsShown(...))]"
```

See [Integrating with Glorp UI](/gui/integrating-with-glorp-ui.md).

# Injection points (worked)

| File | Where |
|------|-------|
| `location_window.gui` | Inside `header_button_left` named `location_rgo` |
| `building_view.gui` | Replace / comment vanilla auto-expand checkbox |
| `production_lateralview.gui` | AutoExpand slot widget column |
| `expand_raw_goods_lateralview.gui` | Per-row 20×44 column + header mass button |

Also use `blockoverride "lateralview_topbuttons_extra"` for cross-navigation (open automation filter).

# Related

* [Glorp location RGO row](/gui/glorp-location-window-rgo-row.md)
* [Expand raw goods lateral view](/gui/expand-raw-goods-lateralview.md)
* [UI override file layering](/gui/ui-override-file-layering.md)

# Citations

[1] CM `in_game/gui/cm_auto_expand_button.gui`, `cm_auto_expand_rgo_button.gui`
[2] CM overrides of `location_window.gui`, `expand_raw_goods_lateralview.gui`
