---
type: Playbook
title: RGO balance telemetry pipeline
description: >-
  End-to-end playbook for modders validating market-driven AI conversion — save
  export, pick telemetry, combined analysis, cobweb interpretation, tuning levers.
tags: [validation, economy, rgo, ai, telemetry, balance]
timestamp: 2026-07-17T17:00:00+10:00
status: complete
source_mod: "Sire, Who Bound This Manor to a Single Merchandise"
---

# RGO balance telemetry pipeline

Use when shipping or tuning **AI that converts RGOs using live market prices** (Sire REQ-009 pattern).

**Concept map:** [Script logging and telemetry](script-logging-and-telemetry.md) · [RGO conversion AI market decision](/economy/rgo-conversion-ai-market-decision.md)

# Two datasets, one story

| Lens | Source | Horizon | Best for |
|------|--------|---------|----------|
| **Pick telemetry** | `error.log` → CSV (`SIRE_AI_PICK`) | Full campaign | Cobweb overshoot, era rotation, margin gate |
| **Market history** | Melted save → CSV | ~120 months at end | Late-game structure, regional dispersion |

**Never** conclude balance from one alone.

# Pipeline (copy-paste)

```powershell
# 1. Melt save (Pdx-Unlimiter) → Observer (Melted).eu5

# 2. Market prices
python scripts/extract_eu5_market_history.py --save "…/Observer (Melted).eu5" --all-goods

# 3. AI picks (telemetry build required)
python scripts/export_sire_ai_pick_csv.py

# 4. Reports
python scripts/analyze_sire_ai_picks.py
python scripts/analyze_market_price_history.py --picks 07_Test/data/sire_ai_picks_latest.csv --out 07_Test/results/market-price-analysis.md
```

Sire reference: `references/market-price-telemetry-analytics.md`

# Pick telemetry essentials

Hidden-event pattern: [Script telemetry via hidden events](script-telemetry-via-hidden-events.md)

Log at minimum:

- `game_year`, `country_tag`, `loc_id`
- `from_good`, `to_good`
- `p_from`, `p_to` from **`price_in_market(scope:location.market)`** (script truth)
- Optional `ui_from`, `ui_to` for GUI drift checks

Margin gate (Sire): convert only if `p_to / p_from - 1 >= 0.50`.

# Cobweb interpretation

Classic sequence for premium goods:

1. **Opening:** High `p_to` → many AI conversions (pulse × countries × eligible tiles).
2. **Supply:** Converted tiles increase output of target good.
3. **Price:** Local `p_to` falls; margin gate blocks new picks.
4. **Stickiness:** RGO **stays converted** — no automatic revert when price falls.
5. **Late game:** Global market medians may look stable while **regions** that converted early stay depressed.

Signals in data:

- Pick `p_to` for horses/pepper **falls 50–70%** from 1337 to 1500.
- Pick **volume** front-loaded (~25% in first 20 years).
- Market CSV: **low** peak-to-trough globally but **high CV** across markets at end.

# Tuning levers (generic)

| Lever | Typical location | Effect |
|-------|------------------|--------|
| Pulse rate | `on_action` monthly, `chance = N` | Linear on global conversion count |
| Margin floor | script_value `multiply = 1.5` | Fewer candidates pass |
| Cooldown | timed modifier on location | Recycle rate for same tile |
| Opening grace | `on_game_start` year gate | Delays first wave |
| Pulse ramp | epoch year → chance lerp | Spreads early shock |

Sire ships: 1%/month, 50% margin, 25y cooldown (0.2.0).

# Analysis scripts (Sire dev repo)

| Script | Output |
|--------|--------|
| `analyze_sire_ai_picks.py` | Pick timeline, geography, `p_to` drift |
| `analyze_market_price_history.py` | Save CSV + optional picks — combined report |
| `build_market_dashboard.py` | Static market charts |
| `build_ai_pick_dashboard.py` | Pick telemetry dashboard |

# Agent skill

Cursor: **`sire-market-price-analytics`** in Sire `.cursor/skills/`.

# Citations

- Sire `references/market-price-telemetry-analytics.md`
- Sire `references/REQ-009-ai-pick-telemetry-analytics.md`
- OKF [Market price history from save](market-price-history-from-save.md)
