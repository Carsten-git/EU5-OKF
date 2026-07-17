---
type: Reference
title: Glorp UI as reference
description: Workshop UI overhaul (glorp.ui) — map of extractable patterns #1–24 into OKF articles.
tags: [glorp, ui, cmf, reference]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
resource: steam://workshop/3450310/3601047146
---

[Glorp UI](https://steamcommunity.com/sharedfiles/filedetails/?id=3601047146) (`glorp.ui` v1.3.10.1) is a CMF-native UI overhaul. Path: `workshop/content/3450310/3601047146`.

Depends on: CMF `2.*`, Construction Manager, Autonomous Diplomats, SmartTaxes.

# Pattern index (1–24)

| # | Pri | Concept | Article |
|---|-----|---------|---------|
| 1 | H | CMM → country-var aliases for GUI | [CMM aliases for GUI](/community-mod-framework/cmm-aliases-for-gui.md) |
| 2 | H | Cross-mod detect via `cmf_active_mod_ids` | [Cross-mod GUI integration](/community-mod-framework/cross-mod-gui-integration.md) |
| 3 | H | Monthly HUD value sync | [HUD value sync](/gui/hud-value-sync.md) |
| 4 | H | CMF registration trinity | [Glorp CMF worked example](/community-mod-framework/glorp-ui-cmf-worked-example.md) |
| 5 | H | `cmfg_*` vanilla-type extraction | [CMFG vanilla-type extraction](/gui/cmfg-vanilla-type-extraction.md) |
| 6 | H | Dual CMM / fallback settings UI | [Dual settings UI fallback](/community-mod-framework/dual-settings-ui-fallback.md) |
| 7 | H | Peer-mod widget embedding | [Cross-mod GUI integration](/community-mod-framework/cross-mod-gui-integration.md) |
| 8 | M | GUI override file layering | [UI override file layering](/gui/ui-override-file-layering.md) |
| 9 | M | `cmm_add_scripted_gui` dependent visibility | [CMM aliases for GUI](/community-mod-framework/cmm-aliases-for-gui.md) |
| 10 | M | Sync CMM map from outside menu | [Dual settings UI fallback](/community-mod-framework/dual-settings-ui-fallback.md) |
| 11 | M | Trait-filter codegen pipeline | [Trait filter codegen](/tooling/trait-filter-codegen.md) |
| 12 | M | Script values in GUI economics | [Script values and ROI in GUI](/gui/script-values-and-roi-in-gui.md) |
| 13 | M | Dropdown aliases → ROI display | [Script values and ROI in GUI](/gui/script-values-and-roi-in-gui.md) |
| 14 | M | Console createwidget fallback | [Dual settings UI fallback](/community-mod-framework/dual-settings-ui-fallback.md) |
| 15 | M | Mass-action UI counter | [Mass-action UI counter](/gui/mass-action-ui-counter.md) |
| 16 | M | Map-mode gradient override | [Custom map modes](/gui/custom-map-modes.md) + Glorp note |
| 17 | M | Multi-file loc split | [Multi-file loc split](/localization/multi-file-loc-split-for-ui-mods.md) |
| 18 | M | Mapmode hover preview | [Mapmode hover preview](/gui/mapmode-hover-preview.md) |
| 19 | L | Alert banner cosmetic override | [UI overhaul niche patterns](/gui/ui-overhaul-niche-patterns.md) |
| 20 | L | HUD type shadowing | [UI override file layering](/gui/ui-override-file-layering.md) |
| 21 | L | Manual suppress (prefer `cmf_suppress`) | [UI overhaul niche patterns](/gui/ui-overhaul-niche-patterns.md) |
| 22 | L | Full-file GUI override cost | [UI override file layering](/gui/ui-override-file-layering.md) |
| 23 | L | One-off mod-compat hacks | [UI overhaul niche patterns](/gui/ui-overhaul-niche-patterns.md) |
| 24 | L | Shared cross-mod GUI types | [Cross-mod GUI integration](/community-mod-framework/cross-mod-gui-integration.md) |
| 25 | H | Location RGO row redesign (`header_button_left`) | [Glorp location RGO row](/gui/glorp-location-window-rgo-row.md) |
| 26 | H | Content-mod compat / merge strategy | [Integrating with Glorp UI](/gui/integrating-with-glorp-ui.md) |
| 27 | H | CMM settings catalog (tabs, defaults, inverted bools) | [CMM settings catalog design](/community-mod-framework/cmm-settings-catalog-design.md) |
| 28 | M | `cmm_set_dropdown_multiselector` | [CMM dropdown multiselector](/community-mod-framework/cmm-dropdown-multiselector.md) |
| 29 | M | Pause menu → CMM host vs client | [CMM pause menu entry](/community-mod-framework/cmm-pause-menu-entry.md) |
| 30 | M | `expand_raw_goods_lateralview` bulk RGO UI | [Expand raw goods lateral view](/gui/expand-raw-goods-lateralview.md) |
| 31 | H | Player options without lobby / new-game wizard | [Player options without lobby events](/community-mod-framework/player-options-without-lobby.md) |

# Local install

Subscribed path: `Steam/steamapps/workshop/content/3450310/3601047146/` — structural map in [Glorp UI file inventory](/references/glorp-ui-file-inventory.md).

# Citations

[1] Workshop `3450310/3601047146` — Glorp UI v1.3.10.1
[2] [Community Mod Framework](/community-mod-framework/)
