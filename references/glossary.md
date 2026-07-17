---
type: Reference
title: Glossary
description: EU5 modding terms used throughout this bundle — DHE, on_action, advance, scope, and more.
tags: [references, glossary, terminology]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
---

Short definitions for terms across this OKF bundle. Cross-links point to detailed articles where they exist.

# A–C

**Advance** — A research node in the technology/advances tree. Defined in `in_game/common/advances/`. Grants modifiers or unlocks laws, buildings, units. See [Advance file structure](/advances/advance-file-structure.md).

**Age** — Time period bucket for advances (`age_1_traditions`, `age_2_renaissance`, …). Controls which tab shows an advance in the research UI.

**BOM (UTF-8 with BOM)** — Byte order mark required on EU5 localization `.yml` files. Without it, loc keys display raw in-game. See [UTF-8 BOM requirement](/localization/utf8-bom-requirement.md).

**CB / Casus belli** — War justification type. Some advances unlock CBs via `unlock_casus_belli`.

**CMF / CMM** — Community Mod Framework / Community Mod Menu: shared multi-mod settings UI and hooks. See [community-mod-framework/](/community-mod-framework/).

**Content priority** — Numeric sort weight on advances; higher values list earlier within an age.

**Country scope** — Script context where `ROOT` is a country. Default for country events, most on_actions, and advance `potential` blocks.

**Customizable localization** — Dynamic loc resolved at runtime via `ROOT.Custom('key')` and `customizable_localization/` defs. Used for tier names and variable-driven text.

# D–G

**DHE (Dynamic Historical Event)** — Flavor event flagged for the historical events browser. Requires `dynamic_historical_event = yes` and usually `historical_info`. See [Dynamic historical events](/events/dynamic-historical-events.md).

**Descriptor / metadata** — EU5 mods register via `.metadata/metadata.json` in the mod root (not legacy `.mod` alone). See [Mod metadata and descriptor](/getting-started/mod-metadata-and-descriptor.md).

**Effect** — Script block that changes game state (`add_country_modifier`, `set_variable`, `change_government_type`, …). Used in event options, on_actions, mission hooks.

**Formable** — Country formation definition in `in_game/common/formable_countries/`. See [Formable countries overview](/formables/formable-countries-overview.md).

**game_data** — Block on static modifier **type definitions** and custom static modifiers specifying `category` (country, location, …). See [game_data category](/modifiers/game-data-category.md).

**Government reform** — Selectable policy slot modifier package. Distinct from government **type**. See [Government reforms](/governments/government-reforms.md).

**Government type** — Base government category (monarchy, theocracy, …) with succession and power meter. See [Government types](/governments/government-types.md).

# I–M

**in_game / main_menu / loading_screen** — Top-level mod folders mirroring the vanilla `game/` install. Session script vs menu/setup/loc vs boot defines/branding. See [Mod folder structure](/getting-started/mod-folder-structure.md) and [Three-root architecture](/total-conversion/three-root-and-override-ladder.md).

**INJECT:** — Operator that merges fields into an existing vanilla definition key without replacing the whole block. Prefer for surgical patches (e.g. attaching production methods). See [Override ladder](/total-conversion/three-root-and-override-ladder.md).

**Institution** — Global tech-social mechanic referenced in advance `allow` blocks (`has_embraced_institution = institution:feudalism`).

**Jomini** — Paradox script engine used by EU5. Errors appear as `jomini_script_system.cpp` lines in `error.log`.

**Load order** — Launcher playset order; lower position wins on same-path file overrides. Among same filename across mods, alphabetical filename tie-break applies per wiki.

**Loc key** — String id in yml mapped to display text. Event titles, advance names, modifier names each follow naming conventions.

**Mission** — Selectable task chain under `in_game/common/missions/`. See [Mission trees overview](/missions/mission-trees-overview.md).

**Modifier stat key** — Identifier for a gameplay stat (`discipline`, `trade_income`, …) defined in `modifier_type_definitions`. See [Modifier stat keys](/modifiers/modifier-stat-keys.md).

# O–S

**on_action** — Hook fired on game events (`on_game_start`, monthly pulses, …). Defined in `in_game/common/on_action/`. See [On game start](/on-actions/on-game-start.md).

**OKF** — Open Knowledge Format (v0.1): markdown + YAML frontmatter knowledge bundles. This library is an OKF bundle. See [OKF format](/references/okf-format.md).

**potential** — Trigger block controlling **visibility** (advances, reforms, missions). Contrast with `allow` (can interact now).

**allow** — Trigger block controlling **eligibility** (can research advance, start mission, pick reform).

**Proximity / control** — Distinct levers: spread costs vs `local_max_control` ceiling. 100% proximity need not imply full control. See [Proximity vs control](/map/proximity-vs-control.md).

**REPLACE:** — Operator that replaces an existing named definition block in place. Heavier than `INJECT:`. See [Override ladder](/total-conversion/three-root-and-override-ladder.md).

**Scope** — Script evaluation context (country, province, character). Wrong scope causes silent no-ops.

**Scripted effect** — Named reusable effect block in `in_game/common/scripted_effects/`. See [Scripted effect basics](/scripted-effects/scripted-effect-basics.md).

**Scripted trigger** — Named reusable trigger block in `in_game/common/scripted_triggers/`.

**Static modifier** — Persistent stat package applied via `add_country_modifier` etc. Defined in `main_menu/common/static_modifiers/`. See [Static modifiers](/modifiers/static-modifiers.md).

**starting_technology_level** — Advance field gating day-1 researched state in Age of Traditions. See [Starting technology level](/advances/starting-technology-level.md).

# T–Z

**Tag** — Three-letter country id (`TEU`, `POL`, `HUN`). Used in `has_or_had_tag`, formables, missions.

**Trigger** — Conditional block returning true/false. Same syntax family as effects but read-only.

**trigger_event_non_silently** — Effect that fires an event popup; preferred over bare `trigger_event` for player-visible events. See [Triggering events](/events/triggering-events.md).

**Variable** — Country/province/character stored value via `set_variable` / `has_variable`. Often paired with static modifiers for UI. See [Variables and monthly mechanics](/scripted-effects/variables-and-monthly-mechanics.md).

**Vanilla** — Base game install under Steam `Europa Universalis V/game/`. Reference for working syntax.

**Workshop mod path** — Steam Workshop content under `steamapps/workshop/content/<appid>/<publishedfileid>/`. MnT EU5: `3450310/3735059838`. See [MEIOU and Taxes reference](/total-conversion/meiou-and-taxes-reference.md).

# See also

* [Paradox wiki and tools](paradox-wiki-and-tools.md)
* [Vanilla file locations](vanilla-file-locations.md)

# Citations

[1] This bundle — cross-linked concept articles under `eu5-modding-knowledge/`
[2] Vanilla: `game/` install tree — paths referenced in [Vanilla file locations](vanilla-file-locations.md)
[3] Mod: `northern_crusade_teu/` — example flavor mod cited throughout the bundle
[4] Workshop: MEIOU and Taxes EU5 — total-conversion patterns in [total-conversion/](/total-conversion/)
