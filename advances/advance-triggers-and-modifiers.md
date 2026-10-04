---
type: Reference
title: Advance triggers and modifiers
description: potential vs allow, modifier stat keys on advances, and unlock fields.
resource: game/in_game/common/advances/country_TEU.txt
tags: [advances, triggers, modifiers]
timestamp: 2026-07-06T08:45:00+10:00
status: complete
---

Country advances combine **visibility triggers**, **research gates**, **stat modifiers**, and **unlock effects**. The same trigger/effect syntax used in events applies here; scope is the country researching the advance.

# potential vs allow

| Block | When evaluated | Typical use |
|-------|----------------|-------------|
| `potential` | Tree construction | "This advance belongs to TEU / Hungarian culture / theocracy governments" |
| `allow` | Player clicks Research | "Requires feudalism institution" or "Capital market trades silver" |

```txt
photduang = {
	age = age_4_reformation
	potential = {
		culture = culture:thai_culture
	}
	allow = {
		num_locations > 0
		capital = {
			market = {
				is_traded_in_market = goods:silver
			}
		}
	}
	requires = global_trade_routes_advance
	minting_income_factor = 0.1
}
```

If `potential` is false, the advance **never appears** in that country's tree. If `allow` is false, it appears but shows a blocked tooltip until conditions are met.

Common `potential` patterns:

- `has_or_had_tag = TEU` — survives tag change via formables
- `culture = culture:hungarian` or merged-culture OR blocks
- `culture = { has_culture_group = culture_group:iberian_group }`
- `religion = religion:orthodox` or `religion.group = religion_group:christian`
- `original_capital ?= { region = region:indonesia_region }` — regional tree (REQ-012)
- `government = monarchy` / reform checks

Full vanilla matrix: [Regional and conditional advances](regional-and-conditional-advances.md).

# Modifier stat keys

Any line `stat_key = value` inside an advance block applies that bonus **while the advance is researched**. Keys must exist in vanilla `main_menu/common/modifier_type_definitions/00_modifier_types.txt`.

Examples from TEU advances:

```txt
discipline = 0.05
army_heavy_cavalry_power = 0.1
global_pop_conversion_speed_modifier = 0.2
clergy_estate_target_satisfaction = medium_permanent_target_satisfaction
```

Non-numeric values (estate satisfaction tiers, societal values) are valid when vanilla uses the same pattern — grep vanilla advances before inventing new value shapes.

**Silent failure:** an unknown stat key often does nothing in-game and may not appear in `error.log`. Use [Modifier stat keys](/modifiers/modifier-stat-keys.md) or `validate_mod.py` to catch typos.

# Unlock fields (non-modifier children)

These are **not** modifier keys — skip them when scanning for stat validation:

```
age, content_priority, potential, allow, requires, icon,
research_cost, depth, starting_technology_level, for, government,
unlock_*, allow_*, may_*, can_*, has_*, enable_*
```

Unlock examples from global advances:

```txt
unlock_law = cultural_traditions_law
unlock_subject_type = vassal
may_explore = yes
can_colonize = yes
```

# Chaining and ages

- `requires = feudalism_advance` links into the global tree — country advances usually hook an existing global node.
- `age = age_2_renaissance` must match or exceed prerequisite age; mismatched ages still load but look wrong in UI.
- `content_priority` controls ordering among siblings in the same age tab (TEU vanilla uses 200–1100).

# Testing checklist

- [ ] Advance visible for target tag at game start
- [ ] Not pre-researched — check [Starting technology level](starting-technology-level.md)
- [ ] Bonus appears in government modifiers tab or relevant tooltip after research
- [ ] `allow` gate clears when condition met (institution, building, etc.)

# See also

* [Advance file structure](advance-file-structure.md)
* [Country-specific advances](country-specific-advances.md)
* [Modifier stat keys](/modifiers/modifier-stat-keys.md)
* [Common pitfalls](/validation/common-pitfalls.md)

# Citations

[1] `game/in_game/common/advances/culture_thai.txt` — `allow` with market scope
[2] `game/in_game/common/advances/country_TEU.txt` — country modifiers
[3] `northern_crusade_teu/tools/validate_mod.py` — `ADVANCE_SKIP_KEYS` list
