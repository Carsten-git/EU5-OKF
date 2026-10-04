---
type: Playbook
title: REPLACE generic actions
description: Patch vanilla generic_actions (markets, exchange, estate bribes) with REPLACE blocks, custom select_trigger, and price-aware ai_will_do.
tags: [interactions, generic-actions, replace, ai, markets]
timestamp: 2026-07-20T21:30:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1"
---

`in_game/common/generic_actions/` defines country- and market-scoped player/AI actions (destroy market, Columbian exchange steps, estate bribes). For surgical changes without duplicating IDs, EU5 supports **`REPLACE:action_key`** blocks that merge with vanilla.

MnT uses this for market destruction, Columbian exchange, and estate interactions.

# Pattern

```txt
REPLACE:destroy_market = {
	type = owncountry
	# ... full or partial override of vanilla fields ...

	select_trigger = {
		looking_for_a = market
		source = actor
		target_flag = target
		# custom visible / enabled rules
		enabled = {
			has_temporary_demands = no
		}
	}

	effect = {
		scope:target = {
			location = { save_scope_as = old_location }
			destroy_market = {}
		}
	}

	ai_will_do = {
		add = {
			every_goods = {
				value = "price_in_market(scope:target.location.second_best_market)"
				subtract = "price_in_market(scope:target)"
			}
		}
		add = "scope:actor.destroy_market_utility(scope:target)"
	}
}
```

# Techniques demonstrated

| Technique | Example in MnT |
|-----------|----------------|
| `REPLACE:` key | `destroy_market`, Columbian exchange actions |
| Subject market rules in `visible` | Owner or subject with `overlord_can_destroy_markets = yes` |
| Block when temp demands active | `has_temporary_demands = no` on `enabled` |
| `price_in_market` in `ai_will_do` | Compare target market vs `second_best_market` per good |
| Engine utility in AI | `destroy_market_utility(scope:target)` alongside script math |
| `ai_tick = never` | Disable vanilla AI tick when custom `ai_will_do` handles it |

# When to use REPLACE vs INJECT

| Approach | Use when |
|----------|----------|
| `REPLACE:action_key` | You own the full action behavior or need to replace `select_trigger` / `effect` wholesale |
| `INJECT` into vanilla file | Adding a field to an existing block without re-listing the whole action |
| New action key | Vanilla has no equivalent; no collision risk |

See [Three-root and override ladder](/total-conversion/three-root-and-override-ladder.md).

# Related

* [price_in_market script API](/economy/price-in-market-script-api.md)
* [Hidden event economy pipeline](/economy/hidden-event-economy-pipeline.md) — `add_temporary_demand` interacts with market actions
* [Character interaction pre-evaluation](/interactions/character-interaction-pre-evaluation.md) — different interaction type, similar AI cost thinking

# Citations

[1] MnT-EU5 `in_game/common/generic_actions/MnT_markets.txt`
[2] MnT-EU5 `in_game/common/generic_actions/MnT_columbian_exchange.txt`
[3] MnT-EU5 `in_game/common/generic_actions/MnT_bribe_estate.txt`
