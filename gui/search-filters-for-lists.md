---
type: Playbook
title: Search filters for lists
description: Add custom search filters under in_game/gui/filters for lateral views — tags, exclusive groups, range sliders, and script_value metrics.
tags: [gui, filters, lateralview, search, script-values]
timestamp: 2026-07-13T17:00:00+10:00
status: complete
source_mod: romaimperator.construction_manager
source_version: "2.2.11"
---

EU5 lateral views support **search filters** defined as data files. Construction Manager pattern **CM-2**: ship hand-authored filters that mirror automation state and computed metrics (RGO value, food potential, wealth).

Distinct from Glorp’s [trait filter codegen](/tooling/trait-filter-codegen.md) (generated trait triggers).

# Problem

Players need to filter production / raw-goods / town-rights lists by **mod state** (auto-expand on/off) or **computed scores**, not only vanilla fields.

# Files

| Path | Role |
|------|------|
| `in_game/gui/filters/<mod>_location.txt` | `scope = location` |
| `in_game/gui/filters/<mod>_buildings.txt` | `scope = building` |
| `in_game/gui/filters/<mod>_province.txt` | `scope = province` |
| `in_game/common/script_values/…` | Metrics used in range triggers |
| `main_menu/localization/<lang>/<mod>_filters_l_<lang>.yml` | `search_filter_<id>_name/desc/format` |

# Schema (CM examples)

**Boolean / exclusive group** (auto-expand on vs off share `group` + `exclusive_group = yes`):

```txt
location_has_auto_expand_rgo = {
	scope = location
	tag = raw_goods
	group = 7
	exclusive_group = yes
	trigger = {
		OR = {
			exists = var:cm_auto_expand_rgo
			AND = {
				owner = { exists = var:cm_mass_auto_expand_rgo }
				NOT = { exists = var:cm_auto_expand_rgo_excluded }
			}
		}
	}
}
```

**Range slider** (uses `scope:min_value` / `scope:max_value` from UI):

```txt
cm_location_wealth = {
	scope = location
	tag = building|town_rights
	range = {
		min = 0
		max = 100
		step = 1
		format = search_filter_cm_location_wealth_format
	}
	trigger = {
		location_tax_base >= scope:min_value
		location_tax_base <= scope:max_value
	}
}
```

| Field | Meaning |
|-------|---------|
| `scope` | Entity the filter evaluates |
| `tag` | Which filter UI groups show it (`raw_goods`, `building`, `town_rights`, …); `|` = multiple |
| `group` + `exclusive_group` | Radio-style mutual exclusion |
| `trigger` | Script; root = filtered entity; comments note `scope:target` = player country |
| `range` | Slider bounds + loc format key |

# Loc keys

- `search_filter_<filter_id>_name`
- `search_filter_<filter_id>_desc`
- `search_filter_<filter_id>_format` (for ranges)

# Reusable lesson

Keep **filter triggers identical to automation eligibility** (same vars / scripted triggers). Players trust filters that match what the mod actually does. Pair range filters with shared script_values also used by [map modes](/gui/custom-map-modes.md).

# Related

* [Mass opt-in / opt-out flags](/gui/mass-opt-in-opt-out-location-flags.md)
* [Expand raw goods lateral view](/gui/expand-raw-goods-lateralview.md)
* [Trait filter codegen](/tooling/trait-filter-codegen.md) — different pipeline

# Citations

[1] CM `in_game/gui/filters/cm_location.txt`, `cm_buildings.txt`, `cm_province.txt`
[2] CM `main_menu/localization/english/cm_filters_l_english.yml`
[3] CM `in_game/common/script_values/cm_filter_script_values.txt`
