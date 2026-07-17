---
type: Playbook
title: Glorp location window RGO row
description: Where Glorp puts the RGO control, how peer widgets slot in, and why vanilla sibling-button patches miss it.
tags: [glorp, gui, location-window, rgo, compatibility]
timestamp: 2026-07-12T22:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#25**: the location panel RGO control is **not** the vanilla compact `button_regular` row. Agents patching vanilla `# RGO` at ~line 1640 will not reach Glorp players.

# Vanilla vs Glorp RGO widgets

| Aspect | Vanilla (and vanilla-copy mods) | Glorp UI v1.3.10.1 |
|--------|--------------------------------|---------------------|
| Primary RGO UI | Compact `button_regular` `size = { 90 28 }` in location summary strip | `header_button_left` `name = "location_rgo"` `size = { 125 40 }` in header stat row |
| Data context | `[Location]` / direct `Location.*` | `[LocationView.GetLocation]` / `LocationView.*` |
| Goods icon | Inside piechart | Separate `button` with `ShowGoods` + `SpecificGoodsMarket_tooltip` |
| Peer widgets | — | Empty type instances before tooltips |

Glorp `# RGO` anchors (search `name = "location_rgo"`):

1. **Header strip** (~line 2248) — main player-facing RGO control.
2. **Building card** (~line 8937) — RGO row inside building list (secondary).

There is **no** compact `button_regular` RGO block in Glorp’s `location_window.gui`.

# Peer widget slots (Construction Manager)

Glorp embeds CM widgets as empty type instances inside the header RGO block:

```txt
# GlorpUI: Construction Manager auto-food widget, food RGO locations only
cm_auto_food_widget_location_window = {}

# GlorpUI: Construction Manager RGO auto-expand widget
cm_auto_expand_rgo_widget_location_window = {}
```

Pattern: the dependency mod defines the widget **type**; Glorp (host UI) **instantiates** it at a stable anchor. Same idea as [cross-mod GUI integration](/community-mod-framework/cross-mod-gui-integration.md) pattern #7.

Related CM hooks elsewhere:

* `expand_raw_goods_lateralview.gui` — `cm_mass_auto_expand_rgo_button`, per-row `cm_auto_expand_rgo_widget_expand_raw_goods_lateralview`
* Building upgrade rows — `cm_check_for_auto_expand_when_upgrading_building` scripted GUI

When CM is active, Glorp hides duplicate vanilla auto-expand chrome via `glorpui_is_cm_active` + `cmf_active_mod_ids` / `flag:cm`.

# Mapmode hover on stat row

Adjacent header stats use pattern #18:

```txt
onmousehierarchyenter = "[PdxGuiWidget.PushMapModeOverride('proximity')]"
```

See [Mapmode hover preview](/gui/mapmode-hover-preview.md).

# CMFG type extract

Types/templates live in `in_game/gui/vanilla/cmfg_location_window_vanilla_types.gui` (auto-generated header). Override file keeps widget tree + `# GlorpUI:` deltas — see [CMFG vanilla-type extraction](/gui/cmfg-vanilla-type-extraction.md).

# Injecting a sibling button (for content mods)

To add a **Convert RGO** button for Glorp users:

1. Start from **Glorp’s** `location_window.gui`, not vanilla.
2. Patch beside `header_button_left` `name = "location_rgo"` (or inside its parent `hbox` if layout allows).
3. Use `LocationView.GetLocation` scopes where Glorp does (or mirror existing `LocationView` bindings in that block).
4. Keep the same ScriptedGui contract: `visible` on owner/player check; `enabled` on `IsValid`; `Execute` with `target_location` scope.
5. Mark edits `# your_mod:` for merge diffs on Glorp updates.

Do **not** assume the vanilla compact-row injection site exists in Glorp.

# Related

* [Integrating with Glorp UI](/gui/integrating-with-glorp-ui.md)
* [UI override file layering](/gui/ui-override-file-layering.md)
* [ScriptedGui IsValid button greying](/gui/scripted-gui-isvalid-greying.md)

# Citations

[1] Glorp `in_game/gui/location_window.gui` — `# RGO`, `location_rgo`, CM widgets
[2] Glorp `in_game/gui/vanilla/cmfg_location_window_vanilla_types.gui`
[3] Glorp `in_game/gui/expand_raw_goods_lateralview.gui` — bulk RGO UI
