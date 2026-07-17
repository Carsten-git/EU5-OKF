---
type: Playbook
title: Advance-gated RGO conversion unlocks
description: Continent advances that unlock convert-to goods via has_advance in location allow triggers.
tags: [advances, rgo, localization, scripted-triggers]
timestamp: 2026-07-12T10:00:00+10:00
status: complete
source_mod: rgo_conversion
source_version: "0.2.0"
---

# Pattern

There is **no** native `unlock_rgo` advance field. Gate conversion with:

```txt
owner ?= { has_advance = my_advance_id }
```

inside location-scoped allow triggers, plus family/topography and continent macros.

Split concerns:

| Concern | Mechanism |
|---------|-----------|
| Who can research | Advance `potential` (`original_capital` continent / culture / tag) |
| Where convert applies | Location macros (`rgo_conv_unlock_*`) + family/topography |
| Which good | Per-good `rgo_conv_allows_<good>` OR-blocks per advance slot |

# Advance block checklist

* `age`, `potential`, `requires` — prefer [free age roots](age-roots-and-institution-gates.md) when the unlock should not wait on institutions
* `starting_technology_level` above typical country start — [starting technology level](starting-technology-level.md)
* `research_cost` — relative to base: UI ≈ `25 × (1 + research_cost)` — [research cost scaling](research-cost-scaling.md) (`-0.8` ≈ 5 UI; `0.2` ≈ 30; `5` ≈ 150)
* `can_extract_*` when vanilla gates that good — [can_extract gates](can-extract-goods-gates.md)
* Loc: `<id>` + `<id>_desc` (UTF-8 BOM)

# Example (Europe Traditions)

```txt
rgo_conv_eu_stud_farms = {
	age = age_1_traditions
	research_cost = 0            # same as typical age peers (omit or 0; NOT 2.0)
	starting_technology_level = 4
	content_priority = 860
	depth = 0                    # free age root — visible in age tab
	potential = {
		original_capital ?= { continent = continent:europe }
	}
	can_extract_horses = yes     # every unlock good needs a visible extract/effect
}
```

Put **`can_extract_*` for every good the advance unlocks** (even if auto_modifiers already grant it) so the advance tooltip is not blank. Prefer `depth = 0` over burying under Public Health roots — [age roots](age-roots-and-institution-gates.md).

For a **temporary** cheap test node only, use `research_cost = -0.8` (~5 UI) — do not ship that.
# Pitfalls

* Duplicate loc keys break option text ([KI-069](/validation/known-issues.md))
* Empty timed modifiers hide Until date ([KI-070](/validation/known-issues.md))
* Script/loc need UTF-8 BOM ([KI-060](/validation/known-issues.md))
* Confusing `research_cost` with UI points ([KI-071](/validation/known-issues.md))
* Continent **potential** vs location **unlock macros** must both be set

# Product design pointer

Slot tables (25 advances, 1–3 family slots each): development OKF `05_Design Documents/advances-unlocks.md` and mod `README.md` (not duplicated here).

# Citations

[1] Development OKF `05_Design Documents/advances-unlocks.md`
[2] Mod `rgo_conversion/in_game/common/advances/rgo_conv_europe.txt` — Stud Farms test node
[3] Vanilla `can_extract_*` on `0_age_of_traditions.txt` mining/ranching advances
[4] Defines `BASE_RESEARCH_COST = 25`
