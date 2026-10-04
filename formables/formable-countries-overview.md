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

# Patching one vanilla formable

A new formable goes in its own file as a new key. Changing an existing key, such as `MAY_f`, uses `REPLACE:` in a separate file under `in_game/common/formable_countries/`. That is the same rung as any other top-level object in `common/`: the wiki's [Mod compatibility](https://eu5.paradoxwikis.com/Mod_compatibility) page says most `common/` folders accept `INJECT:` and `REPLACE:` on top-level blocks, and formables are not one of the listed exceptions (GUI types, events, defines, on actions, localization). The override ladder puts `REPLACE:vanilla_key` at rung 3, ahead of a same-path copy of the whole file.

`REPLACE:MAY_f = { … }` replaces that one entry and leaves every other formable in `00_formable_countries.txt`. The new block is the whole definition. Fields you omit are gone, because replace overwrites the object rather than merging one field. Copy `level`, `rule`, `potential`, `name`, `flag`, `adjective`, `tag`, `color`, and `areas`, then change `required_locations_fraction`, `allow`, and `form_effect`.

`INJECT:` is the wrong tool when the field already exists. Vanilla `MAY_f` already has `allow = { }` and `form_effect = { }`. The wiki says injecting a trigger or effect block fails when that block is already defined. A same-path copy of `00_formable_countries.txt` is a full file overwrite: the mod file wins and every other formable in that file stops tracking vanilla patches.

# Counting land the country and its subjects hold

`required_locations_fraction` counts locations the country owns. It does not count subject land. Spain (`SPA_f`) keeps `required_locations_fraction = 0.75` and adds a separate `allow` so that every Iberian location is owned by a Christian country or by someone in the player's overlord chain. The 75% is still the player's own land.

To require half of a set of areas, counting the country and one subject type together:

1. Give the button its own target with `required_locations_fraction = target / set_size` when that chip should show the country's own land. A fraction of `0` is not a second gate, and the chip then reads `owned/0`. Subject land never fills the chip. Count it in `allow`.
2. Define a `scripted_geography` whose `area = { … }` lists those areas. `albanian_migration_source_geography` already lists more than one area.
3. In `allow`, use `any_location_in_scripted_geography` with `percent >= 0.5`. The Holy Roman Empire kingdom-title interaction uses that iterator at 50%. `assign_governor` uses `any_location_in_area` with `percent >= 0.75` and `owner ?= scope:actor`, so the percent can test the owner.
4. The owner test is `owner = root` or `owner = { is_subject_of = root }` plus `is_subject_type`. `is_owned_by_target_or_subject` in `location_triggers.txt` is the same owner-or-subject shape.

The percent tooltip prints those inner triggers against one sample location, the first location in the geography, and it also prints the current percent. Wrap only the inner owner test in `custom_tooltip`. Wrapping the percent iterator itself hides that current percent. See [KI-052](/validation/known-issues.md).

A check on each area separately would demand half of every area, which is stricter than half of the combined locations.

The form-country button still draws `NewCountryCandidate.GetLocationProgress` (`in_game/gui/form_new_country.gui`). The text is `FORM_COUNTRY_LOCATION_PROGRESS_*` in `economy_l_english.yml`: `$PROGRESS$/$TOTAL$`. Progress is locations the country itself owns inside the geographic set (`areas` and `locations` together). Total is `required_locations_fraction` times that set. A fraction of `0` makes the total `0`, so the chip reads `12/0`. An empty set (no areas and no locations) reads `0/0`, and the map highlight is not a real province. `owns = location:` adds a requirement row (“Owns Mayapán”) and does not move the chip or the highlight. A city the country must own is a separate `allow` line, as in Poland (`owns = location:krakow`), Ethiopia (`owns = location:axum`), and Bavaria (`owns = location:munich`). That line does not add or remove areas. Bavaria’s `locations` list is extra land outside `bavaria_area`, not the Munich requirement. The highlight can still name the first location of the earliest included area in `map_data/definitions.txt`. For the six Maya areas that place is Quechula, the first location of `chilapan_area`. Omitting the area changes the button’s land. It is not how those countries name their city. A `locations` list with no areas, at fraction `1`, checks only those names and the chip becomes that count. The chip has no field that counts subject land, and it cannot count “any 12 locations anywhere.” That count is `num_locations >= 12` in `allow` (`NUM_LOCATIONS_TRIGGER`: “We have ≥ 12 locations”). A `locations` list with fraction `1` only checks those named provinces. A target count on a real area set is `required_locations_fraction = target / set_size`. The engine’s rounding of that product is not guaranteed, so a result of 11 or 13 means the fraction needs a nudge.

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
[6] Wiki: [Mod compatibility](https://eu5.paradoxwikis.com/Mod_compatibility) — `REPLACE:` replaces one top-level object; most `common/` folders, formables not an exception
[7] [Override ladder](/total-conversion/three-root-and-override-ladder.md) — `REPLACE:vanilla_key` before a full-file copy
[8] Vanilla `SPA_f` — fraction stays the country's own land; subject ownership is a separate `allow`
[9] Vanilla `assign_governor.txt` — `any_location_in_area` with `percent >= 0.75` and an owner test
[10] Vanilla `country_interactions/hre.txt` `request_kingdom_title` — `any_location_in_scripted_geography` with `percent >= 0.5`
[11] Vanilla `scripted_geography/00_event_scripted_geography.txt` — `albanian_migration_source_geography` lists several areas
