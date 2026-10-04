---
type: Reference
title: MEIOU and Taxes as reference
description: How to use the MEIOU and Taxes EU5 workshop mod and GitHub dev repo as a systems-overhaul reference implementation.
tags: [total-conversion, meiou, reference, sources, github]
timestamp: 2026-07-20T21:30:00+10:00
resource: steam://workshop/3450310/3735059838
status: complete
source_mod: meiou_and_taxes
source_version: "0.1"
---

[MEIOU and Taxes](https://steamcommunity.com/sharedfiles/filedetails/?id=3735059838) (MnT) for EU5 is an early systems overhaul (not a full map/timeline TC). This bundle treats it as a **reference implementation** for large-scale EU5 modding patterns — not as a dependency.

**Extract status:** Workshop v0.1.6 patterns plus GitHub `MEIOU-and-Taxes/MnT-EU5` dev repo (CI, full `tools/`, `scripted_guis/`). Mine via MCP `MnT-EU5 Docs` (`search_MnT_EU5_code`) or clone the repo. See [knowledge coverage](/references/knowledge-coverage.md).

# Sources

| Source | Path / URL | Contents |
|--------|------------|----------|
| Workshop | `Steam/steamapps/workshop/content/3450310/3735059838` | Playable build uploaded to Steam |
| GitHub | `https://github.com/MEIOU-and-Taxes/MnT-EU5` | CI, generators, plot tools, changelog |
| MCP | Cursor server `MnT-EU5 Docs` | Code search; docs index still minimal (README only) |

Metadata (`id`: `meiou_and_taxes`, `supported_game_version`: `1.3.*`). Inspected builds: **Workshop v0.1.6**, **GitHub v0.1** (May 2026 changelog).

# What it demonstrates well

| System | Knowledge articles |
|--------|-------------------|
| Architecture | [Three-root + overrides](/total-conversion/three-root-and-override-ladder.md), [naming](/total-conversion/naming-and-subsystem-prefixes.md) |
| RGO → buildings | [RGO substitution](/buildings/rgo-to-building-substitution.md), [building caps](/buildings/building-cap-script-values.md), [Scripted GUI building filters](/gui/scripted-gui-building-visibility-filters.md) |
| Estate upkeep / economy | [EPBM](/economy/estate-paid-building-maintenance.md), [event pipeline](/economy/hidden-event-economy-pipeline.md), [goods baskets](/economy/goods-demand-construction-baskets.md) |
| Map / climate / control | [Köppen](/map/koppen-climates.md), [proximity vs control](/map/proximity-vs-control.md), [centers](/map/dynamic-centers-of-importance.md) |
| Military | [Tribal levies](/map/tribal-levies-and-demographics.md), [naval levy chain](/military/naval-transport-levy-chain.md), [custom peace treaties](/military/custom-peace-treaties.md) |
| Governments | [Societal values ↔ estates](/governments/societal-values-estate-power.md), [Subject type overrides](/governments/subject-type-overrides.md) |
| Interactions | [REPLACE generic actions](/interactions/replace-generic-actions.md) |
| Economy (extra) | [Land good development sink](/economy/land-good-development-sink.md) |
| Localization | [Main menu event loc mirror](/localization/main-menu-event-localization-mirror.md) |
| GUI / telemetry | [Custom UI](/gui/custom-ui-patterns.md), [map modes](/gui/custom-map-modes.md), [data-binding macros](/tooling/data-binding-macros.md) |
| Discipline | [Soft-disable](/total-conversion/soft-disable-vanilla-systems.md), [numeric rebalance](/total-conversion/numeric-rebalance-playbook.md), [branding](/total-conversion/loading-screen-branding.md) |
| Tooling | [Toolchain](/tooling/total-conversion-toolchain.md), [GitHub CI](/tooling/github-ci-mod-hygiene.md), [Log cleaner](/tooling/error-log-cleaner-rotation.md) |

# What it is not

- Not a full map/timeline conversion (no new countries/provinces/history in the workshop build).
- Workshop folder may omit `.github/` and some generator inputs present on GitHub.
- Balance numbers will churn — copy **patterns**, not scalars.

# How to cite in this bundle

Use frontmatter `source_mod: meiou_and_taxes` and a `# Citations` section pointing at workshop paths, GitHub paths, or Steam.

# Citations

[1] Workshop content `3450310/3735059838` — MEIOU and Taxes EU5
[2] GitHub `MEIOU-and-Taxes/MnT-EU5` — dev repo with CI and tools
[3] Mod `Documentation/Change log.md` — feature and balance intent
[4] [OKF format](/references/okf-format.md) — how this bundle stores knowledge
[5] [Knowledge coverage](/references/knowledge-coverage.md)
