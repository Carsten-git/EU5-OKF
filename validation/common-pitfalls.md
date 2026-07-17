---
type: Reference
title: Common pitfalls
description: EU5 modding mistakes that often fail silently or show raw loc keys.
tags: [validation, pitfall, checklist]
timestamp: 2026-07-06T08:45:00+10:00
status: complete
---

# Quick reference

| Symptom | Likely cause | Article |
|---------|----------------|---------|
| Raw `event.id.title` in popup | Missing BOM, wrong yml filename, or `.0` event id | [BOM](/localization/utf8-bom-requirement.md), [event IDs](/events/event-id-rules.md) |
| Event never fires | Invalid `trigger_event =` | [Triggering events](/events/triggering-events.md) |
| Intro works on new game only | `on_game_start` not save-safe | [Country pulses](/on-actions/country-pulses.md) |
| DHE entry missing | No `dynamic_historical_event` or bad `historical_info` | [DHE visibility](/events/dhe-browser-visibility.md) |
| Advance already researched | `starting_technology_level` too low | [Starting tech level](/advances/starting-technology-level.md) |
| Advance invisible for tag | Missing or wrong `potential` block | [Country-specific advances](/advances/country-specific-advances.md) |
| Custom meter invisible | Variables alone have no UI | [Displaying mechanics](/modifiers/displaying-hidden-mechanics.md) |
| Modifier has no effect | Unknown stat key on modifier | [Modifier stat keys](/modifiers/modifier-stat-keys.md) |
| Wrong scope on modifier | Mismatched `game_data` category | [game_data category](/modifiers/game-data-category.md) |
| Loc works in menu, not in game | File under wrong tree or language | [Mod structure](/getting-started/mod-folder-structure.md) |
| Mod missing from launcher | No `.metadata/metadata.json` | [Mod metadata](/getting-started/mod-metadata-and-descriptor.md) |
| Script error on load | Bad effect/trigger syntax | [Error log debugging](/validation/error-log-debugging.md) |
| `Unknown effect every_location` | No such iterator | [Location iteration](/scripted-effects/location-iteration-effects.md) |
| `construct_building` PostValidate false | Missing `cost_multiplier_reason` | [Hidden effects](/events/hidden-effects-and-ai-chance.md) |
| Custom location button invisible | `visible` bound to ScriptedGui.IsShown | [UI override layering](/gui/ui-override-file-layering.md) |
| Button visible/clickable but does nothing | `enabled` not tied to ScriptedGui.IsValid | [IsValid greying](/gui/scripted-gui-isvalid-greying.md) |
| No map pie ring for scripted project | `instant = yes` or only timed modifiers | [Construction map markers](/buildings/construction-map-markers.md) |
| Project completes on first monthly tick despite long build timer | Completed on `has_building` while needing standing duration / tools PM | Short build + month counter — [KI-073](/validation/known-issues.md), [Temporary buildings](/buildings/temporary-script-buildings.md) Pattern C |
| Timed location cooldown invisible | Empty modifier (no icon/stat) | [Timed location modifier visibility](/modifiers/timed-location-modifier-visibility.md) |
| Lexer wants utf8-bom on `.txt` | Script saved without BOM | [UTF-8 BOM](/localization/utf8-bom-requirement.md) |
| “Modifies the checksum” warning | Gameplay files changed — expected | [Checksum](/getting-started/checksum-and-gameplay-mods.md) |
| Event option shows raw / blank hover | Duplicate option key across two yml files (`Duplicate localization key` in error.log) | [Event localization naming](/localization/event-localization-naming.md) |
| Timed chip has flavor text but no Until date | Tiny real stat removed / stale empty instance | [Timed location modifier visibility](/modifiers/timed-location-modifier-visibility.md) |
| Advance cost `5`→~150 or `0.2`→~30 | UI ≈ `25×(1+research_cost)`; use `-0.8` for ~5 | [Research cost scaling](/advances/research-cost-scaling.md) |
| Convert-to metal/horses but cannot extract | Missing `can_extract_*` on advance | [can_extract gates](/advances/can-extract-goods-gates.md) |
| Mod advance locked behind institution forever | `requires` institution age root | [Age roots](/advances/age-roots-and-institution-gates.md) |
| Crash in main menu blamed on gameplay mod | Vulkan `ErrorDeviceLost` / frontend Idler | [Crashes mods vs graphics](/validation/diagnosing-crashes-mods-vs-graphics.md) |
| Typos in advance bonuses | Unknown key — no log line | [Mod validation tooling](/validation/mod-validation-tooling.md) |

# Process pitfalls

- Testing with save game after changing `on_game_start` logic
- Not full-restarting after yml changes
- Bundling all event loc into one file
- Skipping `python tools/validate_mod.py` before launch
- Grepping only the mod folder — always confirm effects in vanilla first
- Using EU4 wiki syntax without checking EU5 vanilla files

# See also

* [Known issues registry](/validation/known-issues.md) — session-specific bugs with KI-### IDs
* [Smoke testing checklist](smoke-testing-checklist.md)
* [Reading vanilla examples](/getting-started/reading-vanilla-examples.md)

# Citations

[1] Mod: `northern_crusade_teu/tools/validate_mod.py` — advance and modifier key validation
[2] Vanilla: `game/main_menu/common/modifier_type_definitions/00_modifier_types.txt` — authoritative modifier stat keys
[3] Bundle articles cited in the quick-reference table above
