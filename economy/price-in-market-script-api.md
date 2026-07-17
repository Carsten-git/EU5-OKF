---
type: Reference
title: price_in_market in script
description: Live goods market price in triggers and script_values — scopes, vanilla call sites, and limits vs default_market_price.
tags: [economy, markets, goods, script-values, triggers, rgo]
timestamp: 2026-07-15T21:00:00+10:00
status: complete
source_mod: vanilla
---

EU5 mods can read **current** market prices in script — not only static `default_market_price` from goods definitions. Use this for AI that should react to shortages, trade, and war economy like a player reading the market panel.

# Trigger form (goods scope)

Localization key: `PRICE_IN_MARKET_TRIGGER` in `trigger_localization/goods_triggers.txt`.

```txt
goods:wheat = {
	price_in_market = {
		market = scope:current_location.market
		value >= 2.5
	}
}
```

**Verified call sites:** `generic_actions/great_pestilence.txt`, `scripted_triggers/situation_triggers.txt` (absolute thresholds per good on `scope:actor.capital.market`).

| Field | Role |
|-------|------|
| `market` | Which market center to read (tile market, capital market, saved scope) |
| `value` | Compare operator + number (live price on that market) |

Related market-scope triggers (`trigger_localization/market_triggers.txt`): `market_price`, `target_price`, `food_price`, `goods_demand_in_market`, `is_in_surplus_in_market`.

**Food vs goods:** `food_price` is the abstract food stockpile on a market. `price_in_market(goods:wheat)` is the **wheat commodity** price. Do not use the goods `food =` field as a price or output proxy — see [Goods food vs market price](/economy/goods-food-vs-market-price.md).

# Script value form (comparison / scoring)

In `ai_will_do` or `script_values`, use the string accessor on a **goods** scope:

```txt
scope:target_good = {
	add = "price_in_market(scope:target_location.market)"
	scope:target_location.raw_material = {
		subtract = "price_in_market(scope:target_location.market)"
	}
}
```

**Verified:** `generic_actions/columbian_exchange.txt` (`move_nw_good_to_new_location` `ai_will_do`), `generic_actions/markets.txt` (cross-market diff), `generic_actions/columbian_exchange.txt` (mission-style add/subtract).

`order_by = "price_in_market(scope:mission_target_market)"` — `missions/generic_capital_economy_mission_pack.txt`.

# Scopes cheat sheet

| Question | Typical scope |
|----------|----------------|
| Where does this tile sell RGO? | `scope:current_location.market` |
| Country capital market | `scope:actor.capital.market` |
| Compare two goods on same market | goods scope + same `market =` in both `price_in_market` reads |

# Static vs live

| Source | When to use |
|--------|-------------|
| `default_market_price` in `common/goods/` | Tooling, matrix seeding, tier buckets — **not** live AI decisions |
| `price_in_market` | Player-like “what does the market pay **today**?” |

`relative_raw_material_price` (location trigger) compares **current** tile RGO price on local market to **default** price — useful for “is my current good hot right now?” not for comparing two hypothetical goods (no verified script usage in vanilla content files).

# GUI profit / income (not in script)

Wealth, tax base, and income per level are **GUI-only** (`GetIncomePerLevel`, `GetRGOProfitPerLevel`, …). Full API map and concept ladder: [RGO UI profit metrics](/economy/rgo-ui-profit-metrics.md).

# Same-tile: two goods, one market

Columbian pattern — subtract current good price from candidate on the **same** `location.market`:

```txt
scope:target_good = {
	add = "price_in_market(scope:current_location.market)"
	scope:current_location.raw_material = {
		subtract = "price_in_market(scope:current_location.market)"
	}
}
```

When output per level is flat on the tile, this difference tracks the same ranking as **income per level** in the expand UI. Margin thresholds and location gates are **mod product** choices. Patterns: [RGO conversion AI market decision](/economy/rgo-conversion-ai-market-decision.md).

# Related

* [RGO UI profit metrics](/economy/rgo-ui-profit-metrics.md) — wealth / tax base / income; full GUI vs script table
* [RGO conversion AI market decision](/economy/rgo-conversion-ai-market-decision.md) — pulse / Columbian patterns
* [Columbian Exchange RGO pattern](/economy/columbian-exchange-rgo-pattern.md) — `ai_will_do` scoring
* [Mod performance pulses and scans](/on-actions/mod-performance-pulses-and-scans.md)

# Citations

[1] `game/in_game/common/trigger_localization/goods_triggers.txt` — `price_in_market`
[2] `game/in_game/common/generic_actions/columbian_exchange.txt` L344–377
[3] `game/in_game/common/generic_actions/great_pestilence.txt` — absolute `price_in_market` gates
[4] `game/main_menu/localization/english/interfaces_l_english.yml` — `GetRawMaterialsPrice` tooltip
[5] `logs/data_types/data_types_uncategorized.txt` — `Location.GetRGOProfitPerLevel`, `GoodItem.GetMaxRGOProfit`
