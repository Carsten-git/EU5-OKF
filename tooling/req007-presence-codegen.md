---
type: Playbook
title: REQ-007 historical presence — CSV and codegen
description: >-
  Author and validate the Sire historical profile pool/matrix CSVs and
  generate scripted triggers for region × family × good eligibility.
tags: [tooling, req-007, rgo, map, validation, codegen]
timestamp: 2026-07-18T00:00:00+10:00
status: complete
source_mod: "Sire, Who Bound This Manor to a Single Merchandise"
---

# REQ-007 historical presence — CSV and codegen

Product rules: Sire development OKF `05_Design Documents/DESIGN.md` (canonical v0.2.0) and `references/REQ-007-*.csv`.

## Overview

The **Historical** conversion profile uses:

1. **Pool** — `region,family,good` rows (goods present on vanilla tiles in that region × family).
2. **Matrix** — up to **5** convert-to staples per `(region, family)` cell, ranked by `default_market_price`.
3. **Codegen** — `generate_presence_triggers.py` writes `00_rgo_conv_hist_generated_*.txt` in the mod repo.

**Broad** profile is unchanged (macro × family in `terrain-goods-v2.md`).

## Workflow

```powershell
cd development/Sire, Who Bound This Manor to a Single Merchandise

# After editing references/REQ-007-pool-historical.csv or REQ-007-matrix-historical.csv:
python scripts/generate_presence_triggers.py

python scripts/validate_req007.py
python scripts/smoke_req007_rtv_austria.py   # optional
```

## Key scripts

| Script | Role |
|--------|------|
| `generate_presence_triggers.py` | CSV → scripted triggers |
| `validate_req007.py` | Exit 0 = ship gate |
| `seed_matrix_from_pool.py` | Auto-cap cells at 5 cheapest goods |
| `req007_policy.py` | Fish/wetlands, desert sand, mountain stone, etc. |

## CSV schema

**Pool:** `region,family,good` — e.g. `north_german_region,farmland,wheat`

**Matrix:** `region,family,good,rank` — rank 1–5 per cell

Authoring inputs: `references/REQ-007-vanilla-rgo-by-region-family.md` (vanilla seed).

## RTV cells

Historical profile also uses **topography × vegetation** generated cells (`00_rgo_conv_hist_generated_topoveg.txt`). See product DD archive `DD-REQ-007-rtv-topography-vegetation.md`.

## Citations

- Product: `development/Sire…/references/REQ-007-pool-historical.csv`, `REQ-007-matrix-historical.csv`
- Mod: `in_game/common/scripted_triggers/00_rgo_conv_hist_generated_*.txt`
- Validator: `scripts/validate_req007.py`
