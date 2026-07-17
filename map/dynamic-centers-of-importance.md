---
type: Playbook
title: Dynamic centers of importance
description: Yearly scored location hubs with geographic exclusivity across trade, production, culture, and education.
tags: [map, centers, events, script-values, modifiers]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Static “important city” flags go stale. Dynamic **Centers of Importance** recompute winners from live scores and grant location modifiers by tier.

# Architecture

1. **Score** each location into variables (trade / production / culture / education).
2. **Clear** old center modifiers.
3. **Promote** with geographic exclusivity: area → region → continent → world.
4. **Age-gate** the top tier so early game cannot spawn world hubs.

# Threshold ladder (shared across categories)

| Tier | Typical threshold idea |
|------|------------------------|
| Local | Low bar; one per area |
| Regional | Higher; best of area winners in region |
| Continental | Higher still |
| World | Highest + age requirement (e.g. Absolutism) |

Use `ordered_location_in_area` with `max = 1` for exclusivity. Carry promotion pointers in global vars (`region_center`, …).

# Score inputs (examples)

| Category | Inputs |
|----------|--------|
| Trade | Market access, merchant power, burgher trades, maritime presence |
| Production | Urban goods value × price |
| Culture | Influence, tradition, art quality |
| Education | Clergy/nobles, literacy, university/library buildings, books output |

Compute scores once into variables; do not recalculate inside every sort.

# Player feedback

- Dedicated mapmodes keyed on `has_location_modifier`
- Game concepts + loc that avoid hardcoding threshold numbers (they drift)
- Seed early trade with town_setup marketplace bumps if needed

# Pulse

Fire from yearly country pulse (often gated to one rank-1 / coordinator country) and optionally at game start.

# Citations

[1] MnT `in_game/events/MnT_centers.txt`
[2] MnT `in_game/common/script_values/mnt_centers.txt`
[3] MnT `in_game/common/scripted_effects/MnT_calc_location_center_score.txt`
[4] MnT `main_menu/common/static_modifiers/MnT_centers.txt`
[5] [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md)
