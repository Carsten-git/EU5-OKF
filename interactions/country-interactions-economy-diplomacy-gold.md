---
type: Playbook
title: Country interactions — economy diplomacy and gold
description: Add CATEGORY_ECONOMY_ACTIONS diplomacy with payer/payee, scripted prices, and deterministic accept — art purchase pattern.
tags: [country-interactions, diplomacy, economy, gold, pricing, script-values]
timestamp: 2026-07-27T07:55:00+10:00
status: complete
source_mod: tradeable_maps
source_version: "0.1"
---

Use when a mod needs **gold transfers** through the diplomacy UI (buy/sell), not covert actions or free gifts.

# Vanilla templates

| Flow | File | Payer | Payee |
|------|------|-------|-------|
| Player buys asset | `in_game/common/country_interactions/request_work_of_art_purchase.txt` | `scope:actor` | `scope:recipient` |
| Player sells asset | `in_game/common/country_interactions/sell_work_of_art.txt` | `scope:recipient` | `scope:actor` |

Both use `category = CATEGORY_ECONOMY_ACTIONS`, `type = diplomacy`, `price = price:…`, and `price_modifier`.

Read `in_game/common/country_interactions/readme.txt` for field semantics.

# Critical: `price_modifier` multiplies the base price

The readme states **`price_modifier` multiplies** the scripted `price` base — it does **not** replace it with a flat add.

| Base `gold` in `common/prices/` | Modifier value | Treasury delta |
|---------------------------------|----------------|----------------|
| `0` | `840` | **0** (UI may still show ~840 from modifier breakdown) |
| `750` | `1.12` (factor) | **840** |

**Pattern for a fixed scalable gold total:**

1. Define `price:your_key` with a **non-zero** base (e.g. `gold = 750`).
2. Set `price_modifier` to a **multiplier** script value (e.g. `tradeable_maps_map_price_factor` with `min` / `max`).

Art uses a small base (`gold = 2.5` / `1`) plus modifier carrying `art_price` — same multiply semantics.

# Payee is not automatic

Default payee: **nobody** (gold disappears). Set explicitly:

```txt
payer = scope:actor
payee = scope:recipient
```

Swap for sell flows (buyer pays seller).

# Deterministic “always accept” deals

Art uses `diplo_chance` + weighted `accept` (opinion, treasury, etc.). For **gold-gated** commerce (maps, fixed-price services):

- Omit `diplo_chance`.
- `accept = { value = 1000 }` — same pattern as vanilla `share_maps.txt`.
- Gate affordability in `allow` / country `enabled`: `gold >=` scripted total (or script value name as comparator).

Difficulty is **price only**: if the payer cannot afford the quote, the action is disabled; if they can and the deal is sent, it completes.

# Player-only actions

`potential = { scope:actor = { is_ai = no } }` — no `ai_tick` unprompted offers unless you add them.

# Area targeting (map knowledge)

Reuse vanilla area rules instead of inventing discovery logic:

| Action | Copy `visible` / `enabled` from |
|--------|----------------------------------|
| Buy knowledge | `steal_maps.txt` (recipient knows, actor not; neighbor discovered by actor) |
| Sell knowledge | `share_maps.txt` (actor knows, recipient not; neighbor on recipient) |

Effect: `discover_area = scope:target` on the **buyer** country scope. Commerce buys do **not** need `drop_antagonism_bomb` (steal_maps espionage only).

Optional gates on country pick: `NOT = { is_enemy_of = scope:actor }`, buyer `gold >=` price, sell-side `opinion(scope:actor) >= 0` on the buyer.

# Relation-based price in `script_values`

Centralize additive **P** (%) and final multiplier in `common/script_values/`:

- Opinion glide: `"scope:recipient.opinion(scope:actor)"` with separate positive/negative branches (see vanilla `io_policy.txt` / `hre_action_values.txt`).
- Antagonism: `"scope:recipient.antagonism(scope:actor)"`.
- Factor: `value = 1`, add `P/100`, `min` / `max` on the factor (precedent: `diplomatic_values.txt`).
- Final gold: `value = base`, `multiply = factor`, `min` / `max` on gold if needed.

Use `price_modifier = { value = your_factor }` when base gold holds the **B** in **B × factor**.

# Exploration overlap

EU5 distinguishes:

| Trigger | Meaning |
|---------|---------|
| `has_discovered_area` (country) | Used by steal_maps / map trade eligibility |
| `is_area_fully_discovered` (area + country) | Blocks **starting** new exploration when true |
| `is_being_explored` (area + country) | Active expedition on that area |

Buying map knowledge while exploring is usually **allowed** (area not fully discovered yet). Effect path:

1. `discover_area` — engine full discovery (same as steal_maps / share_maps).
2. Optional cleanup: `hidden_effect` + if `is_being_explored = scope:actor` then `cancel_area_exploration = scope:target` so no stale expedition card remains if the engine did not auto-clear.

Do **not** rely on scripts to document engine crash safety — `discover_area` is widely used in vanilla; mod adds no extra effect types.

# Localization

Mirror art: keys on interaction id — `tradeable_maps_request_area_purchase`, `PROPOSE_…`, `INCOMING_OFFER_…`, `INCOMING_OFFER_TITLE_…`. See `main_menu/localization/english/country_interactions_l_english.yml`.

# Related

* [Map knowledge diplomacy (reference)](/interactions/map-knowledge-diplomacy-pattern.md) — Tradeable Maps 0.1 walkthrough
* [Character interaction pre-evaluation](/interactions/character-interaction-pre-evaluation.md) — performance on large pick lists
* [Common pitfalls](/validation/common-pitfalls.md) — zero-gold payment row
* [Vanilla file locations](/references/vanilla-file-locations.md)

# Citations

[1] Tradeable Maps `mod/Tradeable Maps/in_game/common/country_interactions/`
[2] Vanilla `game/in_game/common/country_interactions/request_work_of_art_purchase.txt`, `sell_work_of_art.txt`, `steal_maps.txt`, `share_maps.txt`, `readme.txt`
[3] Vanilla `game/in_game/common/generic_actions/explorers.txt` — `cancel_area_exploration`, `is_being_explored`
