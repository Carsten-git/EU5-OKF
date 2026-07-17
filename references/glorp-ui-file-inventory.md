---
type: Reference
title: Glorp UI file inventory
description: Structural map of the Glorp UI workshop mod (v1.3.10.1) for agents mining local installs.
tags: [glorp, inventory, reference, workshop]
timestamp: 2026-07-12T22:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
resource: steam://workshop/3450310/3601047146
---

Local path (subscribed): `Steam/steamapps/workshop/content/3450310/3601047146/`

Metadata: `glorp.ui` v**1.3.10.1**, `supported_game_version` `1.3.*`, `glorpui_last_updated_game_version` `1.3.10`.

# Dependencies (metadata)

| Mod id | Display name |
|--------|----------------|
| `community_mod_framework` | Community Mod Framework `2.*` |
| `romaimperator.construction_manager` | Construction Manager |
| `autonomous_diplomats` | Autonomous Diplomats |
| `romaimperator.smart_taxes` | SmartTaxes |

Glorp is **CMF-native** and embeds peer-mod widgets from Construction Manager and SmartTaxes.

# Top-level layout

| Root | Role |
|------|------|
| `.metadata/metadata.json` | Descriptor + hard dependencies |
| `in_game/gui/` | **48** `.gui` files — overrides, panels, shared types, `vanilla/cmfg_*` extracts |
| `in_game/common/` | Scripted GUIs, CMM effects, value sync, trait codegen triggers |
| `main_menu/gui/shared/` | Frontend templates, font icons |
| `main_menu/localization/` | Split across `glorpui_*`, `glorpui_cmm_*`, `glorpui_filters_*`, `glorpui_shared_*` per language |

No `tools/` folder ships in the Workshop package (extraction script lives in CMF/Glorp dev repos; extracts are pre-built under `gui/vanilla/`).

# `in_game/gui/` layers

| Layer | Examples | Pattern |
|-------|----------|---------|
| Vanilla-path overrides | `location_window.gui`, `ingame_topbar.gui`, `outliner_entries.gui`, `economy_lateralview.gui`, `expand_raw_goods_lateralview.gui` | Full or heavy override of vanilla path |
| Type shadow / HUD | `glorpUI_hud_topbar.gui`, `glorpUI_hud_bot.gui` | Redefine `type topbar` etc. without replacing shell file |
| New lateral views | `glorpUI_people_lateral_view.gui`, `glorpUI_build_location_lateralview.gui` | Extra player-facing windows |
| Shared cross-cutting | `glorpUI_shared_types.gui`, `shared/glorpUI_*.gui` | Types/templates/tooltips reused across files |
| Panels | `panels/left_panel/glorpUI_left_panel.gui`, `panels/organization/` | Sub-chrome composition |
| CMFG extracts | `vanilla/cmfg_location_window_vanilla_types.gui` (+ 10 more) | Types/templates only — see [CMFG extraction](/gui/cmfg-vanilla-type-extraction.md) |
| Scripted widgets map | `scripted_widgets/glorpUI_scripted_windows.txt` | Window spawn mapping |

# `in_game/common/` (script side)

| Area | Files | Role |
|------|-------|------|
| CMF registration | `on_action/glorpui_cmm_on_actions.txt`, `scripted_effects/glorpui_cmm_effects.txt`, `scripted_guis/glorpui_cmm_scripted_gui.txt` | CMM settings, banner, callback — [worked example](/community-mod-framework/glorp-ui-cmf-worked-example.md) |
| Peer-mod detect | `scripted_guis/glorpui_construction_manager_scripted_gui.txt`, `glorpui_smart_taxes_scripted_gui.txt` | `cmf_active_mod_ids` gates |
| HUD sync | `on_action/glorpui_value_sync_on_actions.txt`, `scripted_effects/glorpui_value_sync_effects.txt` | Monthly aggregates — [HUD value sync](/gui/hud-value-sync.md) |
| RGO economics | `script_values/glorpui_rgo_script_values.txt` | GUI-facing RGO script values |
| Trait filters | `scripted_triggers/glorpui_generated_trait_scripted_triggers.txt` | Codegen output — [trait filter codegen](/tooling/trait-filter-codegen.md) |
| Engine QA | `scripted_effects/glorpui_cmm_warning_suppression.txt` | `cmf_suppress` / warning hygiene |

# High-traffic override files (size / merge cost)

| File | ~Lines (v1.3.10.1) | Notes |
|------|---------------------|-------|
| `location_window.gui` | ~8,200 | Redesigned location chrome; RGO is `header_button_left` — see [Glorp location RGO row](/gui/glorp-location-window-rgo-row.md) |
| `outliner_entries.gui` | large | Outliner + peer widgets |
| `expand_raw_goods_lateralview.gui` | large | Bulk RGO expand UI + CM widgets |

Vanilla-only content mods that copy vanilla `location_window.gui` (~9,600 lines) **do not** match Glorp’s slimmer, redesigned file — load-order collision replaces Glorp’s panel entirely.

# Markers in source

Glorp marks edits with `# GlorpUI:` comments — use these for three-way merges when patching Glorp’s copy instead of vanilla.

# Related

* [Glorp UI as reference](/references/glorp-ui-as-reference.md) — pattern index #1–31
* [Integrating with Glorp UI](/gui/integrating-with-glorp-ui.md) — content-mod compat playbook
* [Expand raw goods lateral view](/gui/expand-raw-goods-lateralview.md) — bulk RGO panel (#30)
* [Player options without lobby events](/community-mod-framework/player-options-without-lobby.md) — CMM defaults (#31)

# Citations

[1] `workshop/content/3450310/3601047146/.metadata/metadata.json`
[2] `workshop/content/3450310/3601047146/in_game/gui/`
[3] `workshop/content/3450310/3601047146/in_game/common/`
