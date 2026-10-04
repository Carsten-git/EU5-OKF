# GUI and custom UI

Scripted GUI, HUD sync, map modes, and UI-overhaul architecture (MnT + Glorp + CMF + Construction Manager).

* [Custom UI patterns](custom-ui-patterns.md) — scripted GUI, ring-buffer charts, `aaa_` precedence
* [Custom map modes](custom-map-modes.md) — heatmaps + Glorp/CM/ZMC checklists
* [Live vs cached mapmode metrics](live-vs-cached-mapmode-metrics.md) — script_value live vs location-var cache (ZMC-1)
* [Mapmode localization and game concepts](mapmode-localization-and-concepts.md) — loc, legends, textformatting (ZMC-2)
* [Never-trigger-me workaround](never-trigger-me-workaround.md) — prefer `cmf_suppress` when on CMF
* [HUD value sync](hud-value-sync.md) — monthly aggregates for HUD (#3)
* [CMFG vanilla-type extraction](cmfg-vanilla-type-extraction.md) — extracted types for safer overrides (#5)
* [UI override file layering](ui-override-file-layering.md) — shared/panels/type shadowing (#8/#20/#22)
* [ScriptedGui IsValid button greying](scripted-gui-isvalid-greying.md) — visible but disabled when gates fail
* [Script values and ROI in GUI](script-values-and-roi-in-gui.md) — GuiScope script values + ROI aliases (#12/#13)
* [Mass-action UI counter](mass-action-ui-counter.md) — batch UI countdown (#15)
* [Mapmode hover preview](mapmode-hover-preview.md) — temporary mapmode on hover (#18)
* [UI overhaul niche patterns](ui-overhaul-niche-patterns.md) — low-pri Glorp (#19/#21/#23)
* [Glorp location RGO row](glorp-location-window-rgo-row.md) — where to patch RGO in Glorp (#25)
* [Integrating with Glorp UI](integrating-with-glorp-ui.md) — content-mod compat playbook (#26)
* [Expand raw goods lateral view](expand-raw-goods-lateralview.md) — bulk RGO panel + row injection (#30)
* [Off-screen scripted widget drivers](offscreen-scripted-widget-drivers.md) — CM construct queues / classification (CM-1)
* [Search filters for lists](search-filters-for-lists.md) — `gui/filters/` (CM-2)
* [Context-specific widget aliases](context-specific-widget-aliases.md) — `_pl` / `_bv` wrappers (CM-3)
* [Mass opt-in / opt-out location flags](mass-opt-in-opt-out-location-flags.md) — mass + exclusion vars (CM-4)
* [Scripted GUI building visibility filters](scripted-gui-building-visibility-filters.md) — hide RGO-replacement buildings (MnT)

Reference maps: [Construction Manager](/references/construction-manager-as-reference.md) · [Zorange mapmodes](/references/zorange-mapmode-collection-as-reference.md)
