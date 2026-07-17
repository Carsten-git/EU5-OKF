---
type: Reference
title: Mission trees overview
description: File layout, mission blocks, tasks, and dependency structure under in_game/common/missions/.
resource: game/in_game/common/missions/
tags: [missions, structure, syntax]
timestamp: 2026-07-06T00:00:00+10:00
status: complete
---

Mission trees in EU5 are defined as plain-text blocks under `in_game/common/missions/`. Each **mission** is a selectable chain shown in the Missions panel; nested **tasks** are the nodes players complete inside that chain.

Vanilla ships only **generic mission packs** (repeatable, any eligible country). Country-specific trees are added by mods — see the Northern Crusade TEU example below.

# File location

| Layer | Path |
|-------|------|
| Definitions | `in_game/common/missions/*.txt` |
| Schema reference | `in_game/common/missions/____Info.txt` |
| Localization | `main_menu/localization/english/missions/` |

One `.txt` file may contain **multiple mission blocks**. Use descriptive filenames (`generic_trade_mission_pack.txt`, `teu_nc_branch_missions.txt`).

# Mission block structure

The top-level key is the mission ID (also the localization key):

```txt
generic_conquer_province = {
	icon = generic_conquer_province
	repeatable = yes
	player_playstyle = military

	visible = { /* trigger — show in mission picker */ }
	enabled = { /* trigger — country may start this mission */ }
	chance = 3600

	on_start = { /* effects when player selects the mission */ }
	on_completion = { /* effects when the final task completes */ }
	on_abort = { /* effects when mission aborts */ }

	mission_war_chest = { /* task block */ }
	mission_conquer_province = { /* task block */ }
}
```

# Mission-level fields

| Field | Purpose |
|-------|---------|
| `icon` | Sprite key for the mission button and header |
| `header` | Optional header image when mission is active |
| `repeatable` | `yes`/`no` — can the chain be taken again after completion |
| `player_playstyle` | UI hint: `military`, `administrative`, `diplomatic`, etc. |
| `visible` | Trigger — mission appears in the potential list |
| `enabled` | Trigger — country can **start** the mission |
| `abort` | Trigger — mission is cancelled mid-run |
| `chance` | Script value — weight when rolling `POTENTIAL_MISSION_COUNT` slots |
| `ai_will_do` | Script value — AI preference vs other available missions |
| `on_potential` | Effect when mission enters the potential list; mission scope persists until abort/complete |
| `on_start` | Effect when the player starts the mission |
| `on_abort` / `on_completion` / `on_post_completion` | Lifecycle cleanup and rewards |

See [Mission tasks and triggers](mission-tasks-and-triggers.md) for trigger details and [Mission rewards and effects](mission-rewards-and-effects.md) for effect hooks.

# Task blocks

Tasks are nested keys inside the mission block. The task ID doubles as its localization key.

```txt
mission_conquer_province = {
	icon = diplomatic_influence
	requires = { mission_declare_war }
	final = yes
	enabled = { /* completion condition */ }
	duration = 0
	on_completion = { /* reward effects */ }
}
```

| Field | Purpose |
|-------|---------|
| `requires` | List of task IDs that must be done first |
| `final` | `yes` — completing this task finishes the whole mission |
| `visible` | Task hidden from tree when false |
| `enabled` | Condition to **complete** the task (instant) or start a timed bar |
| `bypass` | When true, task auto-completes without reward hooks (see rewards article) |
| `duration` | Days for timed progress; `0` = instant check against `enabled` |
| `highlight` | Provinces to highlight on map hover (`scope:province`) |
| `select_trigger` | Player picks a province, market, good, etc. for scoped follow-up tasks |
| `modifier_while_progressing` | Country modifiers active during timed tasks |
| `on_monthly` | Pulse while a timed task runs |
| `on_start` / `on_completion` | Task lifecycle effects |

# Dependency graph

`requires = { task_a task_b }` means **both** prerequisites must be complete. Omit `requires` (or `requires = { }`) for root tasks available at mission start.

Parallel branches are common: several tasks with no mutual `requires` can progress independently until they converge on a later task.

```
mission_war_chest          mission_obtain_casus_belli
        \                          /
         \    mission_army         /
          \        |             /
           mission_declare_war
                    |
           mission_conquer_province
                    |
        mission_peace_integrate_province  (final)
```

# Generic vs country-specific

**Generic packs** gate on gameplay state, not tags:

```txt
visible = {
	game_has_missions_enabled = yes
	NOT = { has_variable = recently_had_generic_trade_variable }
}
enabled = {
	has_ports = yes
	has_embraced_institution = institution:banking
}
```

**Country-specific trees** (mod pattern) pin to a tag and one-time variables:

```txt
teu_nc_mission_last_crusade = {
	visible = {
		game_has_missions_enabled = yes
		tag = TEU
		NOT = { has_variable = teu_nc_last_crusade_mission_done }
	}
	enabled = { tag = TEU }
	abort = { has_variable = teu_nc_tannenberg_defeat }
	on_completion = {
		set_variable = { name = teu_nc_last_crusade_mission_done value = 1 }
	}
}
```

Branch missions for formable tags (ODR, HPR, PRL, …) typically use `abort` to cancel when the player switches paths.

# Examples

**Vanilla generic conquest pack** — one file, one mission, many tasks with `select_trigger`, timed phases, and event pulses:

* `in_game/common/missions/generic_conquer_province_mission_pack.txt`

**Vanilla generic trade pack** — `on_start` sets scaled variables used in task `enabled` checks:

* `in_game/common/missions/generic_trade_mission_pack.txt`

**Mod country + branch trees** — TEU main tree plus seven path-specific trees in two files:

* `northern_crusade_teu/in_game/common/missions/teu_nc_teu_missions.txt`
* `northern_crusade_teu/in_game/common/missions/teu_nc_branch_missions.txt`

Large mod trees are often generated; Northern Crusade uses `tools/generate_missions.py` alongside `generate_localization.py`.

# Citations

Mission and task field list from vanilla schema stub:

```1:61:c:\Program Files (x86)\Steam\steamapps\common\Europa Universalis V\game\in_game\common\missions\____Info.txt
#test_mission = { # Mission type
#	header = "" # [String] Image to show in the header when the mission has been selected
#	icon = "" # [String] Image to show on the mission selection button and in the header for the active mission
#	repeatable = no [Boolean] (Default no) Whether this mission is repeatable
#	visible = {} # [Trigger] Whether this mission should be displayed
#	enabled = {} # [Trigger] Whether this mission can be completed
#	abort = {} # [Trigger] Whether this mission should be aborted
# ...
#	test_mission_task_2 = { # Task type
#		requires = { test_mission_task_1 } # [{Task type}] Required tasks for starting
#		final = yes # [Boolean] (Default no) Whether completing this task completes the mission
# ...
```

# See also

* [Mission tasks and triggers](mission-tasks-and-triggers.md)
* [Mission rewards and effects](mission-rewards-and-effects.md)
* [Mission localization](mission-localization.md)
* [Vanilla file locations](/references/vanilla-file-locations.md)
* [Mod folder structure](/getting-started/mod-folder-structure.md)
* [Workflow and tools](/getting-started/workflow-and-tools.md)
