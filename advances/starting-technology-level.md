---
type: Reference
title: Starting technology level
description: Prevent mod advances from being already researched when a country starts at tech level N.
resource: mod/northern_crusade_teu/in_game/common/advances/count_TEU_northern_crusade.txt
tags: [advances, technology, pitfall]
timestamp: 2026-07-06T00:00:00+10:00
status: complete
---

# Problem

Countries use a `starting_technology_level` in their government/country template (e.g. TEU catholic military order at **3**). Advances without an explicit level may be treated as **already unlocked** at game start.

# Fix

Set `starting_technology_level` on each mod advance **above** the country's starting level:

```txt
teu_nc_last_crusade = {
	starting_technology_level = 4
	…
}

teu_nc_amber_monopoly = {
	starting_technology_level = 5
	…
}
```

# Testing

Requires a **new game** — existing saves retain researched advances.

# See also

* [Advance file structure](advance-file-structure.md)
* [Country-specific advances](country-specific-advances.md)
* [Enabling your mod](/getting-started/enabling-your-mod.md)
* [Smoke testing checklist](/validation/smoke-testing-checklist.md)

# Citations

[1] `northern_crusade_teu/in_game/common/advances/count_TEU_northern_crusade.txt`
