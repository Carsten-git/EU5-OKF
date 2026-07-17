---
type: Playbook
title: Environmental disease pattern
description: Drive endemic disease with climate/terrain triggers instead of contagious R0 spread.
tags: [diseases, climates, triggers, map]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

For endemic environmental disease (e.g. malaria), contagious-spread parameters are the wrong tool. Drive infection from **environment**.

# Pattern

1. Set contagious knobs inert where needed (`monthly_spawn_chance = 0`, `r0 = 0`, or equivalent — verify field names in vanilla disease files).
2. Use `environmental_infection` (or vanilla’s environmental hook) scaled by scripted triggers.
3. Triggers combine [Köppen climate aliases](/map/koppen-climates.md), topography, vegetation, and optionally [scripted geography](/map/scripted-geography.md).
4. Couple to situations when historical (plague situations gating spawn).

# Compatibility

If you replaced climates, **never** hardcode only new climate keys in disease files without alias triggers — vanilla-shaped content will break.

# Related

* [Köppen climates](/map/koppen-climates.md)
* [Scripted geography](/map/scripted-geography.md)

# Citations

[1] MnT `in_game/common/diseases/MnT_malaria.txt`
[2] MnT `in_game/common/scripted_triggers/MnT_climate_triggers.txt`
[3] MnT plague/smallpox disease files for situation coupling
