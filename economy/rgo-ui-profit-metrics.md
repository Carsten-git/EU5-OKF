---
type: Reference
title: RGO UI profit metrics — wealth, tax base, income
description: Player-facing RGO economics ladder (wealth, tax base, income); GUI data_types APIs vs script triggers; same-tile comparison when output is shared.
tags: [economy, rgo, gui, markets, script-values, triggers]
timestamp: 2026-07-15T21:00:00+10:00
status: complete
source_mod: vanilla
---

EU5 exposes RGO economics in **three layers** (wealth → tax base → income). Most per-level numbers are **GUI-only**; script has live **unit price** and **current-good output**, not hypothetical `GetIncomePerLevel`.

# Concept ladder

From `game_concepts_l_english.yml`:

| Concept | Definition |
|---------|------------|
| **Wealth** | Combined profit of a location's RGO and buildings from selling goods to market (+ burgher trade where applicable) |
| **Tax base** | Wealth the government can tax = wealth affected by **control** |
| **Income** | Gold reaching country **treasury** (estate taxes + trade income) |

Approximate chain (per RGO level, conceptual):

```
goods_output × sell_price  →  wealth  →  tax_base (× control)  →  income (× estates, tax rates)
```

`sell_price` at the location may differ from market-center price via **market access**.

# UI surfaces and APIs

Sources: `game/in_game/gui/`, `interfaces_l_english.yml`, `lists_l_english.yml`, `logs/data_types/data_types_uncategorized.txt`.

## Map — expand RGO tooltip (current good)

| Player label | GUI API | Notes |
|--------------|---------|-------|
| RGO level / employment | `Location.GetMaxRGOWorkers`, `GetRGOWorkers` | Levels vs workers |
| Output per level | `GetRawMaterialsOutput / GetMaxRGOWorkers` | Goods-units; @supply icon |
| **Tax base per level** | `Location.GetRGOProfitPerLevel` | @tax_base; loc key `RAW_MATERIAL_LOCATION_TABLE_PROFIT_PER_LEVEL` |
| Unit price | `Location.GetMarket.GetPrice(GetRawMaterial)` | Market-center commodity price |
| Total monthly sales | `Location.GetRawMaterialsPrice` | Output sold for gold (tooltip) |
| Expand cost | `Location.GetUpgradeRGOConstructionDemand`, `GetRGOCost` | Construction basket |

Market price tooltip uses `Location.GetMarket.GetMarketEntry(GetRawMaterial)` → `GoodsMarketEntry`.

## Expand raw goods lateral view (candidate good × location)

| Column | GUI API | Sort loc key |
|--------|---------|--------------|
| Added **wealth** | `RawGoodLocationItem.GetProfitPerLevel` | `EXPAND_RAW_GOODS_SORT_BY_PROFIT` |
| **Income** per level | `RawGoodLocationItem.GetIncomePerLevel` | `EXPAND_RAW_GOODS_SORT_BY_INCOME` |
| Market price | `Location.GetMarket.GetPrice(...)` | `market_price` |
| Efficiency | `RawGoodLocationItem.GetEfficiency` | — |
| Profit breakdown | `RawGoodLocationItem.GetProfitInfo` | Tooltip |

`PROFIT_EXPAND_RGO_TOOLTIP_TEXT`: each additional level adds `GetProfitPerLevel` to location **wealth**.

Buildings parallel (for comparison): `Building.GetProfitPerLevel` (wealth), `Building.GetIncomeToOwnerPerLevel` (treasury taxes), `Building.GetTaxBaseValuePerLevel`.

## Production panel — per good in market

| Column | GUI API |
|--------|---------|
| Highest **income** per buildable RGO level | `GoodItem.GetMaxRGOIncome` |
| (Unused in current GUI) max wealth | `GoodItem.GetMaxRGOProfit` |

`GOODS_EXPAND_RGO_TT_TEXT`: "Highest **income** increase per buildable RGO level".

`lists_l_english.yml`: `RGO_SORT_BY_PROFIT` = max **tax base** per level; `RGO_SORT_BY_INCOME` = max **income** per level.

## Market hover — `GoodsMarketEntry`

Market-wide intelligence (not tile revenue):

| Field | API |
|-------|-----|
| Current price | `GetPrice` |
| Target price | `GetTargetPrice` |
| Supply / demand | `GetSupply`, `GetDemand`, `GetActualSupply`, `GetActualDemand` |
| For price calc | `GetSupplyForPriceCalculations`, `GetDemandForPriceCalculations` |
| Balance / stockpile | surplus helpers, `GetStockpile` |
| Price bounds / history | `GetMinPrice`, `GetMaxPrice`, `GetPriceHistory` |
| Default reference | `GetGoods.GetDefaultMarketPrice` (GUI) |

`PRICE_TOOLTIP` loc: target price from base price + effective supply/demand + price stability.

# Script access (verified in vanilla content)

| Need | Script | Scope | Notes |
|------|--------|-------|-------|
| Live unit price | `price_in_market` | goods + `market =` | Trigger + script_value string; see [price_in_market](/economy/price-in-market-script-api.md) |
| Current good output | `raw_material_output` | location | Trigger; current RGO only |
| Current price vs default | `relative_raw_material_price` | location | % of base price for **installed** good |
| Market surplus | `is_in_surplus_in_market` | market + goods | Columbian guard pattern |
| Static default price | `default_market_price` in goods files | data | Not live; tooling / tiers |
| Change RGO good | `change_raw_material` | location effect | [Change raw material](/economy/change-raw-material.md) |

**Not found in script** (GUI / `data_types` only): `GetIncomePerLevel`, `GetProfitPerLevel`, `GetRGOProfitPerLevel`, `GetMaxRGOIncome` for hypothetical goods on a tile.

Before using GUI helpers in `scripted_effects`, DD-spike whether a script_value equivalent exists in your game version.

# Same-tile comparison (mods converting RGO)

Vanilla expand UI does not always show **alternate** goods on one tile. When scoring convert A → B on the **same** location:

**If** output per level is **approximately equal** across allowed goods on that tile (validate in-game per tile type), **and** market access and control are shared:

```
wealth_per_level(g)   ∝ O_tile × P(g)
income_per_level(g)   ∝ O_tile × P(g) × f(control, access, estates, tax_rates)

margin(g vs current)  ≈ P(g) / P(current) − 1     (shared factors cancel)
```

**If** output differs materially per good (`GetEfficiency`, terrain), use full revenue:

```
margin ≈ (O_target × P_target) / (O_current × P_current) − 1
```

Script implementation when output is shared: compare `price_in_market` for each candidate vs current on `location.market` (Columbian `ai_will_do` pattern). Product mods set their own margin thresholds and location gates in **product** design docs — not in this KB.

When defines hypothesis `O(g) ∝ 1/D(g)` holds, price-index `(P/D)` approximates revenue margin; when **flat `O_tile`** holds, live **price ratio** matches income-per-level ranking. See [RGO baseline output](/economy/rgo-baseline-output-and-price-balance.md).

# What not to use as output proxy

| Field | Why |
|-------|-----|
| `food` on goods | Nutrition layer, not goods-units output |
| `base_production` | Ambient common-good trickle, not RGO per level |
| `default_market_price` alone | Not live market |

See [Goods food vs market price](/economy/goods-food-vs-market-price.md).

# Related

* [price_in_market in script](/economy/price-in-market-script-api.md)
* [RGO baseline output and price balance](/economy/rgo-baseline-output-and-price-balance.md)
* [RGO conversion AI market decision](/economy/rgo-conversion-ai-market-decision.md) — pulse / Columbian patterns (no product thresholds)
* [Columbian Exchange RGO pattern](/economy/columbian-exchange-rgo-pattern.md)

# Citations

[1] `game/main_menu/localization/english/game_concepts_l_english.yml`
[2] `game/in_game/gui/expand_raw_goods_lateralview.gui`, `location_tooltips.gui`, `goods_production_lateralview.gui`
[3] `game/main_menu/localization/english/interfaces_l_english.yml`, `lists_l_english.yml`
[4] `logs/data_types/data_types_uncategorized.txt`
[5] `game/in_game/common/trigger_localization/` — `raw_material_output`, `relative_raw_material_price`, `price_in_market`
