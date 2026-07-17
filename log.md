# Bundle Update Log

## 2026-07-18 — REQ-007 codegen + Sire doc path fixes

* [REQ-007 presence codegen](tooling/req007-presence-codegen.md) — CSV pool/matrix → scripted triggers
* Fixed legacy `mod/rgo_conversion/docs/` pointers → `mod/Sire, Who Bound This Manor to a Single Merchandise/README.md`

## 2026-07-17 — RGO balance telemetry pipeline (Sire observer analytics)

* [Market price history from save](validation/market-price-history-from-save.md) — Pdx-Unlimiter melt, `market_manager` CSV extract
* [RGO balance telemetry pipeline](validation/rgo-balance-telemetry-pipeline.md) — picks + save prices, cobweb interpretation
* [Agent topic router](references/agent-topic-router.md) — balance telemetry rows
* Bundle **0.28**

## 2026-07-16 — AI pulse cadence tune (Sire REQ-009)

* [RGO conversion AI market decision](economy/rgo-conversion-ai-market-decision.md) — generic example `chance = 1`; Sire ships 1%/month
* [Mod performance pulses and scans](on-actions/mod-performance-pulses-and-scans.md) — already documents Sire 1% path
* Bundle **0.28**

## 2026-07-16 — REQ-009 telemetry validation benchmark

* [Script telemetry via hidden events](validation/script-telemetry-via-hidden-events.md) — post-fix session metrics (135 picks, 100% margin)
* Bundle **0.27**

## 2026-07-16 — REQ-009 margin gate binding (KI-077 update)

* [RGO conversion AI market decision](economy/rgo-conversion-ai-market-decision.md) — quoted `"rgo_conv_ai_margin_delta" >= 0`, post-pick guard, `subtract = floor`
* [KI-077](validation/known-issues.md) — unquoted compare + multiply-on-subtract pitfall
* Bundle **0.26**

## 2026-07-16 — REQ-009 AI margin + pick fixes (KI-076, KI-077)

* [RGO conversion AI market decision](economy/rgo-conversion-ai-market-decision.md) — Columbian 50% margin delta, positive `ordered_goods` pick, anti-patterns
* [KI-076](validation/known-issues.md), [KI-077](validation/known-issues.md) — inverted `order_by`, broken `price_in_market { value >= script_value }`
* [Script telemetry via hidden events](validation/script-telemetry-via-hidden-events.md) — dot-notation `raw_material` for `p_from` stash
* Bundle **0.25**

## 2026-07-16 — Script logging and telemetry reference

* [Script logging and telemetry](validation/script-logging-and-telemetry.md) — binding matrix, vanilla `error_log` survey, `price_in_market` vs `GetMarket.GetPrice`, ship discipline
* Rewrote [Script telemetry via hidden events](validation/script-telemetry-via-hidden-events.md) — owner `save_scope_as` + dual `p_*` / `ui_*` recipe (REQ-009)
* Updated [Error log debugging](validation/error-log-debugging.md) — binding failure grep patterns
* [KI-075](validation/known-issues.md) — telemetry binding mistakes
* Bundle **0.24**

## 2026-07-15 — Script telemetry via hidden events

* [Script telemetry via hidden events](validation/script-telemetry-via-hidden-events.md) — `error_log` scope nullptr in scripted_effects; hidden `location_event` pattern (Sire REQ-009)
* Updated [Error log debugging](validation/error-log-debugging.md) — scoped telemetry cross-link
* Bundle **0.23**

## 2026-07-15 — RGO UI profit metrics; de-productize conversion KB

* [RGO UI profit metrics](economy/rgo-ui-profit-metrics.md) — wealth / tax base / income ladder; GUI `data_types` vs script triggers
* [RGO conversion AI market decision](economy/rgo-conversion-ai-market-decision.md) — generic patterns only; thresholds live in product mods
* Updated [RGO baseline output](economy/rgo-baseline-output-and-price-balance.md), [price_in_market](economy/price-in-market-script-api.md)
* Bundle **0.22**

## 2026-07-15 — RGO flat-tile output; price-ratio conversion AI

* [RGO UI profit metrics](economy/rgo-ui-profit-metrics.md) — initial draft (superseded by 0.22 de-productize pass)
* Rewrote [RGO conversion AI market decision](economy/rgo-conversion-ai-market-decision.md) — `P_tgt/P_cur − 1` concept
* Updated [RGO baseline output](economy/rgo-baseline-output-and-price-balance.md) — playtest flat O_tile; defines vs live tiles
* Updated [price_in_market in script](economy/price-in-market-script-api.md) — same-tile comparison pattern
* Bundle **0.21**

## 2026-07-15 — Vanilla master data index

* [Vanilla master data index](references/vanilla-master-data-index.md) — goods, locations, markets, RGO costs, defines, runtime APIs
* Updated [RGO baseline output](economy/rgo-baseline-output-and-price-balance.md) — inference chain, expand cost scaling
* Bundle **0.20**

## 2026-07-15 — RGO baseline output and price balance

* [RGO baseline output and price balance](economy/rgo-baseline-output-and-price-balance.md) — all 52 vanilla raw materials; defines hypothesis; price index rationale
* [Goods food vs market price](economy/goods-food-vs-market-price.md) — food ≠ output; markets price named goods
* Updated [RGO conversion AI market decision](economy/rgo-conversion-ai-market-decision.md) — generic playbook; price index; no product refs
* Updated [price_in_market in script](economy/price-in-market-script-api.md) — food_price vs commodity price
* Bundle **0.19**

## 2026-07-15 — Live market price + RGO AI decision (REQ-009 research)

* [price_in_market in script](economy/price-in-market-script-api.md) — triggers, script_values, vanilla call sites, GUI profit APIs
* [RGO conversion AI market decision](economy/rgo-conversion-ai-market-decision.md) — monthly pulse, price vs revenue margin, Columbian `ai_will_do` breakdown, Sire thresholds
* Expanded [Columbian Exchange RGO pattern](economy/columbian-exchange-rgo-pattern.md) — `ai_will_do` table (not a % threshold)
* [Agent topic router](references/agent-topic-router.md) — rows for `price_in_market` and AI market decision
* Bundle **0.18**

## 2026-07-14 — Game rules loc (KI-074)

* [KI-074](validation/known-issues.md) — Game Rules UI shows raw `setting_*` when yml lacks BOM and/or paired `*_game_rules_l_*.yml`
* Expanded [Custom game rules](game-rules/custom-game-rules.md) — main_menu + in_game loc pairing, BOM, relaunch note

## 2026-07-13 — Agent discoverability (topic router)

* [Agent topic router](references/agent-topic-router.md) — capability → article for settings / Glorp / RGO / mapmodes / CMF
* Refreshed [Building a mod with this KB](getting-started/building-a-mod-with-this-kb.md) — game rules vs CMM; mapmodes; CM/Glorp shapes
* Bundle **0.17** (product SO `reference.md` + `engineering-kb.md` updated in Sire product OKF)

## 2026-07-13 — Zorange's Mapmode Collection (workshop `3697317887`)

* [Zorange's Mapmode Collection as reference](references/zorange-mapmode-collection-as-reference.md) — ZMC-1–3
* [Live vs cached mapmode metrics](gui/live-vs-cached-mapmode-metrics.md) — script_value live vs location-var cache
* [Mapmode localization and game concepts](gui/mapmode-localization-and-concepts.md) — loc, legends, textformatting, game_concepts
* Expanded [custom map modes](gui/custom-map-modes.md) — additive utility checklist from ZMC
* Bundle **0.16**

## 2026-07-13 — Construction Manager (workshop `3736668860`)

* [Construction Manager as reference](references/construction-manager-as-reference.md) — pattern map CM-1–7
* [Off-screen scripted widget drivers](gui/offscreen-scripted-widget-drivers.md) — construct queue / classification (CM-1)
* [Search filters for lists](gui/search-filters-for-lists.md) — `gui/filters/` (CM-2)
* [Context-specific widget aliases](gui/context-specific-widget-aliases.md) — `_pl` / `_bv` / location wrappers (CM-3)
* [Mass opt-in / opt-out location flags](gui/mass-opt-in-opt-out-location-flags.md) — mass + exclusion vars (CM-4)
* [CMM priority feature dispatcher](community-mod-framework/cmm-priority-feature-dispatcher.md) — ordered CMM features (CM-5)
* [Dummy rank-upgrade buildings](buildings/dummy-rank-upgrade-buildings.md) — urbanize via dummy build (CM-6)
* Extended [custom map modes](gui/custom-map-modes.md) with CM breadbasket / food potential (CM-7)
* Bundle **0.15**

## 2026-07-13 — Player Speed Game Rules (workshop `3755676844`)

* New section [game-rules/](game-rules/) — additive vanilla Game Rules for utility mods
* [Custom game rules](game-rules/custom-game-rules.md) — schema, flags, loc, `has_game_rule`
* [Player-scoped modifiers from game rules](game-rules/player-scoped-modifiers-from-rules.md) — `apply_modifier = player:…`
* [Game rules vs CMM](game-rules/game-rules-vs-cmm.md) — campaign-start house rules vs mid-game toggles
* Corrected [Player options without lobby](community-mod-framework/player-options-without-lobby.md) — game rules are **not** TC-only
* Updated [knowledge coverage](references/knowledge-coverage.md), modifiers index
* Bundle **0.14**

## 2026-07-12 — Glorp patterns #27–31 (CMM catalog, player options, bulk RGO)

* [CMM settings catalog design](community-mod-framework/cmm-settings-catalog-design.md) — #27: tabs/groups/defaults/inverted bools (Glorp worked example)
* [CMM dropdown multiselector](community-mod-framework/cmm-dropdown-multiselector.md) — #28: `cmm_set_dropdown_multiselector`
* [CMM pause menu entry](community-mod-framework/cmm-pause-menu-entry.md) — #29: host vs MP client `CMM_*` scripted GUIs
* [Expand raw goods lateral view](gui/expand-raw-goods-lateralview.md) — #30: bulk RGO panel + row widget injection
* [Player options without lobby events](community-mod-framework/player-options-without-lobby.md) — #31: no new-game wizard; CMM defaults + init hooks
* Updated [Glorp UI reference](references/glorp-ui-as-reference.md), CMF/gui indexes, [community-mod-menu](community-mod-framework/community-mod-menu.md), [cmm-lists-and-advanced](community-mod-framework/cmm-lists-and-advanced.md), [dual-settings-ui-fallback](community-mod-framework/dual-settings-ui-fallback.md), [knowledge coverage](references/knowledge-coverage.md)
* Bundle **0.13**

## 2026-07-12 — Glorp UI local extract (v1.3.10.1)

* [Glorp UI file inventory](references/glorp-ui-file-inventory.md) — subscribed workshop layout (`3601047146`)
* [Glorp location RGO row](gui/glorp-location-window-rgo-row.md) — pattern **#25**: `header_button_left` + CM peer widget slots (not vanilla compact row)
* [Integrating with Glorp UI](gui/integrating-with-glorp-ui.md) — pattern **#26**: merge submod vs CMF fallback vs load-order truth
* Updated [Glorp UI reference](references/glorp-ui-as-reference.md), [UI override layering](gui/ui-override-file-layering.md), [knowledge coverage](references/knowledge-coverage.md)
* Bundle **0.12**

## 2026-07-12 — Advance UX: free roots + extract tooltips

* [Age roots](advances/age-roots-and-institution-gates.md) — prefer `depth = 0` free roots for convert unlocks (not buried under Sanitation/Surgery)
* [Advance-gated RGO unlocks](advances/advance-gated-rgo-unlocks.md) — every unlock good needs `can_extract_*` so tooltip is not blank

## 2026-07-12 — Short build + standing worksite (KI-073)

* [Construction map markers](buildings/construction-map-markers.md) — Pattern A (ring = duration) vs B (short raise + month counter)
* [Temporary script-spawned buildings](buildings/temporary-script-buildings.md) — Pattern C for tools/employment after finish
* KI-073: do not complete solely on `has_building` when duration is a standing worksite
* Bundle **0.11**

## 2026-07-12 — Research cost, extract gates, crash diagnosis

* [Advance research cost scaling](advances/research-cost-scaling.md) — UI ≈ `25×(1+research_cost)`; KI-071 corrected (`0.2`→30, `-0.8`→~5)
* [can_extract goods gates](advances/can-extract-goods-gates.md) — metals/horses vs auto_modifiers
* [Age roots and institution gates](advances/age-roots-and-institution-gates.md) — free public-health `requires`
* [Diagnosing crashes — mods vs graphics](validation/diagnosing-crashes-mods-vs-graphics.md) — Vulkan `ErrorDeviceLost` + FrontendIdler; KI-072
* Expanded advance-gated RGO playbook; pitfalls + error-log crash section; bundle **0.10**

## 2026-07-12 — Advance-gated RGO unlocks

* [Advance-gated RGO conversion unlocks](advances/advance-gated-rgo-unlocks.md) — ship `research_cost = 0` to match age peers; cheap test only via `-0.8`
* RGO Conversion mod implements 25 continent advances per `docs/advances-unlocks.md`

## 2026-07-11 — KI-069 duplicate loc + KI-070 Until date

* Root cause of blank completion option: **`Duplicate localization key`** for `rgo_conversion_complete_option` in main + events yml (not `….3.a` shape)
* Restore **Until \<date\>** on convert lock: `local_monthly_development_modifier = -0.001` (proven); clear+`add_and_extend` on complete
* Updated [event localization naming](localization/event-localization-naming.md), [timed location modifier visibility](modifiers/timed-location-modifier-visibility.md), KI-069/KI-070, common-pitfalls

## 2026-07-11 — Performance analysis (RGO Conversion)

* [Mod performance — pulses and location scans](on-actions/mod-performance-pulses-and-scans.md) — monthly AI `random_owned_location` + allow matrix is the only notable cost; player path negligible
* Cross-links from [country pulses](on-actions/country-pulses.md)

## 2026-07-11 — Immersion loc pass + KI-069 (….3.a unrecognized)

* Flat completion option key `rgo_conversion_complete_option` (avoid `….3.a`)
* Player-facing loc: no meta “separate modifier” copy; immersive names/DESC
* Lock chip: mild migration attraction so timed **Until** tooltip stays reliable
* KI-069 + event-localization-naming / timed-modifier-visibility notes

## 2026-07-11 — Clarify CE RGO worker-cap reset math

* [Change raw material](economy/change-raw-material.md) — worked example: delta `−(rgo_workers−1)` → level 1 (not “wipe RGO”)
* [Columbian Exchange RGO pattern](economy/columbian-exchange-rgo-pattern.md) — cross-link to the math section

## 2026-07-11 — RGO conversion costs: reset levels + settling hangover

* On complete: CE-style `change_max_raw_material_workers` → level 1, then flip good
* New timed modifier `rgo_conv_settling`: `local_raw_material_output = -0.10` for 10 years
* Cooldown remains 25y convert-lock (cosmetic −0.001 dev penalty removed)
* Design doc + [change raw material](economy/change-raw-material.md) updated

## 2026-07-11 — Construction markers + timed mod visibility + IsValid (KI-066–068)

* [Construction map markers](buildings/construction-map-markers.md) — Expand-RGO pie ring via non-instant construction
* [Timed location modifier visibility](modifiers/timed-location-modifier-visibility.md) — empty cooldowns invisible in `GetTimedModifiers`
* [ScriptedGui IsValid greying](gui/scripted-gui-isvalid-greying.md) — visible but disabled buttons
* Rewrote [temporary script buildings](buildings/temporary-script-buildings.md) — instant vs map-marker modes
* Known issues KI-066–068; common-pitfalls; coverage → bundle **0.9**
* Standing rule: fold session discoveries into OKF before closing work ([building a mod](getting-started/building-a-mod-with-this-kb.md))

## 2026-07-11 — Checksum + temp building + event loc (KI-064–065)

* [Checksum and gameplay mods](getting-started/checksum-and-gameplay-mods.md) — gameplay mods cannot avoid checksum warning
* [Temporary script-spawned buildings](buildings/temporary-script-buildings.md) — don’t respawn after complete; use `remove_if`
* Event loc: pair filename + prefer `.a` options ([event-localization-naming](localization/event-localization-naming.md))
* Known issues KI-064–065

## 2026-07-11 — RGO Conversion crash / button learnings (KI-060–063)

* [Location iteration effects](scripted-effects/location-iteration-effects.md) — no `every_location`
* [UTF-8 BOM](localization/utf8-bom-requirement.md) — extended to script `.txt` (not only yml)
* `construct_building` examples require `cost_multiplier_reason` ([hidden effects](events/hidden-effects-and-ai-chance.md))
* GUI: don’t hide buttons with ScriptedGui.IsShown ([UI layering](gui/ui-override-file-layering.md))
* Known issues KI-060–063; common-pitfalls rows updated

## 2026-07-11 — RGO engineering vs product design split

* Kept engineering: [Change raw material](economy/change-raw-material.md), [Columbian Exchange RGO pattern](economy/columbian-exchange-rgo-pattern.md)
* Moved product design out of KB → mod stub `mod/rgo_conversion/docs/` (DESIGN + terrain goods v1)
* Removed `economy/player-rgo-conversion-design.md` from this bundle

## 2026-07-11 — RGO change + conversion design (bundle v0.8)

* [Change raw material](economy/change-raw-material.md) — `change_raw_material`, worker-cap companion, empty `on_raw_material_changed`
* [Columbian Exchange RGO pattern](economy/columbian-exchange-rgo-pattern.md) — chained selects, terrain gates, instant flip
* *(Product design later moved to `rgo_conversion` mod — see entry above)*
* Bumped `bundle_version` to **0.8**

## 2026-07-11 — Glorp UI patterns #1–24 (bundle v0.7)

* [Glorp UI as reference](references/glorp-ui-as-reference.md) — full #1–24 index
* High: [CMM aliases](community-mod-framework/cmm-aliases-for-gui.md), [cross-mod GUI](community-mod-framework/cross-mod-gui-integration.md), [HUD value sync](gui/hud-value-sync.md), [CMF worked example](community-mod-framework/glorp-ui-cmf-worked-example.md), [cmfg extraction](gui/cmfg-vanilla-type-extraction.md), [dual settings fallback](community-mod-framework/dual-settings-ui-fallback.md)
* Medium: [UI layering](gui/ui-override-file-layering.md), [trait filter codegen](tooling/trait-filter-codegen.md), [script values/ROI](gui/script-values-and-roi-in-gui.md), [mass-action counter](gui/mass-action-ui-counter.md), [mapmode hover](gui/mapmode-hover-preview.md), [multi-file loc](localization/multi-file-loc-split-for-ui-mods.md); map-modes article extended (#16)
* Low: [UI overhaul niche patterns](gui/ui-overhaul-niche-patterns.md) (#19/#21/#23); #20/#22 in layering; #24 in cross-mod
* Bumped `bundle_version` to **0.7**

## 2026-07-11 — Community Mod Framework ingest (bundle v0.6)


* New section [community-mod-framework/](community-mod-framework/) from Co-op MCP + `docs/wiki/cmf.wiki` / `cmm.wiki`:
  * Overview/dependency, registration & on-action hooks, CMM settings, lists/advanced, action bar & alerts, banners & action log, utilities (`is_host`, mod detection, `cmf_suppress`), GUI macros & widget overrides, toolkit/visual editor, dependency-check popup
* Cross-links: metadata dependencies, paradox-wiki-and-tools, never-trigger-me → `cmf_suppress`, Cursor skills updated
* Bumped `bundle_version` to **0.6**

## 2026-07-11 — MnT extract completion pass (bundle v0.5)


* Completed remaining MnT how-tos:
  * [Naval transport levy chain](military/naval-transport-levy-chain.md)
  * [Societal values and estate power](governments/societal-values-estate-power.md)
  * [Building cap script values](buildings/building-cap-script-values.md)
  * [Goods demand construction baskets](economy/goods-demand-construction-baskets.md)
  * [Character interaction pre-evaluation](interactions/character-interaction-pre-evaluation.md)
  * [Custom modifier type registration](modifiers/custom-modifier-type-registration.md)
  * [Data-binding macros](tooling/data-binding-macros.md)
* Captured “low value but valid” know-how:
  * [Soft-disable vanilla systems](total-conversion/soft-disable-vanilla-systems.md)
  * [Numeric rebalance playbook](total-conversion/numeric-rebalance-playbook.md)
  * [Loading screen branding](total-conversion/loading-screen-branding.md)
* New sections: [military/](military/), [interactions/](interactions/).
* Rewrote [knowledge coverage](references/knowledge-coverage.md) — MnT reusable extract marked complete; remaining Weak areas are outside MnT’s scope.
* Bumped `bundle_version` to **0.5**.

## 2026-07-11 — Skills + coverage pass (bundle v0.4)


* Cursor personal skills (all agents): `~/.cursor/skills/eu5-okf-modding`, `~/.cursor/skills/eu5-mod-knowledge-extract`.
* [Building a mod with this KB](getting-started/building-a-mod-with-this-kb.md) — agent entry checklist.
* [Knowledge coverage](references/knowledge-coverage.md) — fitness for building mods; MnT gaps remaining.
* Filled high-value MnT gaps: [pulse orchestration](on-actions/pulse-orchestration.md), [never-trigger-me](gui/never-trigger-me-workaround.md), [loading-screen defines](total-conversion/loading-screen-defines.md), [startup world surgery](total-conversion/startup-world-surgery.md), [custom map modes](gui/custom-map-modes.md), [environmental disease](map/environmental-disease.md), [scripted geography](map/scripted-geography.md), [situation enrollment](total-conversion/situation-dynamic-enrollment.md).
* Bumped `bundle_version` to **0.4**.

## 2026-07-11 — MEIOU and Taxes total-conversion extract (bundle v0.3)


* Bumped `bundle_version` to **0.3** on root [index](index.md).
* New sections from workshop MnT EU5 (`3450310/3735059838`, v0.1.6):
  * [total-conversion/](total-conversion/) — [MnT reference](total-conversion/meiou-and-taxes-reference.md), [three-root + override ladder](total-conversion/three-root-and-override-ladder.md), [naming prefixes](total-conversion/naming-and-subsystem-prefixes.md)
  * [buildings/](buildings/) — [RGO substitution](buildings/rgo-to-building-substitution.md), [INJECT PMs](buildings/inject-production-methods.md)
  * [economy/](economy/) — [EPBM estate upkeep](economy/estate-paid-building-maintenance.md), [hidden event pipeline](economy/hidden-event-economy-pipeline.md)
  * [map/](map/) — [Köppen climates](map/koppen-climates.md), [proximity vs control](map/proximity-vs-control.md), [centers of importance](map/dynamic-centers-of-importance.md), [tribal levies](map/tribal-levies-and-demographics.md), [CSV location templates](map/csv-location-templates-pipeline.md)
  * [gui/](gui/) — [custom UI patterns](gui/custom-ui-patterns.md)
  * [tooling/](tooling/) — [TC toolchain](tooling/total-conversion-toolchain.md)
* Updated [mod folder structure](getting-started/mod-folder-structure.md) for `loading_screen/` and three-root guidance.
* Cross-links from [references](references/index.md) and [getting-started](getting-started/index.md).

## 2026-07-06 — TEU playtest round 4 (readout swap, formable ordering, Tannenberg clarity)

* [Known issues](validation/known-issues.md): KI-012 revised — `GetPlayer` also fails in modifier tooltips; added KI-015. Fix: 101-variant readout-swap pattern (`tools/generate_purpose_readout.py`).
* [Displaying hidden mechanics](modifiers/displaying-hidden-mechanics.md): rewritten around readout-swap (replaces meter + tier display modifiers).
* [Dynamic text in loc](localization/dynamic-text-in-loc.md): documented readout-swap as the working pattern for scope-less modifier UI.
* Baltic Dominion circular precondition: split `teu_nc_can_form_bdm` into per-requirement tooltips (removed `teu_nc_can_form_bdm_tt` action phrasing).
* Formable `content_priority`: vanilla PRU/GER at 800, then ODR 799 → Catholic 798/797 → Baltic 796/795 → Commerce 794/793.
* Formable button names shortened; path/step info moved to `_f_desc` only.

* [Known issues](validation/known-issues.md): rewrote KI-012 (use `GetPlayer.MakeScope.GetVariable` in static modifier DESC — `ROOT` has no scope there), updated KI-013; added KI-007 (hidden `random_list` outcomes need UX text), KI-008 (notification events look like "no effect"), KI-014 (no dummy stats — stat-less modifiers display fine), KI-052 (outer `custom_tooltip` hides inner requirement tooltips / circular "form X" precondition), KI-053 (broad `potential` needs tag gates in `allow`), KI-054 (encode formable paths via `content_priority` grouping + path labels in names).
* [Dynamic text in loc](localization/dynamic-text-in-loc.md): rewritten — documented the scope-less GUI context of modifier tooltips and the vanilla-confirmed `GetPlayer` pattern.
* [Displaying hidden mechanics](modifiers/displaying-hidden-mechanics.md): updated meter pattern; added "No dummy stats" section.
* [Formable triggers and effects](formables/formable-triggers-and-effects.md): replaced the outer-wrapper `custom_tooltip` example with the direct-call pattern; added KI-052/KI-053 pitfalls.
* [Formable countries overview](formables/formable-countries-overview.md): added "Presenting branching paths in the list" section.

## 2026-07-06 — OKF alignment pass

* Corrected [OKF format](references/okf-format.md): format is **OKF v0.1** (not v0.2); `bundle_version: "0.2"` documented as producer content-revision extension; `okf_version` scoped to root [index](index.md) frontmatter only.
* Documented link strategy — root-absolute `/path/concept.md` per OKF §5.1 for agent consumption; noted GitHub plain-file browsing may prefer relative links.
* Added official OKF spec citation ([GoogleCloudPlatform/knowledge-catalog okf/SPEC.md](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)); updated `resource` frontmatter on okf-format article.
* Root [index](index.md): confirmed `okf_version: "0.1"` and `bundle_version: "0.2"`; added brief OKF §5.1 link-style note in body.

## 2026-07-06 — scripting systems expansion

### New sections

* [scripted-triggers/](scripted-triggers/) — [basics](scripted-triggers/scripted-trigger-basics.md), [common patterns](scripted-triggers/common-trigger-patterns.md) (`teu_nc_triggers`, vanilla `flavor_teu`).
* [customizable-localization/](customizable-localization/) — [basics](customizable-localization/customizable-localization-basics.md), [country-scoped](customizable-localization/country-scoped-custom-loc.md) (`teu_nc_purpose_tier_name`).
* [country-setup/](country-setup/) — [tags and history](country-setup/country-tags-and-history.md), [setup files](country-setup/setup-countries-files.md).

### Expanded sections

* [events/](events/) — [event triggers and options](events/event-triggers-and-options.md), [hidden effects and AI chance](events/hidden-effects-and-ai-chance.md).
* [on-actions/](on-actions/) — [on-actions overview](on-actions/on-actions-overview.md).
* [localization/](localization/) — [advance and mission localization](localization/advance-and-mission-localization.md), [key conventions](localization/localization-key-conventions.md).

### Index and cross-links

* Updated root [index](index.md), section indexes, [log](log.md).
* Fixed pre-existing broken links listed in v0.2 pass (country-setup, events, localization cross-refs).
* Polished [dynamic-text-in-loc](localization/dynamic-text-in-loc.md) links to customizable-localization section.

## 2026-07-06 — v0.2 complete library

### Bundle status

* Bumped OKF to **v0.2** — root [index](index.md) status line updated; removed *(stub)* labels from missions/governments in root index.
* All new concept articles marked `status: complete` in frontmatter.

### Advances (new)

* [Advance file structure](advances/advance-file-structure.md) — naming, block fields, unlock prefixes, loc keys.
* [Advance triggers and modifiers](advances/advance-triggers-and-modifiers.md) — `potential` vs `allow`, stat keys, skip list for validation.
* [Country-specific advances](advances/country-specific-advances.md) — vanilla `country_TEU.txt` + mod `count_TEU_northern_crusade.txt` pattern.
* Updated [advances/index](advances/index.md); polished [starting technology level](advances/starting-technology-level.md).

### Validation (new + expanded)

* [Error log debugging](validation/error-log-debugging.md) — `Documents/Paradox Interactive/Europa Universalis V/logs/` workflow.
* [Mod validation tooling](validation/mod-validation-tooling.md) — `northern_crusade_teu/tools/validate_mod.py` architecture and CLI flags.
* Expanded [common pitfalls](validation/common-pitfalls.md) — metadata, error log, advance visibility, stat keys, validation tooling rows.
* Updated [validation/index](validation/index.md); polished [smoke testing checklist](validation/smoke-testing-checklist.md).

### References (new + expanded)

* [Paradox wiki and tools](references/paradox-wiki-and-tools.md) — EU5 wiki, forum, community-mod-toolkit, Modhelper.
* [Glossary](references/glossary.md) — DHE, on_action, advance, scope, metadata, OKF terms.
* Expanded [vanilla file locations](references/vanilla-file-locations.md) — modifier types, formables, missions schema, logs path, mod cross-ref table.
* Updated [references/index](references/index.md); bumped [OKF format](references/okf-format.md) to v0.2.

### Getting started (new + expanded)

* [Reading vanilla examples](getting-started/reading-vanilla-examples.md) — grep, diff, read order.
* [Mod metadata and descriptor](getting-started/mod-metadata-and-descriptor.md) — `.metadata/metadata.json` from `northern_crusade_teu`.
* Updated [getting-started/index](getting-started/index.md); polished [mod folder structure](getting-started/mod-folder-structure.md), [workflow and tools](getting-started/workflow-and-tools.md), [enabling your mod](getting-started/enabling-your-mod.md).

### Modifiers (new)

* [Modifier stat keys](modifiers/modifier-stat-keys.md) — `00_modifier_types.txt` discovery workflow.
* [game_data category](modifiers/game-data-category.md) — country vs location scope pairing.
* Updated [modifiers/index](modifiers/index.md); polished [static modifiers](modifiers/static-modifiers.md).

### Other polish

* Updated [governments/index](governments/index.md) — lists all four government articles (was stale stub index).
* Root index now links [Formables](formables/) section.

### Known broken links (pre-existing, outside this pass)

* None from v0.2 list — resolved by scripting systems expansion above.

All links created in this v0.2 pass resolve correctly.

## 2026-07-06 — Playtest UI fixes (Northern Crusade)

* **Update**: [Known issues](validation/known-issues.md) — KI-006, KI-013, KI-023, KI-050, KI-051 from TEU playtest.
* **Update**: [Displaying hidden mechanics](modifiers/displaying-hidden-mechanics.md) — static modifier NAME vs DESC.
* **Update**: [Formable triggers](formables/formable-triggers-and-effects.md) — `potential_requires_own`, plausible game rule.

## 2026-07-06 — Known issues registry

* **Creation**: [Known issues registry](validation/known-issues.md) — agent-oriented KI-### table of prior bugs/mistakes from Northern Crusade and OKF work; linked from root and validation indexes.

## 2026-07-06 — OKF alignment pass (fixes 1–3)

* **Update**: Rewrote [OKF format](references/okf-format.md) — OKF v0.1 vs `bundle_version`, link strategy §5.1, official GitHub SPEC citation.
* **Update**: Root [index](index.md) — note on `/` cross-link convention for agents.
* **Update**: Added `# Citations` to all 20 previously flagged concept articles plus 5 remaining gaps.
* **Update**: Added `resource:` frontmatter to vanilla- and mod-tied concepts (~40 articles); abstract articles (glossary, pitfalls checklist) intentionally omit `resource`.

## 2026-07-06 — v0.1 initialization

* **Initialization**: Created OKF v0.1 bundle `eu5-modding-knowledge/` under Paradox `mod/` folder.
* **Creation**: Root [index](index.md) and section indexes for getting-started, localization, events, on-actions, scripted-effects, modifiers, advances, missions, governments, validation, references.
* **Creation**: Core concept articles from Northern Crusade mod lessons (BOM, event IDs, `trigger_event_non_silently`, DHE, purpose-meter UI pattern).
* **Missions**: Four concepts — [mission trees overview](missions/mission-trees-overview.md), [tasks and triggers](missions/mission-tasks-and-triggers.md), [rewards and effects](missions/mission-rewards-and-effects.md), [localization](missions/mission-localization.md).
