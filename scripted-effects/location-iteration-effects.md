---
type: Reference
title: Location iteration effects
description: Valid EU5 iterators over locations — there is no every_location; use every_owned_location / every_location_in_*.
tags: [effects, locations, on-actions, pitfall]
timestamp: 2026-07-11T20:00:00+10:00
status: complete
---

# Do not use `every_location`

There is **no** global `every_location` effect. Using it logs:

```text
Unknown effect every_location at …
```

and can break `on_game_start` / new-game setup (observed with RGO Conversion).

# Valid patterns (grep vanilla)

| Effect | Scope context |
|--------|----------------|
| `every_owned_location = { … }` | **country** |
| `every_location_in_province = { … }` | province |
| `every_location_in_area = { … }` | area |
| `every_location_in_region = { … }` | region |
| `every_ownable_location_in_province_definition = { … }` | province definition |
| `every_location_in_scripted_geography = { … }` | scripted geography |

# Prefer lazy init

Mass-touching every location at game start is expensive even when valid. Prefer capturing state on first interaction (button / event / first monthly touch of owned locations).

# Related

* [Change raw material](/economy/change-raw-material.md)
* [On-game-start](/on-actions/on-game-start.md)
* [Known issues](/validation/known-issues.md) — KI-061

# Citations

[1] Vanilla `on_action/_hardcoded.txt` — `every_owned_location`
[2] `error.log` — `Unknown effect every_location` from `rgo_conversion` game-start hook
