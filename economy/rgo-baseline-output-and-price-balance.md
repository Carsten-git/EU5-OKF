---
type: Reference
title: RGO baseline output and price balance
description: Vanilla raw-material default prices, defines output hypothesis, and how flat per-tile output changes conversion scoring vs price index.
tags: [economy, rgo, goods, markets, defines]
timestamp: 2026-07-15T21:00:00+10:00
status: complete
source_mod: vanilla
---

Engine **design** may target flat `output × price` at default prices via defines (see below). **Measured output per level on real tiles** can differ from the defines table — validate in-game before choosing a conversion scoring model. See [RGO UI profit metrics](/economy/rgo-ui-profit-metrics.md).

# Profit per level (player / wiki)

From the EU5 wiki and location tooltips:

```
profit_per_level = output_per_level × market_price × market_access × control
```

`output_per_level` on a tile is **location-specific** (terrain, development, modifiers). The goods file does **not** store a universal per-level output constant.

# Inferred baseline output (defines hypothesis)

`loading_screen/common/defines/00_defines.txt` (economy group):

| Define | Vanilla value |
|--------|---------------|
| `GOODS_RGO_BASE_COST` | `0.5` |
| `GOODS_RGO_PRICE_SCALE` | `0.25` |

## How output is inferred (not circular)

**Step 1 — defines only:** read `BASE_COST` and `PRICE_SCALE` from engine constants.

**Step 2 — hypothesized output per level** (goods-units, unmodified flat tile):

```
O(g) = GOODS_RGO_BASE_COST / (default_market_price(g) × GOODS_RGO_PRICE_SCALE)
```

**Step 3 — multiply by default price** (algebra, not assumption):

```
O(g) × D(g) = BASE_COST / PRICE_SCALE = 0.5 / 0.25 = 2.0 gold per level
```

The **2.0 gold** income at default prices is a **consequence** of the defines + goods prices. It was not plugged in as a target. **Validate in-game** on a reference location — real `O` varies with topography, vegetation, development, and modifiers.

**Status:** inferred; not documented by Paradox as a public formula.

# Measured output vs defines table

UI **output per level** = `GetRawMaterialsOutput / GetMaxRGOWorkers` (current good). On some tiles, output is **near-constant across goods** on the same location; on others it may track the defines `O(g)` spread or `GetEfficiency` per candidate good.

| Observation | Scoring implication |
|-------------|---------------------|
| Flat `O_tile` across goods | Live **price ratio** ≈ income/level ranking — [UI profit metrics](/economy/rgo-ui-profit-metrics.md) |
| `O(g)` varies per good | Compare `O×P`; defines table is a starting guess only |
| Modifiers / development | Real `O` below defines 2.0 gold/level identity |

Always validate on reference locations before encoding AI logic.

## Expand cost scales with default price too

| Piece | Where |
|-------|--------|
| Method base gold | `in_game/common/prices/00_hardcoded.txt` — `expand_rgo_mining`, `expand_rgo_farming`, … (`gold = 100` each in vanilla) |
| Per-good scaling | Engine × `default_market_price` × `GOODS_RGO_PRICE_SCALE` | UI: `RGO_BUILD_GOODS_PRICE_IMPACT_ON_COST` |

Hypothesized expand gold for one level:

```
expand_gold(g) ≈ 100 × default_market_price(g) × 0.25 = 25 × D(g)
```

At baseline income **2.0** gold/level/month, payback months ≈ `12.5 × D(g)` — pepper (~5) takes ~5× longer than wheat (~1) to repay expansion, matching higher tier cost.

Method (`mining` vs `farming`) picks which `expand_rgo_*` base row applies; **within** a method, cost differs by good via default price.

# All vanilla raw materials (default price + inferred output)

Sorted by `default_market_price`. `est_out/lv` = inferred goods-units per level; `est_inc/lv` = `est_out × default_price`.

| Good | Default price | Method | food | base_production | max_rgo | est_out/lv | est_inc/lv |
|------|---------------|--------|------|-----------------|---------|------------|------------|
| clay | 0.50 | gathering | — | 0.02 | 1.0 | 4.000 | 2.000 |
| sand | 0.50 | gathering | — | 0.01 | 1.0 | 4.000 | 2.000 |
| stone | 1.00 | mining | — | 0.012 | 1.0 | 2.000 | 2.000 |
| medicaments | 1.00 | gathering | — | — | 1.0 | 2.000 | 2.000 |
| wild_game | 1.00 | hunting | 3.5 | — | 1.0 | 2.000 | 2.000 |
| fish | 1.00 | gathering | 5.0 | — | 1.0 | 2.000 | 2.000 |
| wheat | 1.00 | farming | 8.0 | — | 1.0 | 2.000 | 2.000 |
| maize | 1.00 | farming | 8.0 | — | 1.0 | 2.000 | 2.000 |
| rice | 1.00 | farming | 10.0 | — | 1.0 | 2.000 | 2.000 |
| millet | 1.00 | farming | 5.0 | — | 1.0 | 2.000 | 2.000 |
| legumes | 1.00 | farming | 5.0 | — | 1.0 | 2.000 | 2.000 |
| potato | 1.00 | farming | 8.0 | — | 1.0 | 2.000 | 2.000 |
| olives | 1.00 | farming | 4.0 | — | 1.0 | 2.000 | 2.000 |
| fruit | 1.00 | farming | 4.0 | — | 1.0 | 2.000 | 2.000 |
| lumber | 1.50 | forestry | — | 0.01 | 1.0 | 1.333 | 2.000 |
| livestock | 1.50 | farming | 8.0 | — | 1.0 | 1.333 | 2.000 |
| coal | 2.00 | mining | — | — | 1.0 | 1.000 | 2.000 |
| tin | 2.00 | mining | — | — | 1.0 | 1.000 | 2.000 |
| lead | 2.00 | mining | — | — | 1.0 | 1.000 | 2.000 |
| fiber_crops | 2.00 | farming | — | — | 1.0 | 1.000 | 2.000 |
| saltpeter | 2.00 | mining | — | — | 1.0 | 1.000 | 2.000 |
| wine | 2.00 | farming | — | — | 1.0 | 1.000 | 2.000 |
| fur | 2.00 | hunting | 2.0 | — | 1.0 | 1.000 | 2.000 |
| beeswax | 2.00 | farming | 2.5 | — | 1.0 | 1.000 | 2.000 |
| incense | 2.50 | farming | — | — | 1.0 | 0.800 | 2.000 |
| wool | 2.50 | farming | — | 0.0005 | 1.0 | 0.800 | 2.000 |
| horses | 3.00 | farming | — | — | 1.0 | 0.667 | 2.000 |
| iron | 3.00 | mining | — | — | 1.0 | 0.667 | 2.000 |
| copper | 3.00 | mining | — | — | 1.0 | 0.667 | 2.000 |
| tea | 3.00 | farming | — | — | 1.0 | 0.667 | 2.000 |
| coffee | 3.00 | farming | — | — | 1.0 | 0.667 | 2.000 |
| alum | 3.00 | mining | — | — | 1.0 | 0.667 | 2.000 |
| mercury | 3.00 | mining | — | — | 1.0 | 0.667 | 2.000 |
| cotton | 3.00 | farming | — | — | 1.0 | 0.667 | 2.000 |
| sugar | 3.00 | farming | — | — | 1.0 | 0.667 | 2.000 |
| tobacco | 3.00 | farming | — | — | 1.0 | 0.667 | 2.000 |
| silk | 4.00 | farming | — | — | 1.0 | 0.500 | 2.000 |
| dyes | 4.00 | farming | — | — | 0.5 | 0.500 | 2.000 |
| cocoa | 4.00 | farming | — | — | 1.0 | 0.500 | 2.000 |
| ivory | 4.00 | hunting | — | — | 1.0 | 0.500 | 2.000 |
| salt | 4.00 | gathering | — | — | 1.0 | 0.500 | 2.000 |
| gems | 4.00 | mining | — | — | 1.0 | 0.500 | 2.000 |
| pearls | 4.00 | gathering | — | — | 1.0 | 0.500 | 2.000 |
| amber | 4.00 | gathering | — | — | 1.0 | 0.500 | 2.000 |
| elephants | 5.00 | farming | — | — | 1.0 | 0.400 | 2.000 |
| marble | 5.00 | mining | — | — | 1.0 | 0.400 | 2.000 |
| saffron | 5.00 | farming | — | — | 1.0 | 0.400 | 2.000 |
| pepper | 5.00 | farming | — | — | 1.0 | 0.400 | 2.000 |
| cloves | 5.00 | farming | — | — | 1.0 | 0.400 | 2.000 |
| chili | 5.00 | farming | — | — | 1.0 | 0.400 | 2.000 |
| silver | 6.00 | mining | — | — | 1.0 | 0.333 | 2.000 |
| goods_gold | 8.00 | mining | — | — | 1.0 | 0.250 | 2.000 |

`—` = field absent (0). `food` shown only when &gt; 0 (nutrition, not goods output).

# Implication for conversion scoring

| Scoring approach | When `O_tile` flat on location | When `O(g)` varies per good |
|------------------|-------------------------------|-----------------------------|
| **Live price ratio** `P_tgt/P_cur − 1` | Matches income/level UI ranking | Wrong if outputs differ |
| **Price index** `(P_tgt/D_tgt)/(P_cur/D_cur) − 1` | Suppresses moves at default prices | Fits defines-balanced `O(g)` |
| **Defines `O(g) × P(g)`** | Overfits if real `O_tile` flat | Starting guess; validate per tile |
| **`food` / `base_production`** | **Broken** | Do not use |

Patterns (no product thresholds): [RGO conversion AI market decision](/economy/rgo-conversion-ai-market-decision.md). APIs: [RGO UI profit metrics](/economy/rgo-ui-profit-metrics.md).

# Related

* [RGO UI profit metrics](/economy/rgo-ui-profit-metrics.md)
* [Vanilla master data index](/references/vanilla-master-data-index.md) — where to find goods, locations, prices in game files
* [RGO conversion AI market decision](/economy/rgo-conversion-ai-market-decision.md)
* [price_in_market in script](/economy/price-in-market-script-api.md)
* [Change raw material](/economy/change-raw-material.md)

# Citations

[1] `game/loading_screen/common/defines/00_defines.txt` — `GOODS_RGO_BASE_COST`, `GOODS_RGO_PRICE_SCALE`
[2] `game/in_game/common/goods/` — `default_market_price`, `category = raw_material`
[3] `game/main_menu/localization/english/interfaces_l_english.yml` — `RAW_MATERIAL_LOCATION_TABLE_CURRENT_OUTPUT_TT_TEXT`
[4] EU5 wiki — Resource gathering operation (profit per level formula)
