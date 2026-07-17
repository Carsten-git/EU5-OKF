---
type: Playbook
title: Tribal levies and demographic seeding
description: Buff tribal military via levy size, culture gates, and hidden startup pop redistribution.
tags: [map, levies, tribes, game-start, pops]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Tribal strength is a **stack**, not a single levy number: levy `size`, pop distribution at setup, estate buildings, and control/proximity friction.

# Levy definitions

Raise tribesmen levy `size` well above vanilla generic infantry ratios. Prefer **culture allowlists** for special cavalry (steppe, Bedouin/Berber) over government-type gates when remote peoples should keep composition.

Remove or relax `country_allow` restrictions that force tribal composition to change with government form.

# Startup demographic pipeline

Hidden game-start events (before food systems if they depend on pop types):

1. Convert selected culture peasants → tribesmen where historically nomadic.
2. Erode tribesmen → peasants by urbanization/development formula in settled areas.
3. Special-case outliers (e.g. Mongol/Oirat outside steppe).

Yearly maintenance: auto-fill event-only buildings (transhumant pasture) to cap wherever tribesmen exist (`allow = { always = no }` + event construction).

# Policy gates

- Block tribal governments from “settle tribes” cabinet actions if design wants them to stay tribal.
- Non-tribal owners can get strong local tribal promotion in targeted provinces.

# Related

- [Proximity vs control](/map/proximity-vs-control.md) — tribesmen can resist control / proximity propagation
- [Hidden event economy pipeline](/economy/hidden-event-economy-pipeline.md) — pulse orchestration

# Citations

[1] MnT `in_game/common/levies/051_tribal_levies.txt`
[2] MnT `in_game/events/MnT_tribes.txt`
[3] MnT `in_game/common/cabinet_actions/MnT_settle_tribesmen.txt`
[4] MnT `Documentation/Change log.md`
