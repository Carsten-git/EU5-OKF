---
type: Playbook
title: Known issues registry
description: Documented bugs and agent mistakes from prior modding sessions — symptom, root cause, fix, and links.
tags: [validation, known-issues, agents, pitfalls, registry]
timestamp: 2026-07-06T19:00:00+10:00
status: complete
---

**For agents:** Read this file **before** editing EU5 mods in this workspace. Each entry is a real failure observed in development — not hypothetical. Prefer linking to the full article over re-discovering the bug.

# How to use

| Column | Meaning |
|--------|---------|
| **ID** | Stable reference (`KI-###`) — cite in commits or agent notes |
| **Symptom** | What the user or tester saw |
| **Root cause** | What was actually wrong |
| **Fix** | Minimal correction |
| **Article** | Deep dive in this bundle |

**Contributing:** When you fix a non-obvious bug, append a row here and link the detailed article. Append [log.md](/log.md).

# Registry

## Events & on-actions

| ID | Symptom | Root cause | Fix | Article |
|----|---------|------------|-----|---------|
| KI-001 | Purpose intro never pops up | Used invalid `trigger_event = { … }` — **not an EU5 effect** | `trigger_event_non_silently = flavor_….100` or block form with `id =` | [/events/triggering-events.md](/events/triggering-events.md) |
| KI-002 | Event fires but title/desc show raw keys like `flavor_x.0.title` | Event ID ends in **`.0`** — EU5 mishandles `.0` suffix | Rename to `.100` or higher | [/events/event-id-rules.md](/events/event-id-rules.md) |
| KI-003 | Fixed intro event still never shows on existing save | Old `teu_nc_purpose_guide_v2_seen` flag set from broken intro | Bump migration flag (e.g. `…_v3_seen`); fire from monthly pulse fallback | [/on-actions/country-pulses.md](/on-actions/country-pulses.md) |
| KI-004 | Tannenberg missing from DHE browser | `historical_info` was an **inline string** in script, not a loc key | `historical_info = flavor_teu_nc_tannenberg.1.historical_info` + yml entry | [/events/dhe-browser-visibility.md](/events/dhe-browser-visibility.md) |
| KI-005 | Only Tannenberg in DHE list — other mod events absent | Those events had no `dynamic_historical_event { }` block | Add DHE to anchor events; chain outcomes use `monthly_chance = 0` for browser-only listing | [/events/dynamic-historical-events.md](/events/dynamic-historical-events.md) |
| KI-006 | DHE shows `Does NOT have Variable: teu_nc_…` | Raw `has_variable` in event `trigger` without `custom_tooltip` | Wrap in `custom_tooltip = { text = loc_key … }` | [/events/event-triggers-and-options.md](/events/event-triggers-and-options.md) |
| KI-007 | Player can't tell how an event-chain outcome (e.g. Tannenberg battle) is decided | Outcome resolved by hidden `random_list` weights; nothing in the UI explains the factors or the timeline | Spell out the mechanic in the anchor event **desc** ("resolves in ~30 days, weighted by X, Y, stance") and add `custom_tooltip` per option stating its effect on the odds | [/events/event-triggers-and-options.md](/events/event-triggers-and-options.md) |
| KI-008 | Notification event appears to "do nothing" | Event is a pure tier/state notification; its consequences come from modifiers applied elsewhere | Add a `custom_tooltip` on the option pointing at the active modifier and how to recover | [/events/event-triggers-and-options.md](/events/event-triggers-and-options.md) |

## Localization

| ID | Symptom | Root cause | Fix | Article |
|----|---------|------------|-----|---------|
| KI-010 | All mod loc shows raw keys; advances/modifiers untranslated | yml saved **UTF-8 without BOM** | Write as `utf-8-sig` / UTF-8 with BOM | [/localization/utf8-bom-requirement.md](/localization/utf8-bom-requirement.md) |
| KI-075 | `error_log` shows year only / empty tag, loc, prices | Used `ROOT.*` or `ROOT.MakeScope.GetVariable` in loc from location telemetry; or read script prices via `GetMarket.GetPrice` only | Hidden `location_event` + `SCOPE.sLocation` / `SCOPE.sGoods`; stash script_values on `owner` with `save_scope_as` + `SCOPE.sCountry(…).MakeScope.GetVariable`; dual-log `ui_*` for UI price compare | [/validation/script-logging-and-telemetry.md](/validation/script-logging-and-telemetry.md), [/validation/script-telemetry-via-hidden-events.md](/validation/script-telemetry-via-hidden-events.md) |
| KI-074 | Game Rules screen shows `setting_<id>` / `rule_<id>` raw keys; other loc in same mod may work | Game-rule keys in a yml **without UTF-8 BOM** and/or not in a **paired** `*_game_rules_l_english.yml`; workshop pattern also mirrors keys under `in_game/localization/` | Pair `main_menu/common/game_rules/<mod>.txt` with `main_menu/localization/english/<mod>_l_english.yml` **and** duplicate under `in_game/localization/english/`; save **utf-8-sig**; keys `rule_<group>`, `setting_<setting>`, `setting_<setting>_desc` | [/game-rules/custom-game-rules.md](/game-rules/custom-game-rules.md), [/localization/utf8-bom-requirement.md](/localization/utf8-bom-requirement.md) |
| KI-011 | Event titles wrong; only some events localized | All events bundled in one yml instead of **one file per event script** | `flavor_teu_nc_purpose_l_english.yml` pairs with `flavor_teu_nc_purpose.txt` | [/localization/event-localization-naming.md](/localization/event-localization-naming.md) |
| KI-012 | Purpose score renders blank ("/100") in modifier tooltip | Neither `ROOT` nor `GetPlayer.MakeScope.GetVariable` resolves in `STATIC_MODIFIER_DESC_*` — modifier tooltips are scope-less | Generate **101 static readout modifiers** (`teu_nc_purpose_readout_0` … `_100`) with the score baked into `STATIC_MODIFIER_NAME_*`; monthly pulse swaps the active one via `teu_nc_refresh_purpose_readout` | [/localization/dynamic-text-in-loc.md](/localization/dynamic-text-in-loc.md) |
| KI-015 | Purpose score still invisible after `GetPlayer` attempt | Same root cause as KI-012 — confirmed `GetPlayer` does not work in static modifier loc either | Use the readout-swap pattern (KI-012); do not rely on any dynamic interpolation in modifier tooltips | [/modifiers/displaying-hidden-mechanics.md](/modifiers/displaying-hidden-mechanics.md) |
| KI-013 | Modifier name shows `Error Error Root.Custom(…)` | `ROOT.Custom()` and dynamic text in `STATIC_MODIFIER_NAME_*` not supported | Static **name**; dynamic score in **DESC** via `GetPlayer.MakeScope.GetVariable` (see KI-012); tier in separate modifier | [/modifiers/displaying-hidden-mechanics.md](/modifiers/displaying-hidden-mechanics.md) |
| KI-014 | Modifier list shows a junk stat line like `Monthly prestige = +0.00` | Dummy stat (`monthly_prestige = 0.001`) added just to make the modifier "visible" — it rounds to +0.00 | Stat-less static modifiers display fine (vanilla `hre_internal_peace_enforced` has only `game_data`); remove the dummy stat | [/modifiers/displaying-hidden-mechanics.md](/modifiers/displaying-hidden-mechanics.md) |

## Advances & modifiers

| ID | Symptom | Root cause | Fix | Article |
|----|---------|------------|-----|---------|
| KI-020 | Last Crusade / Amber Monopoly already researched day 1 | Mod advances lacked `starting_technology_level` above country's start level (TEU starts at 3) | Set `starting_technology_level = 4` / `5` on those advances | [/advances/starting-technology-level.md](/advances/starting-technology-level.md) |
| KI-021 | Purpose variable exists but player sees nothing | Variables have **no default UI** | Always-on country modifiers (meter + tier display) | [/modifiers/displaying-hidden-mechanics.md](/modifiers/displaying-hidden-mechanics.md) |
| KI-022 | Modifier applies but stat has no effect | Unknown key in static modifier definition | Grep `00_modifier_types.txt`; run `validate_mod.py` | [/modifiers/modifier-stat-keys.md](/modifiers/modifier-stat-keys.md) |
| KI-023 | Advance bonus only in desc hover, not stat line | Purpose +5 applied via on_action, not an advance stat key | Add visible stat on advance (e.g. `monthly_religious_influence`); one-shot rewards via pulse + desc | [/advances/advance-triggers-and-modifiers.md](/advances/advance-triggers-and-modifiers.md) |

## Formables & UI

| ID | Symptom | Root cause | Fix | Article |
|----|---------|------------|-----|---------|
| KI-050 | Mod formables missing from Form Country list | Narrow `potential` (tag-gated) + default `potential_requires_own = yes` | `capital_required = no`, `potential_requires_own = no`, widen `potential` to all branch tags | [/formables/formable-triggers-and-effects.md](/formables/formable-triggers-and-effects.md) |
| KI-051 | Trade formables (PRL, BDM) never appear | `rule = plausible` hidden by game rule **Only historical** | Enable **Allow plausible formables** in game rules | [/formables/formable-countries-overview.md](/formables/formable-countries-overview.md) |
| KI-052 | Formable lists itself as its own precondition ("Form the Prussian League") and hides real requirements (Tannenberg missing from Orderstaat) | Whole scripted trigger wrapped in **one outer `custom_tooltip`** in `allow` — outer tooltip text *replaces* all detailed inner tooltips; the text was the formable's tagline, not a requirement | Call the scripted trigger **directly** in `allow`; put `custom_tooltip` only on leaf conditions inside the trigger; phrase every tooltip as a requirement ("Has won…", "Playing as…"), never as an action | [/formables/formable-triggers-and-effects.md](/formables/formable-triggers-and-effects.md) |
| KI-053 | Wrong country can form a branch formable (TEU could form Prussian League directly) | Widening `potential` for visibility (KI-050) removed the old tag gating, and `allow` had no tag requirement | Every branch formable needs an explicit tag gate in `allow`, wrapped in a `custom_tooltip` ("Playing as the #Y Orderstaat#!…") so the player also learns *who* forms it | [/formables/formable-triggers-and-effects.md](/formables/formable-triggers-and-effects.md) |
| KI-054 | Formable list order looks random; paths invisible | `content_priority` values interleaved tiers across paths; names carried no path info | Group `content_priority` by path then tier (descending), and put path/step labels in the `_f` name + `_f_desc` (e.g. "Form Holy Prussia (Catholic Path — Step 2)") | [/formables/formable-countries-overview.md](/formables/formable-countries-overview.md) |

## Process & tooling

| ID | Symptom | Root cause | Fix | Article |
|----|---------|------------|-----|---------|
| KI-030 | Loc fix didn't apply after edit | Hot reload unreliable for yml | Full quit to desktop; relaunch EU5 | [/getting-started/enabling-your-mod.md](/getting-started/enabling-your-mod.md) |
| KI-031 | `on_game_start` change not seen in save | `on_game_start` runs once per **new game** only | Use `monthly_country_pulse` fallback for migration | [/on-actions/on-game-start.md](/on-actions/on-game-start.md) |
| KI-032 | Agent used EU4 wiki syntax | EU5 effects differ (e.g. no `trigger_event`) | Grep vanilla game install first | [/getting-started/reading-vanilla-examples.md](/getting-started/reading-vanilla-examples.md) |
| KI-060 | Lexer spam: script `.txt` “should be in utf8-bom encoding” | Script/data files saved UTF-8 **without** BOM | Save common/events scripts as UTF-8 **with** BOM; keep `metadata.json` **without** BOM | [/localization/utf8-bom-requirement.md](/localization/utf8-bom-requirement.md) |
| KI-061 | New-game errors / unstable start with location init | Used invalid **`every_location`** | Use `every_owned_location` (country) or `every_location_in_*`; prefer lazy capture | [/scripted-effects/location-iteration-effects.md](/scripted-effects/location-iteration-effects.md) |
| KI-062 | `construct_building` PostValidate false | `cost_multiplier` set without `cost_multiplier_reason` | Add `cost_multiplier_reason = "game_concept_event"` (or other loc key) | [/events/hidden-effects-and-ai-chance.md](/events/hidden-effects-and-ai-chance.md) |
| KI-063 | Custom location-panel button invisible | `visible = ScriptedGui.IsShown(...)` evaluated false | Gate visibility with GUI owner checks (e.g. `Location.GetOwner.IsPlayer`); use ScriptedGui for Execute only | [/gui/ui-override-file-layering.md](/gui/ui-override-file-layering.md) |
| KI-064 | Temporary building destroyed then still present after scripted “complete” | Monthly tick ran `construct_building` / spawn **after** complete in the same loop | Spawn only in the still-busy branch; add `remove_if` when project flag cleared | [/buildings/temporary-script-buildings.md](/buildings/temporary-script-buildings.md) |
| KI-065 | Completion event shows raw keys (`….3.ok` / untranslated) | Event loc not paired to script filename and/or nonstandard option suffix | Pair `*_events_l_english.yml` with `*_events.txt`; use `.a` for first option | [/localization/event-localization-naming.md](/localization/event-localization-naming.md) |
| KI-066 | Want Expand-RGO map pie ring but only see location-modifier chip / nothing on map | Used timed modifier and/or `construct_building` with **`instant = yes`** | Non-instant `construct_building` + `build_time`; ring tracks construction only | [/buildings/construction-map-markers.md](/buildings/construction-map-markers.md) |
| KI-067 | 25y / flag cooldown applied but missing from location timed modifiers | Empty location modifier (game_data only) filtered from `GetTimedModifiers` | Add id-keyed `modifier_icons` entry **and** a tiny stat; keep NAME/DESC loc | [/modifiers/timed-location-modifier-visibility.md](/modifiers/timed-location-modifier-visibility.md) |
| KI-068 | Convert/custom button clickable but does nothing on cooldown | `enabled` not bound to ScriptedGui.IsValid | `enabled = "[…IsValid(GuiScope…)]"`; keep `visible` on owner check only | [/gui/scripted-gui-isvalid-greying.md](/gui/scripted-gui-isvalid-greying.md) |
| KI-069 | Event option shows raw key / blank hover despite yml entry | **`Duplicate localization key`** — same option key defined in two yml files (e.g. main + `events/`); engine then fails to resolve it. Earlier mis-attributed to `….3.a` shape | Define the option key in **exactly one** yml (prefer paired events file). Check `error.log` for `Duplicate localization key` | [/localization/event-localization-naming.md](/localization/event-localization-naming.md) |
| KI-070 | Timed location chip shows NAME/DESC but no **Until \<date\>** | Lost the tiny real stat that made `TimedModifier.GetDuration` reliable (`local_monthly_development_modifier = -0.001`); migration-only / empty lock, or stale instance from earlier empty apply | Prefer proven tiny development line + icon; `remove_location_modifier` then `add` with `years` + `mode = add_and_extend`; re-complete after definition change | [/modifiers/timed-location-modifier-visibility.md](/modifiers/timed-location-modifier-visibility.md) |
| KI-071 | Advance `research_cost = 5` shows ~150 UI; `0.2` shows ~30 (not 5); `2.0` shows ~75 vs peers at ~25 | Cost is **relative**: UI ≈ age_base × (1 + research_cost) (not raw points). `2.0` is triple Traditions base — not “age-normal” | Match age peers with `research_cost = 0` (or omit); for ~5 UI use `-0.8` | [/advances/research-cost-scaling.md](/advances/research-cost-scaling.md) |
| KI-072 | Game crashes in menu; assume mod script | Vulkan `ErrorDeviceLost` + `FrontendInterfaceIdler`; often no script errors; can recur with mods disabled | Check crash `meta.yml` Idler + `error.log` for DeviceLost; try non-Vulkan / driver | [/validation/diagnosing-crashes-mods-vs-graphics.md](/validation/diagnosing-crashes-mods-vs-graphics.md) |
| KI-073 | Construction UI shows long timer but conversion completes on next monthly tick; or tools/PM never run for intended years | Monthly tick completed on `has_building` while `build_time` was set to full project length (or `has_building` becomes true early). Standing employment/PM needs a **finished** building | Short `build_time` (~1 day) + `months_left` countdown for standing duration; complete only when counter expires — not on first `has_building` | [/buildings/temporary-script-buildings.md](/buildings/temporary-script-buildings.md) Pattern C |

## Knowledge bundle (OKF)

| ID | Symptom | Root cause | Fix | Article |
|----|---------|------------|-----|---------|
| KI-040 | MCP `search_knowledge_catalog_docs` returns no OKF hits | OKF lives under `okf/` in repo; doc index may not include it | Use `search_knowledge_catalog_code` + fetch `okf/SPEC.md` URL | [/references/okf-format.md](/references/okf-format.md) |
| KI-041 | Bundle cited as "OKF v0.2" | Confused **format version** (`okf_version: "0.1"`) with **content revision** (`bundle_version`) | Only `okf_version` tracks OKF spec; use `bundle_version` for corpus revision | [/references/okf-format.md](/references/okf-format.md) |

## Economy & AI (markets, RGO)

| ID | Symptom | Root cause | Fix | Article |
|----|---------|------------|-----|---------|
| KI-076 | AI converts to sand/clay; log shows downgrades despite “pick highest price” | `ordered_goods` `order_by` used `multiply = -1` with `max = 1` → engine picks **lowest** score among negatives | Positive `price_in_market` only (vanilla mission pattern); ban `multiply = -1` on pick score | [/economy/rgo-conversion-ai-market-decision.md](/economy/rgo-conversion-ai-market-decision.md) |
| KI-077 | AI ignores 50% margin; picks fail `ui_to >= 1.5×ui_from` in telemetry | Unquoted `rgo_conv_ai_margin_delta >= 0` ignored in `ordered_goods` limit; `multiply = 1.5` on `subtract` may only subtract 1× current | `margin_delta = candidate − rgo_conv_ai_min_margin_price`; quoted `"rgo_conv_ai_margin_delta" >= 0` in limit **and** post-pick on `scope:rgo_conv_ai_pick_good` | [/economy/rgo-conversion-ai-market-decision.md](/economy/rgo-conversion-ai-market-decision.md) |

# Agent checklist (30 seconds)

Before claiming an EU5 mod fix is done:

1. Grep vanilla for the effect name you are using — [KI-001], [KI-032]
2. New yml? Confirm UTF-8 BOM — [KI-010]
3. New game rules? Paired `*_game_rules_l_*.yml` in **main_menu + in_game** with BOM — [KI-074]
4. New event? No `.0` id; paired loc file — [KI-002], [KI-011]
5. Firing events? `trigger_event_non_silently` / `_silently` only — [KI-001]
6. New advance? Check `starting_technology_level` — [KI-020]
7. Player-visible meter? Modifier + loc, not variable alone — [KI-021]
8. Full game restart after loc changes — [KI-030]
9. Iterating locations? Never `every_location` — [KI-061]
10. `construct_building` with multiplier? Include `cost_multiplier_reason` — [KI-062]
11. Script `.txt`? UTF-8 BOM (not just yml) — [KI-060]
12. Map progress ring? Non-instant construction, not timed modifier — [KI-066]
13. Timed location cooldown invisible? Icon + tiny stat — [KI-067]
14. Custom button inert but visible? Wire `enabled` to IsValid — [KI-068]
15. Event OK button raw / blank? Check `Duplicate localization key` in error.log — [KI-069]
16. Timed chip missing **Until \<date\>**? Restore tiny real stat + fresh timed apply — [KI-070]
17. Cheap test advance still costly? Formula is `25×(1+research_cost)` — use `-0.8` for ~5 — [KI-071]
18. Menu crash blamed on mod? Check Idler + `ErrorDeviceLost` first — [KI-072]
19. Long tools/employment project finishing on first month? Don’t complete on `has_building` alone — [KI-073]
20. `ordered_goods max=1` picking cheapest good? Check for `multiply = -1` on `order_by` — [KI-076]
21. Live margin gate never blocks? Use quoted `"script_value" >= 0` in limit + post-pick guard; `subtract = floor` not `multiply` on subtract block — [KI-077]

# See also

* [Common pitfalls](common-pitfalls.md) — symptom table (overlaps but less session-specific)
* [Smoke testing checklist](smoke-testing-checklist.md)
* [Error log debugging](error-log-debugging.md)

# Citations

[1] Northern Crusade mod (`mod/northern_crusade_teu/`) — source of KI-001 through KI-022
[2] OKF alignment pass — KI-040, KI-041
[3] RGO Conversion (renamed Sire… merchandise mod) — KI-060 through KI-073
[4] [Common pitfalls](common-pitfalls.md) — parallel quick-reference table
