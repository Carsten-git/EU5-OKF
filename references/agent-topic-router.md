---
type: Reference
title: Agent topic router
description: Capability → OKF article map for agents drafting designs or solution options — settings, Glorp, RGO, mapmodes, CMF.
tags: [meta, agents, router, okf]
timestamp: 2026-07-13T17:20:00+10:00
status: complete
---

Use this page when you know the **capability** you need but not which section. Prefer these links over inventing APIs. Check [knowledge coverage](/references/knowledge-coverage.md) for gaps.

**Maintenance:** when `eu5-mod-knowledge-extract` adds a new capability path, add/adjust a row here. Keep rows capability-oriented — not product REQ ids or frozen per-feature read packs.

# Player-facing settings (choose surface first)

| Need | Start here |
|------|------------|
| Compare campaign-start vs mid-game toggles | [Game rules vs CMM](/game-rules/game-rules-vs-cmm.md) |
| Additive vanilla Game Rules + `has_game_rule` | [Custom game rules](/game-rules/custom-game-rules.md) |
| Human-only modifiers from rules | [Player-scoped modifiers from game rules](/game-rules/player-scoped-modifiers-from-rules.md) |
| CMM / pause-menu settings (needs CMF) | [Player options without lobby](/community-mod-framework/player-options-without-lobby.md), [CMM catalog](/community-mod-framework/cmm-settings-catalog-design.md) |
| CMM without CMF present | [Dual settings UI fallback](/community-mod-framework/dual-settings-ui-fallback.md) |
| Ordered feature list → monthly switch | [CMM priority feature dispatcher](/community-mod-framework/cmm-priority-feature-dispatcher.md) |

# Glorp / location / RGO UI

| Need | Start here |
|------|------------|
| Pattern map | [Glorp UI as reference](/references/glorp-ui-as-reference.md) |
| Compat with content mods | [Integrating with Glorp UI](/gui/integrating-with-glorp-ui.md) |
| Where RGO controls live in Glorp | [Glorp location RGO row](/gui/glorp-location-window-rgo-row.md) |
| Bulk RGO lateral view | [Expand raw goods lateral view](/gui/expand-raw-goods-lateralview.md) |
| File REPLACE / type shadowing | [UI override file layering](/gui/ui-override-file-layering.md) |
| Context wrappers (`_pl` / `_bv` / …) | [Context-specific widget aliases](/gui/context-specific-widget-aliases.md) |
| Peer-detect + deep automation UI | [Construction Manager as reference](/references/construction-manager-as-reference.md) |

# RGO / economy scripting

| Need | Start here |
|------|------------|
| Change location raw material | [Change raw material](/economy/change-raw-material.md) |
| CE-style conversion flow | [Columbian Exchange RGO pattern](/economy/columbian-exchange-rgo-pattern.md) |
| Live `price_in_market` in triggers/effects | [price_in_market script API](/economy/price-in-market-script-api.md) |
| AI convert using market economics | [RGO conversion AI market decision](/economy/rgo-conversion-ai-market-decision.md) — patterns (generic) |
| RGO wealth / income / tax base UI vs script | [RGO UI profit metrics](/economy/rgo-ui-profit-metrics.md) |
| `food` vs goods market price / RGO revenue | [Goods food vs market price](/economy/goods-food-vs-market-price.md) |
| RGO output per level / default price balance | [RGO baseline output and price balance](/economy/rgo-baseline-output-and-price-balance.md) |
| Where vanilla master data files live | [Vanilla master data index](/references/vanilla-master-data-index.md) |
| Construction duration / map ring | [Construction map markers](/buildings/construction-map-markers.md) |
| AI / monthly pulse cost | [Mod performance pulses and scans](/on-actions/mod-performance-pulses-and-scans.md) |
| Balance telemetry (save prices + pick logs) | [RGO balance telemetry pipeline](/validation/rgo-balance-telemetry-pipeline.md), [Market price history from save](/validation/market-price-history-from-save.md) |
| Mass location opt-in/out flags | [Mass opt-in / opt-out location flags](/gui/mass-opt-in-opt-out-location-flags.md) |

# Map modes / filters

| Need | Start here |
|------|------------|
| Additive map mode checklist | [Custom map modes](/gui/custom-map-modes.md) |
| Live script_value vs cached var | [Live vs cached mapmode metrics](/gui/live-vs-cached-mapmode-metrics.md) |
| Loc, legends, game concepts | [Mapmode localization and concepts](/gui/mapmode-localization-and-concepts.md) |
| Utility collection worked example | [Zorange mapmodes reference](/references/zorange-mapmode-collection-as-reference.md) |
| Lateral-view search filters | [Search filters for lists](/gui/search-filters-for-lists.md) |
| Off-screen GUI construct drivers | [Off-screen scripted widget drivers](/gui/offscreen-scripted-widget-drivers.md) |

# CMF / multi-mod chrome

| Need | Start here |
|------|------------|
| Depend on CMF | [Overview and dependency](/community-mod-framework/overview-and-dependency.md) |
| Action bar / alerts | [Action bar and alerts](/community-mod-framework/action-bar-and-alerts.md) |
| Registration hooks | [Registration and on-action hooks](/community-mod-framework/registration-and-on-action-hooks.md) |
| Peer mod detection | [Cross-mod GUI integration](/community-mod-framework/cross-mod-gui-integration.md) |

# Always before novel tricks

| Need | Start here |
|------|------------|
| `error_log` / script telemetry concepts (bindings, prices, vanilla) | [Script logging and telemetry](/validation/script-logging-and-telemetry.md) |
| Scoped `error_log` from pulses / AI (ROOT nullptr in scripted_effects) | [Script telemetry via hidden events](/validation/script-telemetry-via-hidden-events.md) |
| Post-ship balance / cobweb from long observer runs | [RGO balance telemetry pipeline](/validation/rgo-balance-telemetry-pipeline.md) |
| Read `error.log` / binding failure lines | [Error log debugging](/validation/error-log-debugging.md) |
| MnT-style `::TG::` delimiter dumps | [Total conversion toolchain](/tooling/total-conversion-toolchain.md), [Data-binding macros](/tooling/data-binding-macros.md) |

* [Common pitfalls](/validation/common-pitfalls.md) · [Known issues](/validation/known-issues.md)
* Grep vanilla `Europa Universalis V/game/` when this router and section indexes are silent

# Related

* [Vanilla master data index](/references/vanilla-master-data-index.md)
* [Building a mod with this KB](/getting-started/building-a-mod-with-this-kb.md)
* [Knowledge coverage](/references/knowledge-coverage.md)
