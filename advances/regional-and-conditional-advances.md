---
type: Reference
title: Regional and conditional advances
description: How vanilla gates advances by country, culture, religion, geography, and age focus — plus mod patterns for regional trees.
resource: game/in_game/common/advances/region_indonesia.txt
tags: [advances, potential, culture, religion, region, age-focus]
timestamp: 2026-07-19T14:15:00+10:00
status: complete
---

Vanilla does **not** assign special advances at runtime via scripts. Every advance is a static definition in `in_game/common/advances/*.txt`. The game merges all files at **load time**, then each country builds its research tree by evaluating each advance's `potential` (and later `allow` when the player clicks Research).

# Assignment model

| Mechanism | When it runs | What it controls |
|-----------|--------------|------------------|
| `potential` block | Tree construction (per country) | Whether the advance appears in **that** country's UI at all |
| `allow` block | Player clicks Research | Whether research can **start** (institutions, markets, etc.) |
| `requires` | Tree layout | Prerequisite chain within the visible tree |
| `for = adm` / `dip` / `mil` | Age tab filtering | Which advances belong to the Administrative / Diplomatic / Military focus pool for that age |
| `set_age_preference` | Once per age (`on_new_age`) | Player/AI picks adm/dip/mil focus — **not** a separate advance file |

There is no CSV or database loaded mid-campaign for advances. Mod codegen CSVs (e.g. REQ-012) are **dev-time only**; the game reads generated `.txt` files.

# Country tag

File pattern: `country_<TAG>.txt`. Use `has_or_had_tag` so formables keep the tree:

```txt
teu_crusader_discipline = {
	age = age_1_traditions
	potential = {
		has_or_had_tag = TEU
	}
	requires = feudalism_advance
	discipline = 0.05
}
```

See [Country-specific advances](country-specific-advances.md).

# Culture

File patterns: `culture_<name>.txt`, `culture_group_<name>.txt`, `region_<name>.txt` (culture mixes).

**Single culture:**

```txt
potential = {
	culture = culture:thai_culture
}
```

**Culture group (common for shared Iberian/German pools):**

```txt
potential = {
	culture = { has_culture_group = culture_group:iberian_group }
}
```

**Merged-culture safety** (Thai example — survives culture mergers):

```txt
potential = {
	OR = {
		culture = culture:thai_culture
		culture = { merged_culture_group_contains_culture = culture:thai_culture }
	}
}
```

**Tag + culture combo** (Catalan galley — ARA tag OR catalan culture OR iberian group with accepted culture):

```txt
potential = {
	OR = {
		has_or_had_tag = ARA
		culture = culture:catalan
		AND = {
			culture = { has_culture_group = culture_group:iberian_group }
			has_primary_or_accepted_culture = culture:catalan
		}
	}
}
```

# Religion

File pattern: `religion_<name>.txt`.

```txt
orthodox_synod = {
	age = age_2_renaissance
	potential = { religion = religion:orthodox }
	requires = some_global_advance
	# modifiers…
}
```

Religion can also appear inside `potential` on global unlock packs:

```txt
potential = {
	religion = religion:sanjiao
}
```

Religion **group** (Christian buildings example):

```txt
potential = {
	religion.group = religion_group:christian
}
```

Religion gates **visibility**, not conversion — changing religion later does not remove already researched advances.

# Geography (`original_capital`)

Vanilla uses `original_capital` with `?=` (safe scope) for regional trees — **the REQ-012 pattern**:

```txt
indonesia_outer_islands_archipelago = {
	age = age_1_traditions
	potential = {
		original_capital ?= { region = region:indonesia_region }
	}
	requires = guilds
	sea_cost_on_distance_from_capital_when_maritime = -0.5
}
```

Other geographic scopes seen in advances:

| Scope | Example |
|-------|---------|
| `region` | `region:indonesia_region` |
| `continent` | `continent:europe` (mod RGO continent trees) |
| `sub_continent` | `sub_continent:east_asia` |

`original_capital` is fixed at game start — moving the capital does **not** change which regional tree a country sees.

# Age Administrative / Diplomatic / Military focus

This is **separate** from law policies named `administrative_focus` / `diplomatic_focus` in `common/laws/` (those are event-gated reforms, e.g. flavor_MOS).

**New-age focus pick:**

1. `on_new_age` in `common/on_action/_hardcoded.txt` fires for every active country.
2. Event `ages_of_eu.1` (`events/ages.txt`, `type = age_event`) presents three options.
3. Each option calls `set_age_preference = adm` / `dip` / `mil`.
4. Advances in `4_choices_adm.txt`, `4_choices_dip.txt`, `4_choices_mil.txt` carry `for = adm` / `dip` / `mil` to mark which focus pool they belong to.

```txt
innovativeness = {
	age = age_2_renaissance
	for = adm
	requires = pound_lock_canals_advance
	global_max_literacy = 5
}
```

AI can weight options (`ai_will_select` on the Ottoman adm option in `ages_of_eu.1`). Human players see localized "Administrative Focus" / "Diplomatic Focus" / "Military Focus" (`advances_l_english.yml`).

**Mod takeaway:** regional RGO advances do **not** use `for = adm/dip/mil` unless you intentionally tie them to age-focus pools. REQ-012 regional slots use `age` + `potential` only.

# Mod pattern: RGO conversion regional tree (REQ-012)

Combines geography, game rule, and REQ-011 research gate:

```txt
rgo_conv_hist_north_german_region_blast_furnaces = {
	age = age_2_renaissance
	potential = {
		rgo_conv_research_enabled = yes
		has_game_rule = rgo_conv_mode_historical
		original_capital ?= { region = region:north_german_region }
	}
	can_extract_iron = yes
}
```

Unlocking convert-to goods still uses `has_advance` in **location** scripted triggers — see [Advance-gated RGO unlocks](advance-gated-rgo-unlocks.md).

# Culture-tree parent

An Age of Traditions advance with no `requires` and no `depth = 0` is not in the regional tree. It shows up on a common root. Meritocracy (`meritocracy_advance` in `0_age_of_traditions.txt`) is one of those roots: `depth = 0` and `allow = { has_embraced_institution = institution:meritocracy }`, so the advance stays locked until that institution spreads.

The Mesoamerican culture tree starts at `system_of_tributaries` (`region_north_america.txt`): `depth = 0`, `potential = { is_capital_mesoamerica = yes }`, `starting_technology_level = 4`. Pyramid Architecture and War Bonfires are its direct children. A new advance that should sit near the top of that tree uses the same potential and `requires = system_of_tributaries`. Set `starting_technology_level = 4` as well. Advanced Mesoamerican monarchies start at technology level 2, and an advance with no level can already be researched on day one.

`requires` is resolved while that file is read. Files in `common/advances/` load in filename order, and a key in a later file does not exist yet. Vanilla never points `requires` at an advance defined in a later file. `meso_canoes.txt` sorts before `region_north_america.txt`, so `requires = system_of_tributaries` fails (`Failed to read key reference`), the line is dropped, and the advance has no parent. A parentless Age of Traditions advance sits on a common root, which is Meritocracy. Name the file so it sorts after the parent’s file, for example `region_z_meso_canoes.txt`.

# File naming cheat sheet

| Vanilla file | Gates on |
|--------------|----------|
| `country_TEU.txt` | Tag |
| `culture_thai.txt` | Culture |
| `culture_group_german.txt` | Culture group |
| `religion_orthodox.txt` | Religion |
| `region_indonesia.txt` | `original_capital` region |
| `region_iberia.txt` | Culture group + tag mixes |
| `4_choices_adm.txt` | `for = adm` (age focus pool) |

# See also

* [Advance triggers and modifiers](advance-triggers-and-modifiers.md) — `potential` vs `allow`
* [Country-specific advances](country-specific-advances.md) — tag pattern
* [Advance file structure](advance-file-structure.md) — fields and file layout
* [Advance-gated RGO unlocks](advance-gated-rgo-unlocks.md) — `has_advance` on locations

# Citations

[1] `game/in_game/common/advances/region_indonesia.txt` — `original_capital` + region
[2] `game/in_game/common/advances/culture_thai.txt` — culture + merged group
[3] `game/in_game/common/advances/region_iberia.txt` — culture group + tag OR
[4] `game/in_game/common/advances/religion_orthodox.txt` — religion potential
[5] `game/in_game/events/ages.txt` — `ages_of_eu.1`, `set_age_preference`
[6] `game/in_game/common/on_action/_hardcoded.txt` — `on_new_age`
[7] `game/in_game/common/advances/4_choices_adm.txt` — `for = adm`
