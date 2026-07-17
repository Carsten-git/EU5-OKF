---
type: Reference
title: Formable countries overview
description: Structure of formable_countries entries in EU5 — levels, geography, identity fields, and game rules.
tags: [formables, formable_countries, overview]
timestamp: 2026-07-06T08:42:00+10:00
resource: game/in_game/common/formable_countries/
status: draft
---

Formables are defined in `in_game/common/formable_countries/*.txt`. Each entry uses an ID suffix `_f` (e.g. `PRU_f`, `ODR_f`) and appears in the Form Country UI when eligible.

Vanilla: single large file `00_formable_countries.txt`. Mods typically add a dedicated file (e.g. `teu_nc_formables.txt`).

# Core structure

```txt
ODR_f = {
	content_priority = 850
	level = 2
	required_locations_fraction = 0.75
	rule = historical
	potential = { has_or_had_tag = TEU }
	allow = { /* click gate */ }
	name = ODR
	flag = ODR
	adjective = ODR_ADJ
	tag = ODR
	color = rgb { 120 20 30 }
	areas = { prussia_area }
	form_effect = { /* on click */ }
}
```

See vanilla `formable_countries/readme.txt` for the commented template.

# Identity fields

| Field | Purpose |
|-------|---------|
| `name` | Loc key for display name (usually matches country tag) |
| `flag` / `adjective` | Loc keys for flag tooltip and adjective |
| `tag` | Country tag switched to on formation |
| `color` | Map color after formation (`map_PRU` or `rgb { … }`) |

# Geographic requirements

One or more of:

```txt
continents = { /* continent keys */ }
sub_continents = { }
regions = { north_german_region baltic_region }
areas = { prussia_area baltic_area }
locations = { location:konigsberg location:gdansk }
```

| Field | Default | Notes |
|-------|---------|-------|
| `required_locations_fraction` | `1.0` | Share of listed locations you must own |
| `capital_required` | no | Capital must be in the required set |
| `potential_requires_own` | yes | Must own at least one required location to see the formable |

Progress UI uses keys like `FORM_COUNTRY_LOCATION_PROGRESS_*` from `economy_l_english.yml`.

# Level and game rules

| Field | Purpose |
|-------|---------|
| `level` | Formation tier — players can only form countries of **higher** level than their current formable tier; AI same rule |
| `rule` | `historical` / `plausible` / `fantasy` — filtered by game rule `rule_ahistorical_formable_countries` |
| `content_priority` | Sort order in UI (higher = listed earlier) |

## Presenting branching paths in the list (KI-054)

The Form Country list is a flat, priority-sorted list — it has no concept of paths or tiers. If a mod has a branching tree, encode the structure yourself:

1. **Group `content_priority` by path, then step** (descending), so each path reads top-to-bottom: e.g. ODR 850 → HPR 849 → HPE 848 (Catholic), BDM 846 → ENR 845 (Baltic), PRL 843 → BLC 842 (Commerce).
2. **Put path/step labels in the button name**: `"Form Holy Prussia (Catholic Path — Step 2)"`.
3. **Explain the route in `_f_desc`**: which tag it is reached from and which paths it opens.
4. **Tag-gate tooltips in `allow`** double as "who can form this" documentation ("Playing as the #Y Orderstaat#!.").

Vanilla **Prussia** (`PRU_f`): `level = 2`, `rule = historical`, `required_locations_fraction = 0.75`, `areas = { prussia_area }`.

Mod **Orderstaat** (`ODR_f`): same level/fraction pattern, TEU-specific `potential` and custom `allow` logic.

# What formation does by default

Clicking Form runs `form_effect` then the engine applies tag/name/flag/color from the definition. Empty `form_effect = { }` is valid (vanilla `POM_f`-style entries).

Typical custom effects:

- `change_government_type` + `add_reform`
- `set_country_rank_effect`
- `add_prestige`, `add_country_modifier`
- `trigger_event_silently`
- `set_variable` for mission/formable chains
- Location integration (`change_integration_level`)

# See also

* [Formable triggers and effects](formable-triggers-and-effects.md)
* [Formable localization](formable-localization.md)
* [Government types](/governments/government-types.md)
* [Estates and reforms](/governments/estates-and-reforms.md)

# Citations

[1] Vanilla: `game/in_game/common/formable_countries/00_formable_countries.txt` — `PRU_f`, geographic fields, `level`, `rule`, `form_effect`
[2] Vanilla: `game/in_game/common/formable_countries/readme.txt` — commented field template (`level`, `required_locations_fraction`, `potential_requires_own`, …)
[3] Mod: `mod/northern_crusade_teu/in_game/common/formable_countries/teu_nc_formables.txt` — `ODR_f` and branch formables
[4] Vanilla: `game/main_menu/localization/english/economy_l_english.yml` — `FORM_COUNTRY_*` progress UI keys
[5] Vanilla: `game/main_menu/localization/english/game_rules_l_english.yml` — `rule_ahistorical_formable_countries`
