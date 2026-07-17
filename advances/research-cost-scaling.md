---
type: Reference
title: Advance research cost scaling
description: research_cost is a relative modifier on BASE_RESEARCH_COST — UI ≈ 25×(1+research_cost).
resource: game/loading_screen/common/defines/00_defines.txt
tags: [advances, research, defines, pitfall]
timestamp: 2026-07-12T10:05:00+10:00
status: complete
source_mod: rgo_conversion
---

# Trap

`research_cost` is **not** raw UI progress.

Observed (Traditions, RGO Conversion Stud Farms):

| `research_cost` | UI progress |
|-----------------|-------------|
| `5` | **~150** |
| `0.2` | **~30** |
| `-0.8` | **~5** (intended cheap test) |

# Working formula

```
UI progress ≈ BASE_RESEARCH_COST × (1 + research_cost) × (age / other mods…)
           ≈ 25 × (1 + research_cost)
```

Matches tooltip key `SPECIAL_RESEARCH_COST` (`$VAL|-=%2$` — percent-style specific cost).

Vanilla mills use `research_cost = -0.75` (= keep 25% of base ≈ large discount).

# Defines

From `loading_screen/common/defines/00_defines.txt`:

| Define | Default | Role |
|--------|---------|------|
| `BASE_RESEARCH_COST` | **25** | Base when advance DB loads |
| `BASE_UNIQUE_RESEARCH_COST` | **5** | Unique/country line in breakdown (`COUNTRY_UNIQUE_RESEARCH_COST`) |
| `AGE_RESEARCH_MODIFIER` | **0.15** | Per-age scaling |
| `PREVIOUS_AGE_REDUCTION` | **-8** | Discount researching older-age advances |

# Quick targets (ignore age mods)

| Desired ~UI (Traditions-era base ~25) | Set `research_cost` |
|-------------|---------------------|
| ~5 (fast test) | **-0.8** |
| Match other advances in the age (~25 / ~28.25 / …) | **0** (or omit) |
| ~50 | **1.0** |
| ~75 (triple Traditions base — not “age-normal”) | **2.0** |
| ~150 | **5.0** |

Later ages raise the age base (e.g. ~28.25); `research_cost = 0` still tracks peers. `2.0` adds roughly `+2 × BASE_RESEARCH_COST` on top (25→75, 28.25→78.25).

# See also

* [Advance file structure](advance-file-structure.md)
* [Advance-gated RGO unlocks](advance-gated-rgo-unlocks.md)
* [KI-071](/validation/known-issues.md)

# Citations

[1] `game/loading_screen/common/defines/00_defines.txt` — `BASE_RESEARCH_COST`, etc.
[2] RGO Conversion `rgo_conv_eu_stud_farms` — measured 5→150, 0.2→30
[3] `1_building_unlocks.txt` — `research_cost = -0.75` mill discounts
[4] Loc `SPECIAL_RESEARCH_COST` / `COUNTRY_UNIQUE_RESEARCH_COST` in `interfaces_l_english.yml`
