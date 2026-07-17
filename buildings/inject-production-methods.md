---
type: Reference
title: INJECT production methods
description: Attach new production methods to vanilla buildings with INJECT without copying whole building blocks.
tags: [buildings, inject, production-methods]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

When a new system needs every (or many) vanilla buildings to gain a PM — e.g. estate maintenance baskets — **do not** copy building definitions.

# Pattern

1. Define PMs in `in_game/common/production_methods/`.
2. In a dedicated building_types file, attach them:

```txt
INJECT:cathedral = {
	possible_production_methods = {
		epbm_clergy_maintenance
	}
}
```

3. Group injects by domain (`epbm_estate_building_maintenance.txt`).

# Why this works

- Survives vanilla building rebalances better than full REPLACE.
- Keeps your PM logic in one place.
- Scales to dozens of buildings with a repetitive but clear file.

# Related

- [Override ladder](/total-conversion/three-root-and-override-ladder.md)
- [Estate-paid building maintenance](/economy/estate-paid-building-maintenance.md)

# Citations

[1] MnT `in_game/common/production_methods/epbm_estate_maintenance_pms.txt`
[2] MnT `in_game/common/building_types/epbm_estate_building_maintenance.txt`
