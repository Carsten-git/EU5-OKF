---
type: Playbook
title: Building a mod with this knowledge base
description: Agent-oriented checklist — which OKF articles to read before creating or extending an EU5 mod.
tags: [getting-started, playbook, agents, okf]
timestamp: 2026-07-13T17:20:00+10:00
status: complete
---

This bundle exists so agents can **build new mods**, not only document them. Follow this checklist; skip sections that do not apply.

**Capability → article map:** [Agent topic router](/references/agent-topic-router.md) (settings, Glorp, RGO, mapmodes, CMF).

**Vanilla file locations (goods, locations, prices):** [Vanilla master data index](/references/vanilla-master-data-index.md).

# 1. Scaffold

1. [Mod folder structure](/getting-started/mod-folder-structure.md)
2. [Mod metadata and descriptor](/getting-started/mod-metadata-and-descriptor.md)
3. [Enabling your mod](/getting-started/enabling-your-mod.md)
4. [UTF-8 BOM](/localization/utf8-bom-requirement.md)

# 2. Pick a shape

| Shape | Read next |
|-------|-----------|
| Flavor (missions, DHE, country modifiers) | [Events](/events/), [Missions](/missions/), [Modifiers](/modifiers/) |
| **Campaign-start house rules** (ironman-visible, no CMF) | [Game rules](/game-rules/) — start with [vs CMM](/game-rules/game-rules-vs-cmm.md) |
| **Mid-game / pause-menu settings** (shared UI) | [Community Mod Framework](/community-mod-framework/) — [player options](/community-mod-framework/player-options-without-lobby.md) + [CMM](/community-mod-framework/community-mod-menu.md) |
| Unsure settings surface | [Game rules vs CMM](/game-rules/game-rules-vs-cmm.md) then [topic router](/references/agent-topic-router.md) |
| Systems overhaul on vanilla map | [Total conversion](/total-conversion/), [Startup world surgery](/total-conversion/startup-world-surgery.md), [Pulse orchestration](/on-actions/pulse-orchestration.md) |
| Patch vanilla definitions | [Override ladder](/total-conversion/three-root-and-override-ladder.md) — INJECT first |
| Custom UI / charts | [GUI](/gui/), [Glorp reference](/references/glorp-ui-as-reference.md), [Never-trigger-me](/gui/never-trigger-me-workaround.md) |
| **Map modes** | [Custom map modes](/gui/custom-map-modes.md), [live vs cached](/gui/live-vs-cached-mapmode-metrics.md), [ZMC reference](/references/zorange-mapmode-collection-as-reference.md) |
| Change location RGO in script | [Change raw material](/economy/change-raw-material.md), [CE pattern](/economy/columbian-exchange-rgo-pattern.md), [Construction map markers](/buildings/construction-map-markers.md), [Mod performance](/on-actions/mod-performance-pulses-and-scans.md) |
| Glorp / CM-compatible location RGO UI | [Integrating with Glorp](/gui/integrating-with-glorp-ui.md), [CM reference](/references/construction-manager-as-reference.md) |
| Engine constants | [Loading-screen defines](/total-conversion/loading-screen-defines.md) |
| Checksum / ironman warning | [Checksum and gameplay mods](/getting-started/checksum-and-gameplay-mods.md) |

# 3. Implement

- Prefer scripted effects/triggers for reuse ([scripted-effects](/scripted-effects/), [scripted-triggers](/scripted-triggers/)).
- Loc keys: [localization key conventions](/localization/localization-key-conventions.md).
- Hidden mechanics UI: [displaying hidden mechanics](/modifiers/displaying-hidden-mechanics.md).

# 4. Validate

1. [Smoke testing checklist](/validation/smoke-testing-checklist.md)
2. [Error log debugging](/validation/error-log-debugging.md)
3. [Common pitfalls](/validation/common-pitfalls.md)
4. [Known issues](/validation/known-issues.md)
5. Before ship: Cursor skill **`eu5-mod-hygiene`** — BOM pass, loc mirrors, log triage (any mod)

# 5. If stuck

1. [Agent topic router](/references/agent-topic-router.md) — capability search
2. [Reading vanilla examples](/getting-started/reading-vanilla-examples.md)
3. Reference mods: Glorp `3601047146`, CM `3736668860`, ZMC `3697317887`, MnT ([reference](/total-conversion/meiou-and-taxes-reference.md)), `northern_crusade_teu/`
4. Check [knowledge coverage](/references/knowledge-coverage.md) — gap may be real; mine another mod or vanilla and extend the KB

# 6. Always update this knowledge base

When a session discovers a non-obvious EU5 fact (crash cause, invisible UI, wrong effect name, map/marker behavior, loc pairing):

1. Add or correct an OKF article (do not leave it only in chat)
2. Append a [known-issues](/validation/known-issues.md) row if it was a real bug/mistake
3. Add a [common-pitfalls](/validation/common-pitfalls.md) symptom row when reusable
4. Append [log.md](/log.md); bump `bundle_version` on root `index.md` when adding several concepts
5. Update [agent topic router](/references/agent-topic-router.md) if you added a new capability path
6. Prefer engineering how-tos here; keep product design in the **mod’s** product OKF / docs

Use skill `eu5-mod-knowledge-extract` when mining another mod; use this checklist when building.

# Cursor skills

- `eu5-okf-modding` — use this bundle while building
- `eu5-mod-hygiene` — post-build cleanup (BOM, loc mirrors, error.log)
- `eu5-mod-knowledge-extract` — mine another mod into OKF
