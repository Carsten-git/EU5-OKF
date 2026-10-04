---
type: Playbook
title: Land good as development sink
description: Model "available land" as a synthetic good supplied by development and consumed by building production methods — MnT design pattern.
tags: [economy, goods, development, buildings, balance]
timestamp: 2026-07-20T21:45:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1"
---

Vanilla ties building expansion to pops, caps, and goods chains — but not a explicit **land budget**. MnT's changelog describes a **`land` good**: a base resource created by development, consumed by basic buildings that need physical land.

**Repo status (2026-07):** the goods definition and PM `land = …` inputs exist in GitHub as **commented stubs** (`MnT_special.txt`, `common_buildings.txt`). The pattern is documented here as an intentional design you can enable or adapt; do not assume `goods:land` is active in shipped Workshop builds without verifying.

# Problem

Slow early expansion and make dense development costly without arbitrary building caps alone. Land becomes a **market-traded constraint** like lumber or clay.

# Goods definition (template)

From `in_game/common/goods/MnT_special.txt` (commented):

```txt
# land = {
# 	category = raw_material
# 	color = goods_land
# 	default_market_price = 2
# 	transport_cost = 10
# 	base_production = 1
# }
```

Register loc in `main_menu/localization/english/MnT_goods_l_english.yml`:

```yaml
# land: "Land"
# land_desc: "Available land that can be used."
```

Use `main_menu` for goods loc ([mod folder structure](/getting-started/mod-folder-structure.md)).

# Supply side — development → land

Typical implementation options (MnT intent; pick one per mod):

| Approach | Technique |
|----------|-----------|
| Auto modifier | `auto_modifiers` `scales_with = development` adding local `land` output or a script_value proxy |
| Monthly pulse | Hidden event adds `add_goods_supply` / market injection proportional to `development` |
| Pseudo-RGO | Treat land like a hidden building category with `produced = land` capped by dev |

Goal: **high-dev tiles generate more land supply** on their market; frontier tiles stay cheap to build on until developed.

# Demand side — buildings consume land

Production method maintenance blocks reference the good:

```txt
PM_some_building = {
	inputs = {
		# land = 0.1   # MnT stub in common_buildings.txt
		lumber = 0.5
	}
}
```

Commented lines in MnT `common_buildings.txt` show planned `land = 0.02`–`0.1` on rural PMs. Tune **per building tier** so cities pay more land than villages.

# Balance workflow

1. Define land good + loc.
2. Wire supply to development (script_value documented in spreadsheet).
3. Add land inputs to PMs incrementally; run observer + [telemetry toolchain](/tooling/total-conversion-toolchain.md).
4. Watch market prices for land in high-dev regions vs frontiers.

Pair with [Goods demand construction baskets](/economy/goods-demand-construction-baskets.md) and [Numeric rebalance playbook](/total-conversion/numeric-rebalance-playbook.md).

# Related

* [RGO baseline output and price balance](/economy/rgo-baseline-output-and-price-balance.md)
* [Building cap script values](/buildings/building-cap-script-values.md) — complementary cap, not replacement
* [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md) — changelog: "'Land' good added…"

# Citations

[1] MnT-EU5 `in_game/common/goods/MnT_special.txt` — commented `land` def
[2] MnT-EU5 `in_game/common/building_types/common_buildings.txt` — commented PM `land` inputs
[3] MnT-EU5 `Documentation/Change log.md` — design intent
[4] MnT-EU5 `main_menu/localization/english/MnT_goods_l_english.yml` — commented loc keys
