---
type: Reference
title: Mission rewards and effects
description: on_start, on_completion, on_monthly, modifiers, events, and scripted effects in mission trees.
resource: mod/northern_crusade_teu/in_game/common/missions/teu_nc_teu_missions.txt
tags: [missions, effects, rewards]
timestamp: 2026-07-06T00:00:00+10:00
status: complete
---

Mission and task blocks use the same **effect** syntax as events and on-actions. Rewards belong in `on_completion`; setup and cleanup use `on_start`, `on_abort`, and mission-level `on_completion`.

# Mission lifecycle effects

| Hook | Typical use |
|------|-------------|
| `on_potential` | Seed scopes/variables when mission enters the pool |
| `on_start` | Set dynamic targets (variables, saved scopes) scaled to country state |
| `on_abort` | Cooldown variables, remove temp variables |
| `on_completion` | Mark mission done, long cooldown, cleanup |
| `on_post_completion` | Runs after mission leaves active state |

Generic trade mission `on_start` computes thresholds from current buildings and estate power, stored as country variables consumed by task `enabled` checks. `on_abort` / `on_completion` remove those variables and set a "recently had" cooldown.

# Task lifecycle effects

| Hook | Runs when | Notes |
|------|-----------|-------|
| `on_start` | Timed task begins | Skipped if `task_rewards_disabled` in rules |
| `on_persistent_start` | Timed task begins | Always runs |
| `on_monthly` | Each month while timed task active | Common for random event chains |
| `on_completion` | Task completes | Skipped if `task_rewards_disabled` |
| `on_persistent_completion` | Task completes | Always runs |
| `on_bypass` | Task bypassed | Optional consolation or cleanup |

Instant tasks (`duration = 0`) fire `on_completion` as soon as `enabled` becomes true.

# Reward patterns

## Direct country effects

Common vanilla rewards:

```txt
on_completion = {
	add_prestige = prestige_mild_bonus
	add_army_tradition = army_tradition_weak_bonus
	add_country_modifier = {
		modifier = consolidated_trade_modifier
		years = modifier_duration_years_normal
		mode = replace
	}
	change_societal_value = {
		type = mercantilism_vs_free_trade
		value = societal_value_move_to_right
	}
}
```

See [Static modifiers](/modifiers/static-modifiers.md) for modifier naming and localization.

## Province and location effects

```txt
on_completion = {
	scope:conquest_province ?= {
		add_province_modifier = {
			modifier = mission_defensive_fortification_modifier
			years = modifier_duration_years_long
			mode = replace
		}
	}
}
```

Use `?=` safe scope assignment when the scope may be missing (player bypassed selection).

## Character effects

Vanilla conquest pack buffs a general on task completion via `ordered_character` — useful template for picking the best eligible character.

## Progress modifiers during timed tasks

```txt
modifier_while_progressing = { global_integration_speed_modifier = 0.1 }
```

Active only while the timed bar runs.

# Linking missions to events

`on_monthly` + `random_list` drives mission event chains without polluting global on-actions:

```txt
on_monthly = {
	random_list = {
		10 = { trigger_event_non_silently = conquest_mission_events.1 }
		10 = { trigger_event_non_silently = conquest_mission_events.2 }
		70 = { }
	}
}
on_start = {
	custom_tooltip = monthly_events_about_war_chest_tt
}
```

Event namespaces for generic missions: `conquest_mission_events`, files under `main_menu/localization/english/missions/generic_conquest_mission_events_l_english.yml`.

Use [trigger_event_non_silently](/events/triggering-events.md) so the popup appears during mission play.

# Tooltips vs hidden effects

Show the player a reward summary; apply heavy logic invisibly:

```txt
on_completion = {
	custom_tooltip = every_pop_conquest_location_var_province_10_sat_tt
	hidden_effect = {
		scope:conquest_province ?= {
			every_location_in_province = {
				every_pop = {
					limit = { owner = root }
					add_pop_satisfaction = pop_satisfaction_mild_bonus
				}
			}
		}
	}
}
```

Tooltip-only tasks (no mechanical reward):

```txt
on_completion = {
	custom_tooltip = teu_nc_teu_a5_face_grunwald_tt
}
```

# Scripted effects

Mod missions call reusable blocks from `in_game/common/scripted_effects/`:

```txt
on_completion = {
	teu_nc_change_purpose = { value = 5 }
	set_variable = { name = teu_nc_samogitia_pacified value = 1 }
}
```

Define parameterized effects once; invoke from many tasks. See [Scripted effect basics](/scripted-effects/scripted-effect-basics.md).

# Variables as mission state

| Pattern | Example |
|---------|---------|
| Cooldown after complete/abort | `recently_had_generic_trade_variable` for 10–25 years |
| One-time country mission | `teu_nc_last_crusade_mission_done` on `on_completion` |
| Dynamic thresholds | `target_navy_size_percentage_variable` set in `on_start`, read in `enabled` |
| Cross-system flags | `teu_nc_tannenberg_victory` set by events, read by mission tasks |

Clean up temp variables in `on_abort` and `on_completion` to avoid stale scope.

# Examples

| Reward type | Where to read |
|-------------|---------------|
| Variable scaling in `on_start` | `generic_trade` mission block, `generic_trade_mission_pack.txt` |
| Monthly mission events | `mission_war_chest`, `mission_declare_war`, `mission_peace_integrate_province` |
| `select_trigger` payoff | `mission_conquer_province` `on_completion` |
| Scripted effect + modifier | `teu_nc_hpr_a5_sword_of_christendom` in `teu_nc_branch_missions.txt` |
| Permanent modifier (`years = -1`) | Several Northern Crusade final tasks |

# Citations

Mod final-task reward calling scripted flow and permanent modifier:

```411:422:C:\Users\Carst\Documents\Paradox Interactive\Europa Universalis V\mod\northern_crusade_teu\in_game\common\missions\teu_nc_branch_missions.txt
	teu_nc_hpr_a5_sword_of_christendom = {
		icon = medieval_military
		requires = { teu_nc_hpr_a5_elite_reiter }
		final = yes
		enabled = {
			is_great_power = yes
			teu_nc_has_steadfast_purpose = yes
		}
		duration = 0
		on_completion = {
			add_country_modifier = { modifier = teu_nc_sword_of_catholicism years = -1 }
		}
	}
```

# See also

* [Mission trees overview](mission-trees-overview.md)
* [Mission tasks and triggers](mission-tasks-and-triggers.md)
* [Scripted effect basics](/scripted-effects/scripted-effect-basics.md)
* [Variables and monthly mechanics](/scripted-effects/variables-and-monthly-mechanics.md)
* [Triggering events](/events/triggering-events.md)
* [Static modifiers](/modifiers/static-modifiers.md)
