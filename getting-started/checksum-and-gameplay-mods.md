---
type: Reference
title: Checksum and gameplay mods
description: Why gameplay mods show “modifies the checksum” and when that warning can be avoided.
tags: [checksum, ironman, multiplayer, metadata, achievements]
timestamp: 2026-07-11T20:20:00+10:00
status: complete
---

# What the warning means

When the launcher/playset shows that a mod **modifies the checksum**, the game’s loaded data no longer matches the stock patch checksum. That matters for:

* Multiplayer compatibility (everyone needs the same checksum)
* Ironman / achievement eligibility (stock checksum usually required)

There is **no `metadata.json` flag** that makes a gameplay mod report as checksum-clean while still changing scripts, buildings, events, or gameplay GUI.

# Can we avoid it?

| Mod content | Typically changes checksum? |
|-------------|----------------------------|
| Buildings, effects, triggers, on-actions, events that alter state | **Yes** |
| Overrides of `in_game/gui` that call scripted GUI / change gameplay actions | **Yes** |
| Loc / gfx / pure cosmetic only | Sometimes still yes on EU5 (broader than EU4’s old ignore list); do not rely on “cosmetic = clean” |

**RGO Conversion** adds buildings, scripted effects/triggers, on-actions, and a location-panel action — it **must** change the checksum. That is expected and not a bug.

# What not to do

* Do not strip gameplay files just to silence the warning — the mod would stop working.
* Do not recommend checksum patchers / exe hacks in this knowledge base for “fixing” achievements.

# See also

* [Mod metadata and descriptor](/getting-started/mod-metadata-and-descriptor.md)
* [Enabling your mod](/getting-started/enabling-your-mod.md)

# Citations

[1] [EU5 Patches wiki — Checksum](https://eu5.paradoxwikis.com/Patches)
[2] Forum reports: GUI/loc may also affect EU5 checksum (unlike older titles’ narrower ignore lists)
