---
type: Playbook
title: Startup world surgery
description: Fix vanilla start state with ordered hidden events — pops, buildings, RGO strip — without editing map history.
tags: [total-conversion, game-start, events, systems-overhaul]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

A **systems overhaul** can reshape the starting world without a map/timeline TC: hidden `on_game_start` events retag pops, place buildings, and disable vanilla systems.

# When to use

- No new countries/provinces/history files.
- Need global demographic or building corrections at 1337 (or bookmark).
- Must run for every country once, silently.

# Pattern

1. Orchestrate from [pulse orchestration](/on-actions/pulse-orchestration.md).
2. One hidden country event per surgery step (or one effect called from start).
3. **Order matters** — document the contract (e.g. strip slaves → seed tribes → food buildings → RGO removal → UI hide lists).
4. After mass building creation, short-lived extreme promotion modifiers can fill jobs (see [RGO substitution](/buildings/rgo-to-building-substitution.md)).

# Examples of surgery (MnT)

| Step | Technique |
|------|-----------|
| Remove slave pops | Startup event clears slave pop type |
| Tribal demographics | Culture-gated peasant↔tribesmen conversion |
| Food baseline | Spawn `farming_village` levels from peasant counts |
| Kill vanilla RGOs | Replace with buildings; `change_max_raw_material_workers` |

# Not a substitute for

- New tags, bookmarks, or province ownership → need [country setup](/country-setup/) / history.
- Fine climate assignment at scale → [CSV location templates](/map/csv-location-templates-pipeline.md).

# Citations

[1] MnT `in_game/events/MnT_slaves.txt`, `MnT_tribes.txt`, `MnT_food.txt`, `MnT_rgo_removal.txt`
[2] MnT `in_game/common/on_action/MnT_pulse.txt`
