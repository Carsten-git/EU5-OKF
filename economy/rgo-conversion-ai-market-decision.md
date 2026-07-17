---
type: Playbook
title: RGO conversion AI — market economics patterns
description: Generic patterns for pulse-based RGO conversion AI using live market prices; Columbian ai_will_do; links to UI profit ladder and script APIs.
tags: [economy, rgo, ai, on-actions, markets, playbook]
timestamp: 2026-07-15T21:00:00+10:00
status: complete
source_mod: vanilla
---

Patterns for mods that make AI **pick convert targets from live market economics** instead of a fixed staple priority list or frozen `default_market_price` tiers.

**Concepts and APIs:** [RGO UI profit metrics](/economy/rgo-ui-profit-metrics.md) (wealth / tax base / income, GUI vs script).  
**Product-specific thresholds** (margin %, control gates, cadence) belong in the **mod's** design docs — not here.

# Decision model (generic)

1. **Eligibility** — terrain, advances, mod allow matrix (same as human Convert).
2. **Location viability** — mod-defined gates (e.g. minimum control, market access) so the tile can generate and sell wealth.
3. **Score candidates** — typically live `price_in_market(good, location.market)` per allowed good; see [when price ratio is enough](#when-live-price-ratio-matches-income-per-level).
4. **Pick** — commonly argmax score among qualifiers; tie-break policy is mod-specific.
5. **Margin bar** — mod-tuned minimum uplift vs current good before starting a project.
6. **Optional guards** — surplus block (`is_in_surplus_in_market`), protected current goods, conquest grace.

# When live price ratio matches income per level

If on a given location **output per level is ~flat across goods** (validate in-game) and **control / market access are shared**, treasury **income per level** and location **wealth per level** rank candidates the same as **live unit price**:

```
margin ≈ price_in_market(target) / price_in_market(current) − 1
```

If output differs per good (`RawGoodLocationItem.GetEfficiency`, terrain), compare full `O × P` — script may lack hypothetical `O`; see UI article.

Do **not** use `food`, `base_production`, or defines output tables without tile validation — [Goods food vs market price](/economy/goods-food-vs-market-price.md).

# Script: compare two goods on one market

Columbian `change_raw_material` `ai_will_do` (`columbian_exchange.txt`) — **1×** price delta for AI scoring:

```txt
scope:target_good = {
	add = "price_in_market(scope:target_location.market)"
	scope:target_location.raw_material = {
		subtract = "price_in_market(scope:target_location.market)"
	}
}
```

**50% margin gate** (pulse mods): candidate price minus floor (`1.5×` current), require quoted compare in goods-scope limit **and** post-pick guard:

```txt
# goods scope — scope:rgo_conv_ai_pick_loc saved on tile before ordered_goods
rgo_conv_ai_min_margin_price = {
	scope:rgo_conv_ai_pick_loc.raw_material = {
		value = "price_in_market(scope:rgo_conv_ai_pick_loc.market)"
		multiply = 1.5
	}
}
rgo_conv_ai_margin_delta = {
	value = "price_in_market(scope:rgo_conv_ai_pick_loc.market)"
	subtract = rgo_conv_ai_min_margin_price
}

# ordered_goods limit (goods scope) — quoted name required (parliament pattern)
"rgo_conv_ai_margin_delta" >= 0

# after max=1 pick — belt-and-suspenders before rgo_conv_start_*
scope:rgo_conv_ai_pick_good = { "rgo_conv_ai_margin_delta" >= 0 }
```

Do **not** use `multiply = 1.5` on a `subtract` block inside `raw_material` — engine may only subtract 1× current; use `subtract = rgo_conv_ai_min_margin_price` instead.

```txt
ordered_goods = {
	order_by = rgo_conv_ai_pick_order_score   # value = price_in_market(tile.market)
	limit = { rgo_conv_ai_ordered_candidate_ok = yes }
	max = 1
}
```

**Avoid:** `price_in_market = { value >= some_script_value }` — no vanilla precedent; RHS may evaluate as 0. **Avoid:** `multiply = -1` on `order_by` with `max = 1` — picks **lowest** price, not highest.

Trigger form for **absolute** checks against literals only: `goods:wheat = { price_in_market = { market = … value >= 2.5 } }` — see [price_in_market](/economy/price-in-market-script-api.md).

Do **not** copy Columbian `multiply = 10` / `value = -1` into pulse mods — those scale **action competition**, not a percentage margin.

# Monthly pulse pattern (generic)

```txt
monthly_country_pulse = { on_actions = { my_mod_monthly_country_pulse } }

my_mod_monthly_country_pulse = {
	effect = { my_ai_try_convert = yes }
}

my_ai_try_convert = {
	if = {
		limit = { is_ai = yes has_game_rule = my_ai_on }
		random = {
			chance = 1
			random_owned_location = {
				limit = { my_location_ok = yes my_can_start = yes }
				my_execute_market_pick = yes
			}
		}
	}
}
```

| Piece | Typical role |
|-------|----------------|
| `chance` | Throttle attempts per country per month |
| `random_owned_location` | One tile per successful roll |
| `my_location_ok` | Mod gates + human-equivalent eligibility |
| `my_execute_market_pick` | Score, margin check, start conversion |

Cost: [Mod performance pulses and scans](/on-actions/mod-performance-pulses-and-scans.md).

**Sire (REQ-009):** ships `chance = 1` (1%/month per AI country); tuned 2026-07-16 from 2% after observer playtest.

# Scoring approaches (choose after validation)

| Approach | Use when |
|----------|----------|
| **Live price ratio** `P_tgt/P_cur` | Flat `O_tile` on location; matches income/level UI |
| **Price index** `(P_tgt/D_tgt)/(P_cur/D_cur)` | Defines `O(g)∝1/D` validated; shock vs baseline |
| **`O(g)×P(g)` with per-good O** | Per-good output validated on tile |
| **Unit price only** | Avoid as sole metric if `O_tile` flat (tier-chasing) |
| **`food` / `base_production`** | **Never** as output proxy |

Defines table and price-index math: [RGO baseline output](/economy/rgo-baseline-output-and-price-balance.md).

# Columbian `ai_will_do` reference

`move_nw_good_to_new_location` (`columbian_exchange.txt`):

| Piece | Role |
|-------|------|
| `add target_price; subtract current_price` | Live unit price delta |
| `multiply = 10` | AI action scoring scale |
| Surplus / food terms | Domain-specific guards |

Cadence: `ai_tick_frequency = 60` (months) — not the same as a monthly country pulse.

Full pattern: [Columbian Exchange RGO pattern](/economy/columbian-exchange-rgo-pattern.md).

# Related

* [RGO balance telemetry pipeline](/validation/rgo-balance-telemetry-pipeline.md) — validate long observer runs (save prices + pick logs)
* [RGO UI profit metrics](/economy/rgo-ui-profit-metrics.md)
* [price_in_market in script](/economy/price-in-market-script-api.md)
* [RGO baseline output and price balance](/economy/rgo-baseline-output-and-price-balance.md)
* [Change raw material](/economy/change-raw-material.md)

# Citations

[1] `game/in_game/common/generic_actions/columbian_exchange.txt`
[2] `game/in_game/gui/expand_raw_goods_lateralview.gui`
[3] `game/in_game/common/trigger_localization/goods_triggers.txt` — `price_in_market`
