---
type: Playbook
title: Expand raw goods lateral view
description: Country-level bulk RGO panel — list UI, mass actions, and peer-widget injection (Glorp pattern #30).
tags: [gui, rgo, lateralview, glorp, economy]
timestamp: 2026-07-12T22:45:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#30**: besides `location_window.gui`, EU5 exposes **`expand_raw_goods_lateralview.gui`** — a lateral panel listing all raw-good locations for bulk expand/unqueue, market filter, and per-row location actions. Glorp overrides it for ROI display, Construction Manager widgets, and mass auto-expand.

Alternative entry point for **RGO/economy mods** that should not fight `location_window` load order.

# What the panel is

| Property | Value |
|----------|--------|
| Widget root | `lateralview = { name = "expand_raw_goods_lateralview" }` |
| Data | `ExpandRawGoodsLateralView.GetRawGoodLocationsSortSearch` |
| Row scope | `RawGoodLocationItem.GetLocation` |
| Vanilla purpose | Expand RGO in best location, unqueue, auto-expand toggles |

Open from production / raw-goods flows in vanilla; Glorp also styles and extends rows.

# File layout (Glorp)

`in_game/gui/expand_raw_goods_lateralview.gui` (~1,400+ lines) — full override.

Notable blocks:

* **Header** — `RAW_GOOD_BUILDER_TT`, automation shortcut to filtered automation panel.
* **Market filter** — `ExpandRawGoodsLateralView.GetSelectedMarket`, toggle market selection.
* **Mass actions** — expand/unqueue in best/latest locations (vanilla actions on `RawGoodLocationItem`).
* **Per-location row** — pan/highlight, context menu, subject icon, RGO expand controls.

# Peer widget injection (Construction Manager)

Per-row widget slot (parallel to `location_window`):

```txt
widget = {
	size = { 20 44 }
	datacontext = "[RawGoodLocationItem.GetLocation]"
	cm_auto_expand_rgo_widget_expand_raw_goods_lateralview = {}
	button_square_checkbox = {
		visible = "[Not(GetScriptedGui('glorpui_is_cm_active').IsShown(...))]"
		# vanilla Glorp auto-expand when CM not active
		on_action = "[ToggleAutoExpandRGO(Location.Self)]"
	}
}
```

Mass header button:

```txt
cm_mass_auto_expand_rgo_button = {}
```

Same [cross-mod embed](/community-mod-framework/cross-mod-gui-integration.md) pattern as `cm_auto_expand_rgo_widget_location_window` — empty type instance; Construction Manager mod defines the type body.

# Script values in row tooltips

Glorp wires RGO expand **cost** through scoped script values (see [Script values and ROI in GUI](/gui/script-values-and-roi-in-gui.md)):

* `glorpui_rgo_expand_cost` — country + location scopes
* `glorpui_rgo_base_cost_adjustment`, `glorpui_rgo_construction_cost_adjustment`

Defined in `in_game/common/script_values/glorpui_rgo_script_values.txt`.

Useful reference when showing **conversion project cost** in a list row without duplicating vanilla `Location.GetRGOCost` logic.

# Injecting a Convert RGO action here

For Glorp-compatible **RGO conversion** mods:

1. Override `expand_raw_goods_lateralview.gui` (or ship Glorp-merge submod).
2. Add a per-row `button_regular` beside expand controls on `RawGoodLocationItem.GetLocation`.
3. Same ScriptedGui contract as location panel:

```txt
on_action = "[GetScriptedGui('rgo_conv_open').Execute(
	GuiScope.SetRoot(GetPlayer.MakeScope).AddScope('target_location', Location.MakeScope).End)]"
visible = "[Location.GetOwner.IsPlayer]"
enabled = "[GetScriptedGui('rgo_conv_open').IsValid(...)]"
```

4. Mark `# rgo_conv:` for merge diffs.

**Pros:** avoids `location_window` collision with Glorp; players already manage RGOs here.  
**Cons:** still a large-file override; must re-merge on Glorp/vanilla updates.

# vs location_window vs CMF action bar

| Entry | Glorp conflict | Context |
|-------|----------------|---------|
| `location_window` `# RGO` | **High** — full-file fight | In-province, beside RGO pie |
| `expand_raw_goods_lateralview` | **Medium** — separate file | Country list, all RGO provinces |
| CMF action bar | **None** | No location context built-in |

Recommended combo for maximum reach: **action bar** (compat) + **optional** expand-panel or Glorp-merge location button.

# Related

* [Glorp location RGO row](/gui/glorp-location-window-rgo-row.md)
* [Integrating with Glorp UI](/gui/integrating-with-glorp-ui.md)
* [Context-specific widget aliases](/gui/context-specific-widget-aliases.md)
* [Mass opt-in / opt-out location flags](/gui/mass-opt-in-opt-out-location-flags.md)
* [Construction Manager as reference](/references/construction-manager-as-reference.md)
* [Cross-mod GUI integration](/community-mod-framework/cross-mod-gui-integration.md)

# Citations

[1] Glorp `in_game/gui/expand_raw_goods_lateralview.gui`
[2] Glorp `in_game/common/script_values/glorpui_rgo_script_values.txt`
[3] Vanilla `in_game/gui/expand_raw_goods_lateralview.gui` — compare in game install
[4] Construction Manager `3736668860` — same lateral view + mass RGO widgets
