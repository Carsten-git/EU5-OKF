---
type: Playbook
title: Hidden event economy pipeline
description: Monthly pulses, delayed aggregators, temporary demand ping-pong, and economic take-off drag.
tags: [economy, events, on-actions, market-demand, balancing]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Complex economies in EU5 are often a **cron of hidden events**, not a single define. MnT wires government sliders into real market demand and slows take-off with baseline auto_modifiers.

# Monthly orchestration

One pulse file chains subsystems in order:

1. Per-country accumulation events (court cost, diplomatic spending, goods value, …)
2. `delay = { days = 1 }`
3. World/market aggregation events that read the accumulated vars

Player-only heavy analytics (`is_ai = no`) keep AI cheap.

# Slider → market demand

**Phase A (every country):** write a scaled amount onto `capital.market.location` variables:

```txt
# Conceptual
amount = slider_value * country_economical_base / (1 + efficiency)
```

**Phase B (once per world):** pick a single coordinator (MnT uses a fixed tag such as FRA) to `every_market_in_world` and `add_temporary_demand` scaled by basket price.

Diplomatic spending can **split**: part to an estate as gold, part to market demand.

# Temporary demand ping-pong

Updating the same temporary demand in place may not refresh the UI. Maintain **two** demand definitions (`demand1` / `demand2`) and alternate remove/add each month.

# Economic take-off drag (levers)

| Lever | Technique |
|-------|-----------|
| Full building upkeep | `building_upkeep_multiplier = 1` then redistribute (EPBM) |
| Lower default prices | Bulk `REPLACE` goods with half vanilla `default_market_price` |
| Stability drift | Negative `stability_investment` in country base; offset via peasant share |
| Minting pressure | Auto_modifiers `scales_with` minting slider vs threshold |
| Nullify upkeep advances | `INJECT` negative `building_upkeep_efficiency` to cancel vanilla bonuses |
| Cut food/output | Lower `food` / `base_production` on goods |

Tune **price and physical output** together — price alone is not enough.

# Income history without arrays

Numbered country variables (`mnt_income_hist_01` … `_50`) as a shift register, pushed monthly. GUI reads them as a chart. See [Custom UI patterns](/gui/custom-ui-patterns.md).

# Citations

[1] MnT `in_game/common/on_action/MnT_pulse.txt`
[2] MnT `in_game/events/MnT_cost_of_the_court.txt`, `MnT_diplomatic_spending.txt`
[3] MnT `in_game/common/auto_modifiers/MnT_country.txt`
[4] MnT `in_game/common/goods/MnT_*.txt`
[5] [Estate-paid building maintenance](/economy/estate-paid-building-maintenance.md)
