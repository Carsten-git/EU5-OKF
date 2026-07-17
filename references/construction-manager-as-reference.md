---
type: Reference
title: Construction Manager as reference
description: Pattern map for romaimperator Construction Manager (workshop 3736668860) — UI REPLACE, CMM automation, off-screen queues, filters, map modes.
tags: [reference, construction-manager, gui, cmf, rgo]
timestamp: 2026-07-13T17:00:00+10:00
status: complete
source_mod: romaimperator.construction_manager
source_version: "2.2.11"
---

**Construction Manager (CM)** is a CMF-dependent utility that automates building/RGO expand, food, urbanize, and town rights — with deep GUI overrides and Glorp peer sync.

Workshop: `3450310/3736668860` · metadata id `romaimperator.construction_manager` · requires CMF `2.*`.

# When to open this mod

| You need… | Start here |
|-----------|------------|
| Inject controls into location / production / expand-RGO panels | [Context-specific widget aliases](/gui/context-specific-widget-aliases.md), [Expand raw goods](/gui/expand-raw-goods-lateralview.md) |
| Run logic that needs GUI-only bindings monthly | [Off-screen scripted widget drivers](/gui/offscreen-scripted-widget-drivers.md) |
| Search filters on lateral lists | [Search filters for lists](/gui/search-filters-for-lists.md) |
| Mass toggle UX (all on + exclusions) | [Mass opt-in / opt-out flags](/gui/mass-opt-in-opt-out-location-flags.md) |
| Economy map modes tied to automation state | [Custom map modes](/gui/custom-map-modes.md) (CM section) |
| Ordered CMM feature priority + list lookups | [CMM priority feature dispatcher](/community-mod-framework/cmm-priority-feature-dispatcher.md) |
| Rank change via constructible dummy building | [Dummy rank-upgrade buildings](/buildings/dummy-rank-upgrade-buildings.md) |
| Peer-detect with Glorp | [Integrating with Glorp UI](/gui/integrating-with-glorp-ui.md) |

# Architecture snapshot

1. **Full-file REPLACE** of vanilla GUI paths (`location_window.gui`, `building_view.gui`, `production_lateralview.gui`, `expand_raw_goods_lateralview.gui`) with inline CM widgets + `glorpui_is_cm_active` coexistence gates.
2. **Always-on scripted widgets** stage/drain construction queues and classify building types off-screen.
3. **CMM** (`mod_id = cm`) holds tabs: settings priority list, auto_build, auto_food, auto_urbanize, auto_town_rights.
4. **Monthly pulse** (`cmf_monthly_human_country_pulse`) walks an ordered feature list; GUI window constructs.
5. **Shared Glorp artifacts** (`glorpui_*` scripted_guis, ROI script values, `cm_glorp_synced_types.gui`) so Glorp or CM can load alone.

# Pattern index (CM extract)

| # | Pattern | Article |
|---|---------|---------|
| CM-1 | Off-screen scripted widget drivers | [offscreen-scripted-widget-drivers](/gui/offscreen-scripted-widget-drivers.md) |
| CM-2 | Search filters (`gui/filters/`) | [search-filters-for-lists](/gui/search-filters-for-lists.md) |
| CM-3 | Context widget aliases (`_pl` / `_bv` / …) | [context-specific-widget-aliases](/gui/context-specific-widget-aliases.md) |
| CM-4 | Mass opt-in / opt-out location flags | [mass-opt-in-opt-out-location-flags](/gui/mass-opt-in-opt-out-location-flags.md) |
| CM-5 | CMM priority feature dispatcher | [cmm-priority-feature-dispatcher](/community-mod-framework/cmm-priority-feature-dispatcher.md) |
| CM-6 | Dummy rank-upgrade buildings | [dummy-rank-upgrade-buildings](/buildings/dummy-rank-upgrade-buildings.md) |
| CM-7 | Map modes + shared filter script values | [custom-map-modes](/gui/custom-map-modes.md) |

Deprioritized for this extract (balance / feature-specific): auto-food scoring internals, town-rights catalog contents, exact ROI thresholds.

# Key paths

| Area | Path under workshop mod |
|------|-------------------------|
| Scripted widgets | `in_game/gui/scripted_widgets/cm_scripted_widgets.txt` |
| CMM register | `in_game/common/scripted_effects/cm_cmm_effects.txt` |
| Monthly automation | `in_game/common/on_action/cm_on_action.txt` |
| Filters | `in_game/gui/filters/cm_*.txt` |
| Map modes | `in_game/gfx/map/map_modes/cm_map_modes.txt` |
| Peer detect | `in_game/common/scripted_guis/glorpui_construction_manager_scripted_gui.txt` |
| CMF dep UI | `main_menu/gui/cmf_dependency_check.gui` |

# Citations

[1] Workshop `3736668860` Construction Manager v2.2.11
[2] CMF dependency declared in `.metadata/metadata.json`
[3] Cross-links to Glorp peer files already inventoried under [Glorp UI file inventory](/references/glorp-ui-file-inventory.md)
