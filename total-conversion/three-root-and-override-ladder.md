---
type: Playbook
title: Three-root architecture and override ladder
description: Place content in in_game / main_menu / loading_screen and choose INJECT, REPLACE, or full-file override.
tags: [total-conversion, structure, inject, replace, loading_screen]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Large EU5 mods should mirror vanilla's **three load-phase roots** and escalate overrides only as far as needed.

# Three roots

| Root | Lifecycle | Put here |
|------|-----------|----------|
| `main_menu/` | Bookmark, setup, menu | Static modifiers, game concepts, setup start files, **most localization**, menu GUI |
| `in_game/` | Active session | Events, on_actions, scripted_*, buildings, climates, lateral-view GUI, map modes |
| `loading_screen/` | Boot | Selective `defines`, branded loading UI, version label |

Wrong root → silent miss or menu-only load. See also [Mod folder structure](/getting-started/mod-folder-structure.md).

# Override ladder (prefer lower rungs)

| Rung | Technique | When | Maintenance |
|------|-----------|------|-------------|
| 1 | New keys only | Wholly new content | Low |
| 2 | `INJECT:vanilla_key = { … }` | Patch one field / attach PMs | Low–medium |
| 3 | `REPLACE:vanilla_key = { … }` | Replace a whole named block | Medium |
| 4 | Same-path full file shadow | Must own entire file (GUI, subject type) | High |
| 5 | `replace_path` | Excise vanilla trees | Highest — MnT v0.1.6 uses **none** |

# Examples (MnT)

```txt
# Surgical — attach maintenance PM to vanilla building
INJECT:cathedral = {
	possible_production_methods = { epbm_clergy_maintenance }
}

# Block replace — retune proximity→control ceiling
REPLACE:proximity_to_capital = {
	local_max_control = 0.50  # from 0.75
}

# Defines — only the keys you change (loading_screen/common/defines/)
# Comment the vanilla value next to each edit for future merges.
```

# Full GUI copies

When you must copy a lateral view (economy, production, location window):

1. Mark every insertion with a unique comment (`# EPBM ADDITION`).
2. Expect re-sync on every vanilla UI patch.
3. For **named templates**, use [aaa_ filename precedence](/gui/custom-ui-patterns.md).

# Pulse files

Redefining `on_game_start` / `monthly_country_pulse` in a mod on_action file is powerful and fragile — verify merge vs replace behavior against current vanilla, and keep a single orchestration file (MnT: `MnT_pulse.txt`).

# Citations

[1] [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md)
[2] MnT `loading_screen/common/defines/MnT_Defines.txt` — selective defines
[3] MnT `in_game/common/building_types/epbm_estate_building_maintenance.txt` — INJECT PMs
