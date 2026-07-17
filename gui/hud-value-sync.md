---
type: Playbook
title: HUD value sync
description: Monthly aggregate country variables for HUD display when the GUI datamodel lacks the metric.
tags: [gui, on-actions, variables, hud, glorp]
timestamp: 2026-07-11T13:30:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#3**: engine GUI often cannot show “average X across locations.” Compute into country variables on a pulse; HUD binds to those vars.

# Effect

```txt
glorpui_sync_avg_stats = {
	if = {
		limit = { num_locations >= 1 }
		set_variable = { name = glorpui_avg_proximity value = 0 }
		set_variable = { name = glorpui_avg_max_control value = 0 }
		every_owned_location = {
			root = {
				change_variable = { name = glorpui_avg_proximity add = prev.proximity }
				change_variable = { name = glorpui_avg_max_control add = prev.max_control }
			}
		}
		change_variable = { name = glorpui_avg_proximity divide = num_locations }
		change_variable = { name = glorpui_avg_proximity divide = 100 }
		change_variable = { name = glorpui_avg_max_control divide = num_locations }
	}
}
```

# Hooks

- `monthly_country_pulse` → human-only sync
- Init: `on_game_start_after_lobby_human_country` / `on_game_load_after_lobby_human_country` (CMF hooks)

Optional: scripted GUI `RefreshAvgStats` for on-hover refresh.

# GUI bind

```txt
[Player.MakeScope.GetVariable('glorpui_avg_proximity').GetValue|2%]
```

Suppress unused-var warnings with [`cmf_suppress`](/community-mod-framework/utility-triggers-and-effects.md) (prefer over Glorp’s unused dead-branch suppress).

# Related

* [Pulse orchestration](/on-actions/pulse-orchestration.md)
* [CMF registration hooks](/community-mod-framework/registration-and-on-action-hooks.md)

# Citations

[1] Glorp `in_game/common/scripted_effects/glorpui_value_sync_effects.txt`
[2] Glorp `in_game/common/on_action/glorpui_value_sync_on_actions.txt`
[3] Glorp `in_game/gui/glorp.gui`
