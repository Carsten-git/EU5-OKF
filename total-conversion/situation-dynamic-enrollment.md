---
type: Playbook
title: Situation dynamic enrollment
description: REPLACE situations to enroll countries via flags, market origin checks, and map_color by variable.
tags: [situations, events, total-conversion]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Situations can be more than flavor popups: **dynamic enrollment** decides who participates each month based on markets, geography, or flags.

# Pattern (MnT Columbian Exchange style)

1. `REPLACE:` the vanilla situation block (or add a new one).
2. In `on_start` / `on_monthly`, set/clear per-country enrollment flags.
3. Gate with market goods origin checks (`has_origin_in_old_world` / `new_world` style triggers — verify names in vanilla).
4. Drive `map_color` (or equivalent) from the enrollment variable for player feedback.

# Related

* [Pulse orchestration](/on-actions/pulse-orchestration.md)
* [Override ladder](/total-conversion/three-root-and-override-ladder.md)

# Citations

[1] MnT `in_game/common/situations/MnT_columbian_exchange.txt`
