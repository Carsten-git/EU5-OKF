---
type: Playbook
title: Goods demand construction baskets
description: Named goods baskets for construction/maintenance on buildings, roads, and ships — including the 1% road upkeep rule.
tags: [economy, goods-demand, roads, buildings]
timestamp: 2026-07-11T12:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1.6"
---

Goods demand is a **named basket registry**. Define baskets in `goods_demand/`, assign via `construction_demand` / `maintenance_demand`.

# Basket schema

```txt
REPLACE:build_gravel_road_demand = {
	lumber = 0.25
	masonry = 0.25
	category = building_construction
}

# maintain ≈ 1% of build per good (MnT design rule)
REPLACE:maintain_gravel_road_demand = {
	lumber = 0.0025
	masonry = 0.0025
	category = building_maintenance
}
```

Categories include `building_construction`, `building_maintenance`, `ship_construction`, …

# Wire to consumers

```txt
REPLACE:gravel_road = {
	construction_demand = build_gravel_road_demand
	maintenance_demand = maintain_gravel_road_demand
}

farming_village = {
	construction_demand = village_construction   # or mod basket
}
```

Add mod-only baskets (`rgo_constructions_lumber`) for new building families; `REPLACE` vanilla keys when retuning.

# Navy parallel

Ship construction baskets use `category = ship_construction`; unit types reference them the same way. MnT often scales goods up to shift cost from “magic ducats” to market goods.

# Related

* [Proximity vs control](/map/proximity-vs-control.md) — roads also affect proximity
* [Numeric rebalance playbook](/total-conversion/numeric-rebalance-playbook.md)

# Citations

[1] MnT `in_game/common/goods_demand/MnT_special_construction_demands.txt`
[2] MnT `in_game/common/goods_demand/MnT_building_construction_costs.txt`
[3] MnT `in_game/common/road_types/MnT_generic.txt`
[4] MnT `in_game/common/goods_demand/navy_demands.txt`
