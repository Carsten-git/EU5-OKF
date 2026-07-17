---
type: Playbook
title: Societal values and estate power
description: REPLACE societal-value axes to shift global_*_estate_power while keeping control_importance for AI.
tags: [societal-values, estates, governments, replace]
timestamp: 2026-07-11T12:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1.6"
---

Societal-value sliders can drive **estate power** directly. Put the heavy lift on the axis; keep reforms as smaller residuals.

# Pattern

```txt
REPLACE:centralization_vs_decentralization = {
	left_modifier = {
		global_crown_estate_power = 0.5
		global_tribes_estate_power = -0.5
		control_importance_modifier = 0.2   # keep for AI
		# …
	}
	right_modifier = {
		global_tribes_estate_power = 0.5
		control_importance_modifier = -0.1
		# …
	}
	opinion_importance_multiplier = 0.5
}
```

# Field families

| Family | Role |
|--------|------|
| `global_*_estate_power` | Direct estate power per slider side |
| `control_importance_modifier` | AI control priority — preserve from vanilla |
| `*_estate_levy_size`, levy recovery | Military side effects |
| `monthly_towards_*` | Usually on reforms/privileges, not the axis file |

# Pair with reforms

```txt
REPLACE:universal_serfdom = {
	societal_values = { serfdom_focus }
	country_modifier = {
		monthly_towards_serfdom = societal_value_monthly_move
		global_crown_estate_power = 0.33
		global_peasants_estate_power = -0.1  # small residual; axis carries the rest
	}
}
```

# Related

* [Government reforms](/governments/government-reforms.md)
* [Override ladder](/total-conversion/three-root-and-override-ladder.md)

# Citations

[1] MnT `in_game/common/societal_values/MnT_00_default.txt`
[2] MnT `in_game/common/government_reforms/MnT_common.txt`
[3] Vanilla `in_game/common/societal_values/00_default.txt`
