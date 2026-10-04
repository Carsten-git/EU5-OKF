---
type: Reference
title: Change raw material (RGO)
description: Vanilla location effect to switch RGO goods, worker-cap companion, and empty on_raw_material_changed hook.
tags: [economy, rgo, raw-material, effects, locations]
timestamp: 2026-07-11T21:20:00+10:00
status: complete
---

Locations have a single **raw material** (RGO good). Vanilla can change it at runtime — flavor events, situations, and Columbian Exchange all use the same effect.

# Core effect

Scope: **location**.

```txt
change_raw_material = goods:iron
# or
change_raw_material = scope:target_good
```

Loc keys (engine): `CHANGE_RAW_MATERIAL_EFFECT` / past / third-person variants in `main_menu/localization/*/effects_l_*.yml`.

# Companion: RGO worker cap (reset expansions to level 1)

In EU5, **Expand RGO** raises the location’s RGO size, tracked as `rgo_workers` (what players often call “RGO level”). Flipping the good alone does **not** always wipe those expansions — so vanilla Columbian Exchange pairs the flip with:

```txt
change_max_raw_material_workers = {
	value = rgo_workers   # current size, e.g. 4
	subtract = 1          # → 3
	min = 0
	multiply = -1         # → -3  (delta applied to the cap)
}
```

**Meaning:** apply a **negative delta** of `(rgo_workers − 1)`, so the location ends at **1** worker slot / base expansion.

| Before (`rgo_workers`) | Delta `(n−1)×(−1)` | After |
|------------------------|--------------------|-------|
| 1 | 0 | 1 (already base) |
| 2 | −1 | 1 |
| 4 | −3 | 1 |
| 10 | −9 | 1 |

So “Columbian Exchange worker-cap formula” = **reset expanded RGO back to level 1**, then (usually) `change_raw_material`. It does **not** delete the RGO entirely; MnT-style `change_max_raw_material_workers = -100` is the hard-off variant.

Order in vanilla CE: worker reset → prosperity hit → change good. RGO Conversion follows the same worker reset before applying the new good.

Current vanilla `flavor_swe.38` provides a second, direct **RGO level** form:

```txt
if = {
	limit = { rgo_level > 1 }
	change_max_raw_material_workers = {
		value = rgo_level
		subtract = 1
		multiply = -1
	}
}
```

Use this form when an event must reset the displayed RGO level to 1 while keeping the existing good. Do not call `change_raw_material`. Runtime-check the displayed level, especially when construction is queued.

Also common: `change_prosperity = prosperity_weak_penalty` on the location when the good flips.

QoL conversion (RGO Conversion mod) on complete:

1. CE worker reset (above) → expansions back to **level 1**
2. `change_raw_material`
3. Timed **settling** hangover: `local_raw_material_output = -0.10` for 10 years (`rgo_conv_settling`)
4. Separate **25-year** convert-lock modifier (`rgo_conv_cooldown`)

# Trigger / query helpers

| Check | Use |
|-------|-----|
| `raw_material = goods:X` / `raw_material ?= goods:X` | Current RGO is / is not X |
| `rgo_workers` | Current RGO employment size on the location |
| `"raw_material_amount(scope:good)"` | Count of that good as RGO in a wider area (province/region scripts) |
| `has_industrial_raw_material = yes` | Category gate used in institution content |

# Hook: `on_raw_material_changed`

In `in_game/common/on_action/_hardcoded.txt`, vanilla ships an **empty** hook:

```txt
on_raw_material_changed = {
}
```

Mods can append effects here (logging, UI flags, “remember previous good”, compatibility with conversion projects). Confirm root/scopes in-game when wiring — treat as location-centric until verified.

# Vanilla call sites (examples)

| Source | Pattern |
|--------|---------|
| `events/DHE/flavor_flo.txt` | Instant `change_raw_material = goods:alum` (+ location modifier) |
| `events/DHE/flavor_SWE.txt` / `flavor_BOH.txt` / `flavor_HAB.txt` | Silver/iron discovery-style flips |
| `events/situations/little_ice_age.txt` | Climate stress → millet |
| `events/colonization/conquest_of_paradise.txt` | Scripted location picks → iron/wool |
| `generic_actions/columbian_exchange.txt` | Player/AI action → `scope:target_good` |

Full player-facing selection UX: [Columbian Exchange RGO pattern](/economy/columbian-exchange-rgo-pattern.md).

Design for a timed, employment-based player conversion mod lives in the mod stub: `mod/Sire, Who Bound This Manor to a Single Merchandise/README.md` (not in this engineering KB). Map progress UX: [Construction map markers](/buildings/construction-map-markers.md). Cooldown chips: [Timed location modifier visibility](/modifiers/timed-location-modifier-visibility.md).

# Related

* [Columbian Exchange RGO pattern](/economy/columbian-exchange-rgo-pattern.md)
* [RGO to building substitution](/buildings/rgo-to-building-substitution.md) — MnT removes vanilla RGOs entirely
* [Construction map markers](/buildings/construction-map-markers.md)
* [Custom map modes](/gui/custom-map-modes.md)

# Citations

[1] `in_game/common/effect_localization/location_effects.txt` — `change_raw_material`, `change_max_raw_material_workers`
[2] `in_game/common/generic_actions/columbian_exchange.txt`
[3] `in_game/common/on_action/_hardcoded.txt` — `on_raw_material_changed`
[4] `in_game/events/DHE/flavor_flo.txt` — Societas alum
[5] `in_game/events/DHE/flavor_SWE.txt`, `flavor_swe.38` — `rgo_level` reset to 1
