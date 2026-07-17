---
type: Playbook
title: Pulse orchestration
description: Wire game-start and monthly/yearly systems with ordered on_actions, delays, and dependency contracts.
tags: [on-actions, pulses, orchestration, game-start]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Large mods need a **single orchestration file** that owns pulse hooks and documents event order. Scattering monthly fires across files makes dependency bugs inevitable.

# Hook layers

| Hook | When | Use for |
|------|------|---------|
| `on_game_start` | Before country selection finishes | World surgery that must run for all tags; init globals |
| Delayed follow-up (`delay = { days = 1 }` → custom on_action) | Day after start | Player-only setup; `is_human` is unreliable too early |
| `monthly_country_pulse` / `yearly_*` / `biyearly_*` | Recurring | Economy, centers, AI maintainers |

MnT pattern: redefine vanilla pulse keys to call `*_MnT` child actions that list events in order.

# Dependency contract

Write the order in comments and keep it:

```txt
# Example contract (MnT-style):
# slaves strip → tribes seeding → food buildings → RGO hide → EPBM init
```

When pass B reads variables written in pass A:

1. Fire country accumulation events first.
2. `delay = { days = 1 }`.
3. Fire world/market aggregators (often tag-gated to one country).

# Player vs AI

- Heavy analytics: `is_ai = no`.
- AI maintainers: cheaper yearly events.
- Human-only toggles: delayed `on_game_has_started`-style hook after selection.

Prefer these over each mod checking `is_ai = no` on vanilla pulses when only humans matter. Complements [CMF registration hooks](/community-mod-framework/registration-and-on-action-hooks.md).

# Related

* [On game start](/on-actions/on-game-start.md)
* [Country pulses](/on-actions/country-pulses.md)
* [Hidden event economy pipeline](/economy/hidden-event-economy-pipeline.md)
* [Startup world surgery](/total-conversion/startup-world-surgery.md)

# Citations

[1] MnT `in_game/common/on_action/MnT_pulse.txt`
[2] [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md)
