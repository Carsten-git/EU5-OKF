---
type: Reference
title: Vanilla master data index
description: Where to find goods, prices, locations, markets, RGO costs, and runtime APIs in EU5 vanilla game files — quick lookup for modders and agents.
tags: [reference, vanilla, goods, locations, markets, rgo, meta]
timestamp: 2026-07-15T20:30:00+10:00
status: complete
source_mod: vanilla
---

Single map of **where master data lives** in the vanilla install (`game/`). Paths are relative to the game root unless noted.

# Game install layout

| Root | Role |
|------|------|
| `game/in_game/` | Simulation content loaded in campaign (goods, events, map, script) |
| `game/main_menu/` | Lobby, localization, shared UI strings |
| `game/loading_screen/common/defines/` | Engine constants (`00_defines.txt`) |
| `logs/data_types/` | Dumped script/GUI API names (after running game with logging) |

# Goods (commodities)

| What | Where | Key fields |
|------|-------|------------|
| **All goods definitions** | `in_game/common/goods/*.txt` | `category`, `method`, `default_market_price`, `food`, `base_production`, `transport_cost`, `demand_*`, `max_rgo_size`, `location_potential` |
| **Field readme** | `in_game/common/goods/readme.txt` | Documents each goods-block key |
| **Raw materials** | Same files; filter `category = raw_material` | `00_raw_materials.txt`, `03_food.txt`, … |
| **Produced goods** | `category = produced` in goods files | Buildings / urban production |
| **Goods demand** | `in_game/common/goods_demand/` | Construction baskets, pop needs |
| **Default unit price (static)** | `default_market_price` on each good | **Not** live market price |
| **Live unit price (runtime)** | Script: `price_in_market` on goods + market scope | See [price_in_market API](/economy/price-in-market-script-api.md) |
| **GUI default price** | `Goods.GetDefaultMarketPrice` | `logs/data_types/` — not verified in script |

# RGO (resource gathering)

| What | Where | Notes |
|------|-------|-------|
| **Per-good method** | `method = farming/mining/forestry/gathering/hunting` on goods | Drives expand price bucket |
| **Expand level base prices** | `in_game/common/prices/00_hardcoded.txt` | `expand_rgo_mining`, `expand_rgo_farming`, … — each `gold = 100` in vanilla |
| **Price scaling on expand cost** | Engine + defines `GOODS_RGO_PRICE_SCALE` | UI: `RGO_BUILD_GOODS_PRICE_IMPACT_ON_COST` in `economy_l_english.yml` — cost scales with good’s **default market price** |
| **Expand cost modifiers** | Modifiers `expand_rgo_*_cost_modifier` | Per method; see `modifier_types_l_english.yml` |
| **RGO timing** | `loading_screen/common/defines/00_defines.txt` | `RGO_BASE_TIME = 180`, `RGO_LOAD_TIME_FRACTION`, … |
| **Output / income balance defines** | Same defines file | `GOODS_RGO_BASE_COST`, `GOODS_RGO_PRICE_SCALE` — see [RGO baseline output](/economy/rgo-baseline-output-and-price-balance.md) |
| **Change raw material** | `change_raw_material` effect | [Change raw material](/economy/change-raw-material.md) |
| **Columbian-style AI** | `in_game/common/generic_actions/columbian_exchange.txt` | [CE pattern](/economy/columbian-exchange-rgo-pattern.md) |

# Locations and map

| What | Where | Key fields |
|------|-------|------------|
| **Location templates (setup)** | `in_game/map_data/location_templates.txt` | `topography`, `vegetation`, `climate`, `religion`, `culture`, `raw_material`, `modifier`, `natural_harbor_suitability` |
| **Named location IDs** | `in_game/map_data/named_locations/*.txt` | Internal id → map color / province link |
| **Map structure** | `in_game/map_data/default.map`, `definitions.txt` | Provinces, continents |
| **Location script triggers** | `in_game/common/trigger_localization/location_triggers.txt` | `raw_material_output`, `relative_raw_material_price`, … |
| **Location scripted effects** | `in_game/common/scripted_effects/location_effects.txt` | `change_raw_material`, etc. |
| **Runtime output (current good)** | Location scope `raw_material_output` | Triggers, missions, `order_by` — **current** RGO only |
| **Runtime output (UI, any good)** | `Location.GetRawMaterialsOutput`, `RawGoodLocationItem.GetIncomePerLevel`, `GoodItem.GetMaxRGOIncome` | `logs/data_types/` — GUI; script exposure unvalidated |

**Important:** Per-level output on a **specific tile** is **not** a single static table — it is computed from good, method, topography, vegetation, development, modifiers, and defines. Location files give **which** raw material spawns, not monthly output quantity.

# Markets and food

| What | Where | Notes |
|------|-------|-------|
| **Goods market price** | Runtime `price_in_market(good, market)` | Per **named good** |
| **Food stockpile price** | Market triggers: `food_price` | Abstract food layer — not wheat vs rice |
| **Food on goods** | `food =` in goods file | Nutrition provision, not commodity quantity |
| **Market creation cost** | `prices/00_hardcoded.txt` — `create_market` | |

See [Goods food vs market price](/economy/goods-food-vs-market-price.md).

# Prices and costs (other)

| What | Where |
|------|-------|
| **Hardcoded action prices** | `in_game/common/prices/00_hardcoded.txt` |
| **Building prices** | Referenced from `building_types/` via `price =` |
| **Construction baskets** | `goods_demand/` |

# Localization

| What | Where |
|------|-------|
| **In-game English** | `main_menu/localization/english/*.yml` |
| **Tooltips (RGO, economy)** | `interfaces_l_english.yml`, `economy_l_english.yml`, `game_concepts_l_english.yml` |
| **Goods names** | `goods_l_english.yml` |
| **BOM requirement** | [UTF-8 BOM](/localization/utf8-bom-requirement.md) |

# Derived / compiled tables in this KB

| Topic | Article |
|-------|---------|
| All raw materials + default price + inferred output/lv | [RGO baseline output and price balance](/economy/rgo-baseline-output-and-price-balance.md) |
| Live price in script | [price_in_market](/economy/price-in-market-script-api.md) |
| Conversion AI scoring | [RGO conversion AI market decision](/economy/rgo-conversion-ai-market-decision.md) |

# Agent workflow

1. **Static good facts** → parse `in_game/common/goods/`
2. **Who can grow what** → `location_templates.txt` `raw_material` + goods `location_potential`
3. **Live economics** → `price_in_market` + `location.raw_material_output` (current good)
4. **Engine constants** → `loading_screen/common/defines/00_defines.txt`
5. **Unsure API** → `logs/data_types/data_types_uncategorized.txt` after a game run

# Citations

[1] `game/in_game/common/goods/readme.txt`
[2] `game/in_game/map_data/location_templates.txt`
[3] `game/in_game/common/prices/00_hardcoded.txt`
[4] `game/loading_screen/common/defines/00_defines.txt`
[5] `game/main_menu/localization/english/economy_l_english.yml` — `RGO_BUILD_GOODS_PRICE_IMPACT_ON_COST`
