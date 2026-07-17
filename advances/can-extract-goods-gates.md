---
type: Reference
title: can_extract goods gates on advances
description: Which raw materials need can_extract_* from advances vs always-on auto_modifiers.
resource: game/in_game/common/advances/0_age_of_traditions.txt
tags: [advances, rgo, goods, extract]
timestamp: 2026-07-12T10:00:00+10:00
status: complete
source_mod: rgo_conversion
---

# Two sources of extract permission

| Source | Path | Meaning |
|--------|------|---------|
| Country auto_modifiers | `in_game/common/auto_modifiers/country.txt` | Baseline `can_extract_* = yes` for many goods (salt, amber, silk, dyes, chili, pepper, incense, wool, …) |
| Advances | `in_game/common/advances/0_age_of_traditions.txt` | Extra gates for metals, horses, bullion, etc. |

`change_raw_material` may still apply a good, but **production / legality** often expects the matching `can_extract_*` if vanilla gated it.

# Advance-gated extracts (vanilla Traditions)

| Flag | Typical unlock advance |
|------|------------------------|
| `can_extract_horses` | `ranching` |
| `can_extract_stone`, `can_extract_marble` | `mining_advance` |
| `can_extract_copper`, `can_extract_tin`, `can_extract_lead` | `bronze_working` |
| `can_extract_coal`, `can_extract_iron` | `iron_working` |
| `can_extract_saltpeter`, `can_extract_alum` | `advanced_mining` |
| `can_extract_goods_gold`, `can_extract_silver`, `can_extract_mercury` | `fine_metals_mining` |
| Grain/tuber extracts | `agriculture_advance` (`can_extract_wheat`, `maize`, …) |

Gold’s good id is `goods_gold` → flag is `can_extract_goods_gold`.

# Always-on examples (auto_modifiers)

Salt, amber, ivory, silk, dyes, chili, pepper, incense, wool, lumber, fish, livestock, … — usually **no** need to re-declare on a custom advance unless you are replacing auto_modifiers.

# Mod pattern

When an advance unlocks convert-to for a **gated** good, put the extract flag on that advance:

```txt
rgo_conv_eu_stud_farms = {
	…
	can_extract_horses = yes
}

rgo_conv_eu_blast_furnaces = {
	…
	can_extract_iron = yes
}
```

# See also

* [Advance-gated RGO unlocks](advance-gated-rgo-unlocks.md)
* [Change raw material](/economy/change-raw-material.md)
* [Modifier type definitions](/modifiers/modifier-stat-keys.md) — `can_extract_*` keys exist under modifier types

# Citations

[1] `game/in_game/common/advances/0_age_of_traditions.txt` — ranching / mining tree
[2] `game/in_game/common/auto_modifiers/country.txt` — baseline extracts
[3] `game/main_menu/common/modifier_type_definitions/00_modifier_types.txt` — `can_extract_*` definitions
