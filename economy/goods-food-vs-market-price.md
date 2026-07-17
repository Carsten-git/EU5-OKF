---
type: Reference
title: Goods food vs market price
description: food on goods definitions is nutrition provision — not goods output quantity; markets price named goods via price_in_market.
tags: [economy, markets, goods, food, rgo, script-values]
timestamp: 2026-07-15T19:30:00+10:00
status: complete
source_mod: vanilla
---

Three different numbers appear on raw-material goods. **Do not mix them** when scoring RGO conversion or revenue.

# What the market prices

Markets trade **named goods** (`goods:wheat`, `goods:pepper`, `goods:wool`, …) via `price_in_market` on a `market` scope.

This is separate from the **food stockpile** layer (`food_price`, `market_food`, provincial food buy/sell). Food raw materials *also* produce nutrition (`food` on the goods definition), but the wheat **commodity** still has its own goods price on the market.

| Layer | What moves | Script / UI |
|-------|------------|---------------|
| **Goods market** | Specific commodities | `price_in_market` (goods + market) |
| **Food economy** | Abstract food units for pops/armies | `food` on goods; `food_price` on market; `local_food` on location |

Player RGO income tooltip: `GetRawMaterialsOutput` goods units sold for `GetRawMaterialsPrice` gold = **output × unit goods price** on `location.market`.

# Goods definition fields (do not conflate)

From `common/goods/readme.txt` and `game_concepts`:

| Field | Meaning | Use in RGO revenue? |
|-------|---------|-------------------|
| `default_market_price` | Baseline goods unit price | Index denominator for live-price shocks; **not** live price |
| `food` | Nutrition from this good (food layer) | **No** — not goods output quantity |
| `base_production` | Ambient output of **common goods** everywhere without RGO (clay, sand, stone, lumber, wool) | **No** for tile RGO — tiny values, wrong units |
| `raw_material_output` | Location trigger: current RGO goods output on tile | **Yes** for current good; no verified hypothetical-good form |

# Broken proxies (do not use for revenue)

Using `revenue = price × food` or `price × base_production` produces absurd spreads:

| Good | Default price | food | base_production | broken food rev | broken bp rev |
|------|---------------|------|-----------------|-----------------|---------------|
| livestock | 1.5 | 8 | 0 | **12.0** | 1.5 |
| wool | 2.5 | 0 | 0.0005 | 2.5 | **0.0013** |
| wheat | 1.0 | 8 | 0 | 8.0 | 1.0 |

Livestock “wins” almost everything under the food proxy → livestock spam. Wool collapses under base_production.

# Correct mental model

```
tile_revenue(good) = goods_output_on_tile(good) × price_in_market(good, location.market)
```

- **Current good:** `location.raw_material_output` × `price_in_market(current, market)` (script-verified pieces).
- **Hypothetical good:** engine UI `RawGoodLocationItem.GetIncomePerLevel` / `GoodItem.GetMaxRGOIncome` — **not verified in script**.

# Related

* [RGO baseline output and price balance](/economy/rgo-baseline-output-and-price-balance.md)
* [RGO conversion AI market decision](/economy/rgo-conversion-ai-market-decision.md)
* [price_in_market in script](/economy/price-in-market-script-api.md)

# Citations

[1] `game/in_game/common/goods/readme.txt` — `food`, `base_production`
[2] `game/main_menu/localization/english/game_concepts_l_english.yml` — `game_concept_food`, `game_concept_market_food`
[3] `game/main_menu/localization/english/interfaces_l_english.yml` — `RAW_MATERIAL_LOCATION_TABLE_TITLE_TT`
