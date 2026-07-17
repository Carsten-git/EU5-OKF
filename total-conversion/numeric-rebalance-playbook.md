---
type: Playbook
title: Numeric rebalance playbook
description: Audit-friendly scalar rebalance — REPLACE/INJECT with vanilla provenance comments and optional spreadsheet links.
tags: [balancing, replace, inject, defines, prices]
timestamp: 2026-07-11T12:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1.6"
---

Large overhauls touch hundreds of numbers. Without provenance, rebalance is un-auditable and merge-hostile. Treat comments as part of the design.

# Conventions

| Pattern | Example |
|---------|---------|
| Inline from-value | `building_upkeep_multiplier = 1  #M&T from Vanilla 0.2` |
| Defines style | `BURGHER_TRADE_IMPACT_ON_SUPPLY_SCALE = 0.75 #vanilla: 0.25` |
| Comment out removed effects | `# monthly_prestige = -0.1` under “From vanilla” |
| Prices REPLACE | `gold = 600 #M&T from 2000` |
| Surgical cancel | `INJECT` with opposite sign to nullify one vanilla field |
| Bulk tables | File header link to spreadsheet (climates/topography) |

# Playbook

1. Prefer `REPLACE:<key>` for whole objects (`country_base_values`, prices, estates).
2. Tag **every** changed scalar with old value.
3. Comment out removed vanilla effects instead of silent deletion.
4. Put cross-cutting constants in [loading-screen defines](/total-conversion/loading-screen-defines.md) — changed lines only.
5. Use `INJECT` to cancel a single vanilla contribution.
6. Offload mass tables to a sheet; one-line header link in the data file.
7. Subjects/opinions: tune `diplo_chance_*` and related weights with the same provenance style.

# Related

* [Loading-screen defines](/total-conversion/loading-screen-defines.md)
* [Soft-disable](/total-conversion/soft-disable-vanilla-systems.md)
* [Goods demand baskets](/economy/goods-demand-construction-baskets.md)

# Citations

[1] MnT `in_game/common/auto_modifiers/MnT_country.txt`
[2] MnT `in_game/common/prices/MnT_01_buildings.txt`
[3] MnT `loading_screen/common/defines/MnT_Defines.txt`
[4] MnT `in_game/common/climates/MnT_default.txt` — spreadsheet header link
