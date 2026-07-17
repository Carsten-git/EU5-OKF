---
type: Playbook
title: Soft-disable vanilla systems
description: Gate disasters, CBs, subjects, buildings, and events with always=no instead of deleting definitions.
tags: [total-conversion, soft-disable, disasters, casus-belli, subjects]
timestamp: 2026-07-11T12:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1.6"
---

Deleting vanilla definitions breaks references and merge workflows. **Soft-disable** with the narrowest visibility/start gate; keep the rest of the block for documentation and future re-enable.

# Gate by system

| System | Gate field | Example |
|--------|------------|---------|
| Disaster | `can_start` (first clause) | `always = no` |
| Casus belli | `create_visible` | `always = NO` |
| Subject type | `creation_visible`, `visible_through_*`, `subject_creation_enabled` | `always = no` |
| Building | `location_potential` | `always = no` |
| Event | `trigger` | `always = no` |

```txt
REPLACE:time_of_troubles = {
	can_start = {
		always = no
		# original conditions kept below for reference
		current_age_or_later = { age = age_4_reformation }
		# …
	}
}

attack_threat = {
	create_visible = {
		always = NO  # disabled — humiliate rival exists
	}
}
```

Always comment **why**. Prefer soft-disable over empty files.

# Related

* [Override ladder](/total-conversion/three-root-and-override-ladder.md)
* [Numeric rebalance playbook](/total-conversion/numeric-rebalance-playbook.md)

# Citations

[1] MnT `in_game/common/disasters/MnT_time_of_troubles.txt`
[2] MnT `in_game/common/casus_belli/attack_threat.txt`
[3] MnT `in_game/common/subject_types/uc_bey.txt`, `colonial_nation.txt`
[4] MnT `in_game/common/building_types/rural_buildings.txt` — `location_potential = { always = no }`
