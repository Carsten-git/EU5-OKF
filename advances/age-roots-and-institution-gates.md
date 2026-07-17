---
type: Reference
title: Advance age roots and institution gates
description: Which age-root advances need institutions vs free public-health roots — for requires = hooks.
resource: game/in_game/common/advances/
tags: [advances, institutions, requires]
timestamp: 2026-07-12T10:00:00+10:00
status: complete
source_mod: rgo_conversion
---

# Problem

Hooking `requires = renaissance_advance` / `new_world_advance` / `confessionalism_advance` / `manufactories_advance` means the player must **embrace the institution** before the mod advance is researchable.

# Free vs institution age roots (vanilla)

| Age | Institution-gated root (examples) | Often-free / always-available root |
|-----|-----------------------------------|--------------------------------------|
| Traditions | `feudalism_advance` (institution) | `agriculture_advance`, `mining_advance`, `written_alphabet` (`depth = 0`) |
| Renaissance | `renaissance_advance`, banking, professional armies | Prefer institution root if themed; no simple public-health free root in age 2 |
| Discovery | `new_world_advance` | **`surgery_advance`** (public health, `depth = 0`) |
| Reformation | `confessionalism_advance` | **`pharmacology_advance`** |
| Absolutism | `manufactories_advance` | **`sanitation_advance`** |

# Mod practice

For convert-unlock trees that should appear when the **age** is open—not when an institution arrives—either:

1. **`depth = 0` with no `requires`** (own free root in the age tab — clearest UX; RGO Conversion continent nodes), or
2. `requires` a free public-health root (Discovery+ `surgery_advance` / `pharmacology_advance` / `sanitation_advance`) or early Traditions (`agriculture_advance`) — works, but easy to miss under Public Health.

```txt
rgo_conv_eu_saltworks = {
	age = age_3_discovery
	depth = 0
	# no requires — visible free root for that continent's capital
	…
}
```

Avoid `requires = renaissance_advance` / `new_world_advance` / `manufactories_advance` unless you intentionally want institution gates.

# See also

* [Advance file structure](advance-file-structure.md)
* [Advance-gated RGO unlocks](advance-gated-rgo-unlocks.md)

# Citations

[1] `0_age_of_discovery.txt` — `surgery_advance` vs `new_world_advance`
[2] `0_age_of_reformation.txt` — `pharmacology_advance` vs `confessionalism_advance`
[3] `0_age_of_absolutism.txt` — `sanitation_advance` vs `manufactories_advance`
[4] RGO Conversion continent advances — Discovery/Reformation/Absolutism hook free roots
