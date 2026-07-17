---
type: Reference
title: Mod performance — pulses and location scans
description: How monthly_country_pulse + every_owned_location / random_owned_location affect cost; RGO Conversion as a worked example.
tags: [performance, on-actions, pulses, locations, ai]
timestamp: 2026-07-11T21:35:00+10:00
status: complete
source_mod: rgo_conversion
source_version: "0.1.1"
---

EU5 script cost is dominated by **how often** code runs × **how many scopes** it walks × **how heavy** each trigger is — not by file count alone.

# Cost ladder (cheap → expensive)

| Pattern | Typical cost |
|---------|----------------|
| `on_game_start` set global / few effects | Negligible (once) |
| Player ScriptedGui / location panel `IsValid` | Negligible (UI only) |
| Timed modifiers / 1 construction marker | Engine-native; negligible |
| `monthly_country_pulse` + cheap `if` / `random = { chance = N }` | Low (once per country per month) |
| `every_owned_location` with **tight** `limit` (e.g. `var:busy = 1`) | Low if almost never matches; scales with owned locations only to *test* the limit |
| `random_owned_location` / `every_owned_location` with **heavy** `limit` (terrain×region allow matrix, pop counts, many ORs) | Can be expensive on large AI empires when it runs |
| World-wide invalid iterators (`every_location`) | Do not use — invalid / crash (KI-061) |

# RGO Conversion (worked example)

## What runs

| Hook | Work | Verdict |
|------|------|---------|
| `on_game_start` | Set `rgo_conv_mod_loaded` | No impact |
| `monthly_country_pulse` → `rgo_conv_monthly_tick` | `every_owned_location` limited to `var:rgo_conv_busy = 1` | **Negligible** — almost always zero matches; at most a handful of active projects |
| Same pulse → `rgo_conv_ai_try_convert` | AI only; `random = { chance = 1 }` then `random_owned_location = { limit = rgo_conv_can_start }` | **Main risk** — rare (1%/month/AI), but when it fires the limit can evaluate family × macro allow-lists (+ peasants / cooldown) across many locations |
| Full `location_window.gui` override | Load + open location UI | Memory / load-time only; not sim tick |
| Settling / lock modifiers | Up to 2 timed modifiers per converted location | Negligible |

## Player experience

Expect **no meaningful FPS impact** from Convert button, construction markers, or human play. Cost is in the monthly AI path, and even there only on the 1% roll.

## When AI cost could matter

- Late game, many mid/large AI tags
- Each 1% hit may scan owned locations until one passes `rgo_conv_can_start` (includes `rgo_conv_has_any_alternate` → many `rgo_conv_allows_*`)

Still unlikely to rival total-conversion economy pulses (MnT-scale), but worth watching if profiling shows script spikes.

## Mitigations (if needed later)

1. Move AI attempts to `yearly_country_pulse` (or biyearly).
2. Lower `chance` further, or gate on `num_locations < N` / great-power only.
3. Cheap pre-filter before allow matrix (e.g. `rgo_conv_not_busy` + `rgo_conv_cooldown_clear` + peasant check only; defer `has_any_alternate`).
4. Prefer `cmf` human-only pulses when AI must not run the path at all.

# Design rules of thumb

1. Never scan all locations monthly without a **cheap** limit first.
2. Put expensive terrain/region matrices behind rare chance or yearly cadence for AI.
3. Busy/project ticks: filter on a **variable or modifier**, not recompute allow-lists.
4. Full GUI file overrides are fine for QoL mods; cost is not in the sim loop.

# See also

* [Country pulses](/on-actions/country-pulses.md)
* [Pulse orchestration](/on-actions/pulse-orchestration.md)
* [Location iteration effects](/scripted-effects/location-iteration-effects.md)

# Citations

[1] `mod/rgo_conversion/in_game/common/on_action/rgo_conv_on_actions.txt`
[2] `mod/rgo_conversion/in_game/common/scripted_effects/rgo_conv_effects.txt` — `rgo_conv_monthly_tick`, `rgo_conv_ai_try_convert`
[3] MnT-scale contrast: [Pulse orchestration](/on-actions/pulse-orchestration.md)
