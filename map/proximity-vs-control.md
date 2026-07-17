---
type: Playbook
title: Proximity vs control
description: Decouple how far influence spreads from how much control 100% proximity grants — the four-knob model.
tags: [map, proximity, control, auto-modifiers, static-modifiers]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

In vanilla, high proximity often implies high control. MnT's design goal: **proximity spreads further**, but **100% proximity does not mean 100% control**.

# Four knobs

| Knob | What it changes | Typical fields |
|------|-----------------|----------------|
| **Spread cost** | How far proximity reaches | `land_cost_on_distance_from_capital`, road/sea/port/river costs |
| **Ceiling** | Max control from proximity alone | `REPLACE:proximity_to_capital` → `local_max_control` |
| **Growth / decay** | How fast control converges | `local_monthly_control`, `global_monthly_control_decline` |
| **Friction / bonuses** | Terrain, pops, ranks, integration | topography/vegetation `proximity`, pop impacts, location ranks, cores |

# MnT concrete change

```txt
REPLACE:proximity_to_capital = {
	local_max_control = 0.50  # vanilla 0.75
}
```

Spread costs in country auto_modifiers were lowered (land/road/sea/port), so influence travels farther — while the ceiling above prevents automatic full control.

# Stack other control sources

Do not rely on proximity alone. MnT also tunes:

- Location ranks (town/city/megalopolis `local_max_control`)
- Integration / core static modifiers
- Buildings (bailiff / rural control)
- Estate privileges (tribal strongholds → rural control)
- Capital: keep proximity source; consider removing extra capital `local_max_control` if proximity already dominates

# Terrain as friction

Topography and vegetation definitions carry `proximity` penalties (mountains, jungle, …). Roads add proximity on connections. Tune **templates** (what terrain a location has) and **type modifiers** together.

# Lesson

Changing only spread costs is insufficient. Always retune the **ceiling** and the **growth** knobs, then verify with inverse-control penalties and cabinet actions gated on average control.

# Citations

[1] MnT `main_menu/common/static_modifiers/MnT_location.txt` — `REPLACE:proximity_to_capital`
[2] MnT `in_game/common/auto_modifiers/MnT_country.txt` — distance cost fields
[3] MnT `Documentation/Change log.md` — 75% → 50% intent
[4] MnT topography/vegetation/road_types under `in_game/common/`
