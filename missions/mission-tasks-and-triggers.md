---
type: Reference
title: Mission tasks and triggers
description: visible, enabled, bypass, abort, duration, select_trigger, and completion patterns for mission tasks.
resource: game/in_game/common/missions/generic_conquer_province_mission_pack.txt
tags: [missions, triggers, tasks]
timestamp: 2026-07-06T00:00:00+10:00
status: complete
---

Mission UI visibility and completion are entirely trigger-driven. The same trigger syntax used in [events](/events/event-file-basics.md) applies here; scope is typically the **country** (`ROOT`) unless a `select_trigger` or `highlight` introduces a province/market scope.

# Mission picker triggers

| Block | When evaluated | Effect |
|-------|----------------|--------|
| `visible` | Rolling potential missions | Mission appears as a selectable card |
| `enabled` | Player clicks Start | Mission can begin; if false, start is blocked |
| `abort` | Each tick while active | Mission cancels — runs `on_abort`, drops progress |
| `chance` | Weighting among visible missions | Higher = more likely in the limited slot pool |

Always include `game_has_missions_enabled = yes` in `visible` when missions are optional in game rules.

Cooldown pattern (generic packs):

```txt
visible = {
	game_has_missions_enabled = yes
	NOT = { has_variable = recently_had_generic_conquer_province_variable }
}
```

# Task triggers

| Block | Role |
|-------|------|
| `visible` | Hide tasks until conditions met (e.g. age-gated branch) |
| `enabled` | **Completion condition** — when true, task completes (instant) or timed bar finishes |
| `bypass` | When true, task skips without normal player action; use for "already done" shortcuts |
| `highlight` | Map provinces matching trigger while hovering the task row |

There is no separate `completed` block — **`enabled` is the completion check**.

## Instant vs timed tasks

```txt
duration = 0    # Instant: completes the moment enabled is true
duration = 365  # Timed: progress bar runs 365 days; enabled must stay true
```

Timed tasks often pair `enabled = { }` (empty or minimal) with `modifier_while_progressing` and `on_monthly` event pulses. The conquest pack's `mission_war_chest` runs 365 days with monthly random events while `global_estate_max_tax` applies.

## Bypass for skip-ahead

When the player already satisfies a later goal, bypass avoids dead ends:

```txt
mission_army = {
	enabled = { army_size_percentage > 0.75 /* ... */ }
	bypass = {
		any_current_war = {
			OR = {
				AND = { attacker_leader = root defender_leader = { is_neighbor_of = root } }
				AND = { defender_leader = root attacker_leader = { is_neighbor_of = root } }
			}
		}
	}
	duration = 0
}
```

If the country is already at war with a neighbor, the army task auto-resolves via bypass.

# Conditional requirements with scopes

Tasks that depend on a player-chosen province use `trigger_if` / `trigger_else` and `scope:conquest_province`:

```txt
enabled = {
	trigger_if = {
		limit = { exists = scope:conquest_province }
		scope:conquest_province = { province_prosperity > 0 }
	}
	trigger_else = {
		custom_tooltip = {
			text = requirements_after_mission_conquer_province_tt
			always = no
		}
	}
}
```

The `custom_tooltip` + `always = no` pattern shows a readable requirement in the UI while keeping the trigger false until the scope exists.

# select_trigger — player picks a target

Used when later tasks need a saved scope (province, market, good, urban location):

```txt
select_trigger = {
	looking_for_a = province
	source = actor
	target_flag = conquest_province
	name = "select_mission_for_next_mission_tasks"
	none_available_msg_key = "integrate_province_no_provinces"
	column = { data = name }
	column = { data = integration }
	column = { data = population }
	visible = {
		any_location_in_province = {
			integration_level = conquered
			modifier:local_separatism > 0
		}
	}
}
```

| Field | Purpose |
|-------|---------|
| `looking_for_a` | Scope type: `province`, `market`, `good`, etc. |
| `target_flag` | Saved as `scope:<target_flag>` for downstream tasks |
| `name` | Loc key for the selection dialog title |
| `none_available_msg_key` | Shown when no valid targets |
| `column` | UI columns in the picker |
| `visible` | Filter list entries |

Localization for shared picker strings lives in `generic_missions_l_english.yml` (`mission_select_market`, `select_mission_for_next_mission_tasks`, …).

# Task-level visible gating

Hide branches until the game reaches a certain age:

```txt
mission_winds_of_trade = {
	visible = {
		current_age_or_later = { age = age_4_reformation }
	}
	requires = { mission_build_entrepot }
	enabled = { /* ... */ }
}
```

# Mod triggers — tag, variables, custom triggers

Northern Crusade tasks lean on mod variables and scripted triggers:

```txt
enabled = {
	OR = {
		has_variable = teu_nc_tannenberg_victory
		has_variable = teu_nc_tannenberg_marginal
	}
	NOT = { has_variable = teu_nc_tannenberg_defeat }
}
```

```txt
enabled = { teu_nc_has_steadfast_purpose = yes }
```

Branch missions abort when the player tags into a different path:

```txt
abort = {
	OR = { tag = HPR tag = BDM tag = PRL tag = HPE tag = ENR tag = BLC }
}
```

# Examples

| Pattern | Vanilla reference |
|---------|-------------------|
| `select_trigger` + scoped follow-ups | `mission_conquer_province` in `generic_conquer_province_mission_pack.txt` |
| Scaled `enabled` vs variables set in `on_start` | `mission_establish_wharfs` in `generic_trade_mission_pack.txt` |
| `bypass` when ports irrelevant | `mission_merchant_fleet` — `bypass = { has_ports = no }` |
| Tag + variable gating | `teu_nc_mission_last_crusade` in `teu_nc_teu_missions.txt` |

# Citations

Timed task with empty `enabled` and monthly pulse (vanilla):

```82:101:c:\Program Files (x86)\Steam\steamapps\common\Europa Universalis V\game\in_game\common\missions\generic_conquer_province_mission_pack.txt
	mission_war_chest = {
		icon = court_accounting
		enabled = {	}

		duration = 365

		modifier_while_progressing = { global_estate_max_tax = 0.1 }

		on_monthly = {
			random_list = {
				10 = { trigger_event_non_silently = conquest_mission_events.1 }
				...
			}
		}
	}
```

# See also

* [Mission trees overview](mission-trees-overview.md)
* [Mission rewards and effects](mission-rewards-and-effects.md)
* [Triggering events](/events/triggering-events.md)
* [Variables and monthly mechanics](/scripted-effects/variables-and-monthly-mechanics.md)
* [Common pitfalls](/validation/common-pitfalls.md)
