---
type: Playbook
title: Subject type overrides
description: Patch vanilla subject_types for loyalty, diplomacy limits, overlord building rights, and annexation pacing without new subject IDs.
tags: [governments, subject-types, diplomacy, vassals, replace]
timestamp: 2026-07-20T21:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1"
---

EU5 subject relationships (vassal, march, tributary, colonial nation, …) live in `in_game/common/subject_types/`. Total conversions often need **per-type behavioral patches** — not new subject types for every tweak.

MnT ships 11 override files (vassal, march, fiefdom, appanage, tributary, colonial_nation, conquistador, uc_bey, D008_pronoia, …). Pattern: **same file name as vanilla**, edit fields in place (EU5 merge replaces the definition).

# High-value fields

| Field group | Modding use |
|-------------|-------------|
| `strength_vs_overlord` | Swarm nerf / march buff (MnT: vassal `-1` from vanilla `-0.5`) |
| `annexation_*` | Speed, min years, opinion gates |
| `has_limited_diplomacy` / `allow_declaring_wars` | Subject autonomy |
| `can_overlord_build_*` | Roads, buildings, RGOs on subject land |
| `overlord_modifier` / `subject_modifier` | Stability cost, cabinet efficiency |
| `diplo_chance_accept_subject` / `_overlord` | AI offer weights (many scalar biases) |
| `join_offensive_wars_*` / `join_defensive_wars_*` | War participation rules |
| `visible` / `creation_visible` | Hide types behind advances or situations |

# Example — vassal loyalty pressure

```txt
vassal = {
	strength_vs_overlord = -1   # M&T from -0.5

	overlord_modifier = {
		stability_cost_efficiency = -0.04
		monthly_towards_decentralization = societal_value_tiny_monthly_move
	}

	subject_modifier = {
		country_cabinet_efficiency = -0.20
	}

	ai_wants_to_be_overlord = {
		scope:subject = {
			add = {
				desc = "COALITION_GRADE_ANTAGONISM_TOWARDS_THEM_SUBJECT"
				value = { every_country_with_coalition_grade_antagonism_against_us = { add = -10 } }
			}
		}
	}
}
```

# Workflow

1. Copy vanilla `subject_types/<name>.txt` from game files into your mod.
2. Change only fields you need; keep `subject_pays`, `level`, and color unless intentionally redesigning.
3. Grep `subject_type =` across your mod for hard-coded assumptions.
4. Test: subject offer UI, independence wars, market destruction rights (`overlord_can_destroy_markets` referenced from [REPLACE generic actions](/interactions/replace-generic-actions.md)).

# Related

* [Soft-disable vanilla systems](/total-conversion/soft-disable-vanilla-systems.md) — `uc_bey`, colonial nation gates
* [Societal values and estate power](/governments/societal-values-estate-power.md)
* [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md)

# Citations

[1] MnT-EU5 `in_game/common/subject_types/vassal.txt`
[2] MnT-EU5 `in_game/common/subject_types/march.txt`, `fiefdom.txt`, `tributary.txt`
[3] MnT-EU5 `Documentation/Change log.md` — vassal swarm nerf intent
