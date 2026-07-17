---
type: Reference
title: Advance file structure
description: Where advances live, how files are named, and the fields inside an advance block.
resource: game/in_game/common/advances/
tags: [advances, syntax, structure]
timestamp: 2026-07-06T08:45:00+10:00
status: complete
---

Advances are country research nodes defined as plain-text blocks under `in_game/common/advances/`. Each top-level key is the advance ID (also the localization key).

# Location and naming

| Pattern | Example file | Contents |
|---------|--------------|----------|
| Age buckets | `0_age_of_traditions.txt` | Global advances grouped by age |
| Country tag | `country_TEU.txt`, `country_HUN.txt` | Nation-specific advances |
| Culture / religion / region | `culture_group_german.txt`, `religion_catholic.txt` | Shared unlock pools |
| Unlock packs | `1_building_unlocks.txt`, `2_army_unlocks.txt` | Building and unit gates |
| Mod country pack | `count_TEU_northern_crusade.txt` | Mod convention — any descriptive name works |

Vanilla loads **all** `.txt` files in the folder. Use one file per theme or nation; multiple advance blocks per file is normal.

# Minimal advance block

```txt
teu_crusader_discipline = {
	content_priority = 1100
	age = age_1_traditions
	potential = {
		has_or_had_tag = TEU
	}
	discipline = 0.05
	fort_limit = 1
	requires = feudalism_advance
}
```

# Core fields

| Field | Purpose |
|-------|---------|
| `age` | Which age tab shows the advance (`age_1_traditions`, `age_2_renaissance`, …) |
| `requires` | Prerequisite advance ID(s) — forms the research tree |
| `potential` | Trigger — advance appears in the tree only for matching countries/cultures |
| `allow` | Trigger — must be true to **start** research (institutions, capital checks, etc.) |
| `content_priority` | UI sort weight within the age; higher = listed earlier |
| `icon` | Sprite key for the advance button |
| `research_cost` | Relative cost vs base: UI ≈ `25 × (1 + research_cost)` (age mods may apply). `-0.8` ≈ 5 UI; `5` ≈ 150. Not raw points. |
| `depth` | Tree depth hint for global roots (`depth = 0`) |
| `starting_technology_level` | Game-start unlock threshold — see [Starting technology level](starting-technology-level.md) |

# Effect fields

Advances apply bonuses the same way static modifiers do — any valid **modifier stat key** as a direct child of the block:

```txt
global_pop_conversion_speed_modifier = 0.15
trade_income = 0.05
fort_limit = 1
```

They can also **unlock** mechanics instead of (or in addition to) stat bonuses:

| Prefix / field | Unlocks |
|----------------|---------|
| `unlock_law` | A law the country can codify |
| `unlock_building` | Building type |
| `unlock_unit` / `unlock_levy` | Military units |
| `unlock_government_reform` | Reform slot entry |
| `unlock_casus_belli` | CB type |
| `unlock_country_interaction` | Diplomatic action |
| `may_explore`, `can_colonize`, `allow_subjects` | Boolean capability flags |

Validate stat keys against vanilla — see [Modifier stat keys](/modifiers/modifier-stat-keys.md).

# File organization tips

- **Chain prerequisites** with `requires = parent_advance` so the UI draws a dependency line.
- **Pin country advances** with `potential = { has_or_had_tag = TAG }` so formables and tag switches still see the tree.
- **Set `starting_technology_level`** on mod advances above the country's template level so they are not pre-researched at day 1.
- **Localization**: keys `<advance_id>` and `<advance_id>_desc` in `main_menu/localization/english/advances_l_english.yml`.

# Vanilla references

| Topic | Path |
|-------|------|
| Global age tree + comment on `starting_technology_level` | `in_game/common/advances/0_age_of_traditions.txt` |
| Country-specific TEU | `in_game/common/advances/country_TEU.txt` |
| `allow` with institution gate | `in_game/common/advances/culture_thai.txt` |
| Advance loc keys | `main_menu/localization/english/advances_l_english.yml` |

# See also

* [Advance research cost scaling](research-cost-scaling.md)
* [can_extract goods gates](can-extract-goods-gates.md)
* [Advance age roots and institution gates](age-roots-and-institution-gates.md)
* [Advance triggers and modifiers](advance-triggers-and-modifiers.md)
* [Country-specific advances](country-specific-advances.md)
* [Starting technology level](starting-technology-level.md)
* [Vanilla file locations](/references/vanilla-file-locations.md)

# Citations

[1] `game/in_game/common/advances/0_age_of_traditions.txt` — header comment on `starting_technology_level`
[2] `game/in_game/common/advances/country_TEU.txt`
[3] `northern_crusade_teu/in_game/common/advances/count_TEU_northern_crusade.txt`
[4] `game/loading_screen/common/defines/00_defines.txt` — `BASE_RESEARCH_COST`
