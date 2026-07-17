---
type: Playbook
title: Market price history from melted save
description: >-
  Extract per-market goods price history from EU5 SAV0200 text saves via
  market_manager.database — Pdx-Unlimiter melt, CSV schema, rolling window limits.
tags: [validation, economy, market, save, telemetry, balance]
timestamp: 2026-07-17T17:00:00+10:00
status: complete
source_mod: "Sire, Who Bound This Manor to a Single Merchandise"
---

# Market price history from melted save

# Problem

Balance analysis for **market-driven AI** (RGO conversion, trade mods) needs **time series** of `price_in_market`, not a single end-state screenshot. EU5 saves embed a **rolling** price history per good per market (~120 months).

Binary saves are not grep-friendly — **melt** to text first.

# Prerequisites

| Tool | Purpose |
|------|---------|
| [Pdx-Unlimiter](https://github.com/ParadoxGameConverters/Pdx-Unlimiter) | Melt `SAV0203` → `SAV0200` text |
| Python 3.10+ | `extract_eu5_market_history.py` in Sire dev repo |

# Workflow

## 1. Melt save

```
save games/MyCampaign (Melted).eu5
```

Header must start with `SAV0200` or contain `0004`. Script rejects packed binary.

## 2. Extract CSV

From Sire development repo:

```powershell
python scripts/extract_eu5_market_history.py `
  --save "path/to/Campaign (Melted).eu5" `
  --all-goods `
  --out observer_market_price_history_all.csv
```

| Flag | Effect |
|------|--------|
| `--all-goods` | Every good in every market (large file) |
| `--goods horses silk …` | Subset only |
| (default) | Premium set: horses, silk, pepper, incense, iron, fruit, wheat |
| `--max-markets N` | Debug cap |

## 3. CSV schema

| Column | Source in save |
|--------|----------------|
| `market_id` | `market_manager.database.{id}` |
| `center_loc_index` | `center=` under that market |
| `good` | Key under `goods={}` |
| `months_ago` | Index in `history={…}`; `0` = newest |
| `price` | Float in history array |

Parser: `scripts/extract_eu5_market_history.py` — regex on indented blocks.

# Rolling window limit

- Typical depth: **118–119 months** (~10 years).
- **Not** full campaign length — early-game price shocks require **event-sampled telemetry** (e.g. `SIRE_AI_PICK` `p_to` per year).
- Map `months_ago` → game year: `save_end_year - months_ago // 12`.

# Tile market vs market centre

- Script AI uses `location.market` — the market **attached to the province tile**.
- CSV `center_loc_index` is the market **hub** location id.
- All provinces in a market share one price series per good in this export.

Cross-link: [price_in_market script API](/economy/price-in-market-script-api.md).

# Dashboard (optional)

Sire ships `scripts/build_market_dashboard.py` → static JSON for `07_Test/dashboard/markets/`.

# Citations

- `development/Sire, Who Bound This Manor to a Single Merchandise/scripts/extract_eu5_market_history.py`
- `development/Sire, Who Bound This Manor to a Single Merchandise/references/market-price-telemetry-analytics.md`
