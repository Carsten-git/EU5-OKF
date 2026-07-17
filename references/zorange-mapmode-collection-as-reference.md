---
type: Reference
title: Zorange's Mapmode Collection as reference
description: Pattern map for zoranges.mapmode.collection (workshop 3697317887) — additive utility map modes without CMF.
tags: [reference, map-modes, utilities]
timestamp: 2026-07-13T17:10:00+10:00
status: complete
source_mod: zoranges.mapmode.collection
source_version: "1.0"
---

**Zorange's Mapmode Collection (ZMC)** is a small, CMF-free utility that only adds map modes + supporting script values, loc, icons, and one setup on_action.

Workshop: `3450310/3697317887` · metadata id `zoranges.mapmode.collection` · supported_game_version `1.1.10` (verify against current game when adapting).

# Modes shipped (inventory)

| Mode id | Category | Metric style |
|---------|----------|--------------|
| `zmc_urbanisation_suitability` | geography | **Cached** location var from composite script_value |
| `zmc_control_times_market_access` | economy | **Live** control × market access |
| `zmc_tax_base_per_population` | economy | Live tax base / population |
| `zmc_building_cap_percentage` | economy | Live used building slots % |
| `zmc_pop_cap` / `zmc_pop_cap_percentage` | population | Live capacity / % used |
| `zmc_pop_growth_percentage` | population | Live local growth |
| `zmc_local_monthly_development` | economy | Live monthly development |
| `zmc_time_until_100_development` | economy | Live years-to-100 (guarded divide) |
| `zmc_local_production_efficiency` | economy | Live local+global PE |
| `zmc_local_max_literacy` | economy | Live (heavy — pop loop; lag risk) |
| `zmc_control_difference` | economy | Live max − local control |
| `zmc_location_size` | debug | Live location size |
| `zmc_province_tax_base` | economy | Live province tax base |

# Pattern index

| # | Pattern | Article |
|---|---------|---------|
| ZMC-1 | Live vs cached metrics | [live-vs-cached-mapmode-metrics](/gui/live-vs-cached-mapmode-metrics.md) |
| ZMC-2 | Loc, legends, game concepts, textformatting | [mapmode-localization-and-concepts](/gui/mapmode-localization-and-concepts.md) |
| ZMC-3 | Full additive map mode file checklist | [custom-map-modes](/gui/custom-map-modes.md) (ZMC section) |

# Key paths

| Area | Path |
|------|------|
| Modes | `in_game/gfx/map/map_modes/zmc_map_modes.txt` |
| Script values | `in_game/common/script_values/zmc_*.txt` |
| Setup | `in_game/common/on_action/zmc_on_game_start.txt` |
| Icons | `main_menu/gfx/interface/icons/map_modes/zmc_*.dds` |
| Loc | `main_menu/localization/english/zmc_loc_l_english.yml` |
| Concepts | `main_menu/common/game_concepts/zmc_concepts.txt` |
| Text colors | `main_menu/gui/zmc_textformatting.gui` |

# Citations

[1] Workshop `3697317887` Zorange's Mapmode Collection v1.0
