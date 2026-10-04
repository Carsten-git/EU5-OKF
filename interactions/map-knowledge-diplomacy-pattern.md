---
type: Reference
title: Map knowledge diplomacy pattern
description: Gold buy/sell of area cartography via economy country_interactions — reference implementation Tradeable Maps 0.1.
tags: [country-interactions, exploration, discover_area, tradeable_maps]
timestamp: 2026-07-27T07:55:00+10:00
status: complete
source_mod: tradeable_maps
source_version: "0.1"
---

Worked example: **Tradeable Maps** — human player buys/sells **area** map knowledge with gold. General mechanics live in [Country interactions — economy diplomacy and gold](/interactions/country-interactions-economy-diplomacy-gold.md).

# Mod file map

| Path | Role |
|------|------|
| `in_game/common/country_interactions/tradeable_maps_request_area_purchase.txt` | Buy |
| `in_game/common/country_interactions/tradeable_maps_offer_area_sale.txt` | Sell |
| `in_game/common/script_values/tradeable_maps_prices.txt` | Relation **P** + price factor |
| `in_game/common/prices/tradeable_maps_diplomacy.txt` | `gold = 750` base price |
| `main_menu/localization/english/tradeable_maps_l_english.yml` | Workshop-facing strings (+ `in_game` mirror) |

# Product rules (0.1)

- Launcher on/off only — no game rule.
- Human-initiated; AI does not unprompted map trade.
- Vanilla **Steal Maps** / **Share Maps** unchanged.
- Sell: buyer `opinion(actor) >= 0` and buyer `gold >=` price.

# Pricing sketch

**Gold = clamp(750 × (1 + P/100), 375, 7500)** with **P** from ally, subject, priced opinion, rival, antagonism (`recipient` view of `actor`).

Implementation: base **750** in `prices/`, `price_modifier` = factor script value (not flat gold in modifier alone).

# Workshop changelog

Steam BBCode release notes: `STEAM_UPDATE_0.1.0.bbcode` (+ `_PLAIN` fallback) in mod root. See [Steam Workshop BBCode changelog](/tooling/steam-workshop-bbcode-changelog.md).

# Citations

[1] `mod/Tradeable Maps/` — REQ-001 / SO-script-values implementation
[2] Product OKF `development/Tradeable Maps/` (design deltas, not engineering KB)
