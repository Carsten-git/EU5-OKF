---
type: Playbook
title: Character interaction pre-evaluation
description: Score candidates cheaply then fully evaluate only the top N — keep expensive visible checks last.
tags: [character-interactions, performance, ai, diplomacy]
timestamp: 2026-07-11T12:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1.6"
---

Character pickers can stall when `visible` runs expensive checks on hundreds of characters. Use **two-phase pre-evaluation**.

# Pattern

On the candidate `select_trigger`:

```txt
select_trigger = {
	looking_for_a = character
	source = actor
	target_flag = target
	pre_evaluation_sort_value = {
		value = 50
		subtract = age_in_years    # cheaper score first
	}
	pre_evaluation_number_to_evaluate_fully = 10
	visible = {
		# cheap gates first
		NOT = { is_same_gender = scope:recipient }
		# expensive dynasty / graph checks LAST
		OR = { … }
	}
}
```

| Field | Role |
|-------|------|
| `pre_evaluation_sort_value` | Score before full `visible` |
| `pre_evaluation_number_to_evaluate_fully` | How many top scores get full evaluation |

# Related

* Exists in vanilla `marry_noble` — MnT preserves it while rewriting `ai_will_do`
* Same idea appears on some `generic_actions` / `country_interactions`

# Citations

[1] MnT `in_game/common/character_interactions/MnT_marry_noble.txt`
[2] Vanilla `in_game/common/character_interactions/marry_noble.txt`
