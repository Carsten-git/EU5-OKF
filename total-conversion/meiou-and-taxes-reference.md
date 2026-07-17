---
type: Reference
title: MEIOU and Taxes as reference
description: How to use the MEIOU and Taxes EU5 workshop mod as a systems-overhaul reference implementation.
tags: [total-conversion, meiou, reference, sources]
timestamp: 2026-07-11T12:30:00+10:00
resource: steam://workshop/3450310/3735059838
status: complete
source_mod: meiou_and_taxes
source_version: "0.1.6"
---

[MEIOU and Taxes](https://steamcommunity.com/sharedfiles/filedetails/?id=3735059838) (MnT) for EU5 is an early systems overhaul (not a full map/timeline TC). This bundle treats it as a **reference implementation** for large-scale EU5 modding patterns — not as a dependency.

**Extract status:** reusable how-tos from workshop v0.1.6 are captured; see [knowledge coverage](/references/knowledge-coverage.md).

# Workshop path

```
C:\Program Files (x86)\Steam\steamapps\workshop\content\3450310\3735059838
```

Metadata (`id`: `meiou_and_taxes`, `supported_game_version`: `1.3.*`). Inspected build: **v0.1.6**.

# What it demonstrates well

| System | Knowledge articles |
|--------|-------------------|
| Architecture | [Three-root + overrides](/total-conversion/three-root-and-override-ladder.md), [naming](/total-conversion/naming-and-subsystem-prefixes.md) |
| RGO → buildings | [RGO substitution](/buildings/rgo-to-building-substitution.md), [building caps](/buildings/building-cap-script-values.md) |
| Estate upkeep / economy | [EPBM](/economy/estate-paid-building-maintenance.md), [event pipeline](/economy/hidden-event-economy-pipeline.md), [goods baskets](/economy/goods-demand-construction-baskets.md) |
| Map / climate / control | [Köppen](/map/koppen-climates.md), [proximity vs control](/map/proximity-vs-control.md), [centers](/map/dynamic-centers-of-importance.md) |
| Military | [Tribal levies](/map/tribal-levies-and-demographics.md), [naval levy chain](/military/naval-transport-levy-chain.md) |
| Governments | [Societal values ↔ estates](/governments/societal-values-estate-power.md) |
| GUI / telemetry | [Custom UI](/gui/custom-ui-patterns.md), [map modes](/gui/custom-map-modes.md), [data-binding macros](/tooling/data-binding-macros.md) |
| Discipline | [Soft-disable](/total-conversion/soft-disable-vanilla-systems.md), [numeric rebalance](/total-conversion/numeric-rebalance-playbook.md), [branding](/total-conversion/loading-screen-branding.md) |
| Tooling | [Toolchain](/tooling/total-conversion-toolchain.md) |

# What it is not

- Not a full map/timeline conversion (no new countries/provinces/history in the workshop build).
- Workshop folder may omit CI configs and some generator inputs from the private repo.
- Balance numbers will churn — copy **patterns**, not scalars.

# How to cite in this bundle

Use frontmatter `source_mod: meiou_and_taxes` and a `# Citations` section pointing at workshop paths or Steam.

# Citations

[1] Workshop content `3450310/3735059838` — MEIOU and Taxes EU5 v0.1.6
[2] Mod `Documentation/Change log.md` — feature and balance intent
[3] [OKF format](/references/okf-format.md) — how this bundle stores knowledge
[4] [Knowledge coverage](/references/knowledge-coverage.md)
