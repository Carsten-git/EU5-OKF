---
type: Reference
title: Knowledge coverage
description: What the OKF bundle covers well, what remains thin, and fitness for agents building new mods.
tags: [meta, coverage, gaps, okf]
timestamp: 2026-07-13T17:10:00+10:00
status: complete
---

Honest coverage map for agents. **Goal:** enable building new EU5 mods.

# Fitness snapshot

| Capability | Status |
|------------|--------|
| Scaffold + loc + flavor (events/missions) | **Strong** |
| CMF/CMM multi-mod UI APIs | **Strong** |
| UI overhaul architecture (Glorp #1–31) | **Strong** |
| Construction automation UI (CM) | **Good** — [CM reference](/references/construction-manager-as-reference.md) |
| **Utility / additive map modes** | **Good** — [custom map modes](/gui/custom-map-modes.md), [live vs cached](/gui/live-vs-cached-mapmode-metrics.md), [ZMC reference](/references/zorange-mapmode-collection-as-reference.md) |
| Glorp + content-mod GUI compat | **Good** |
| CMM player settings / game rules | **Good** |
| Lateral-view search filters | **Good** |
| Systems overhaul (MnT) | **Strong** — GitHub MCP for ongoing mining |
| Player RGO change + construction/map UX | **Strong** |
| Economy diplomacy / map commerce (gold transfers) | **Good** — [economy diplomacy gold](/interactions/country-interactions-economy-diplomacy-gold.md), [map knowledge pattern](/interactions/map-knowledge-diplomacy-pattern.md) |
| Warfare / map TC / deep religion | **Weak** — [custom peace treaties](/military/custom-peace-treaties.md) added; religion/casus belli still thin |

# Source extracts

| Source | Status |
|--------|--------|
| MEIOU and Taxes v0.1.6 + GitHub `MnT-EU5` | Reusable how-tos complete; CI, log cleaner, scripted GUI filters, generic action REPLACE, subject overrides added 2026-07-20 |
| Community Mod Framework | Core APIs ingested |
| Glorp UI v1.3.10.1 | Patterns **#1–31** |
| Construction Manager `3736668860` | Patterns **CM-1–7** |
| **Zorange's Mapmode Collection** `3697317887` | Patterns **ZMC-1–3** — live/cached metrics, loc/concepts, additive checklist |
| Player Speed Game Rules `3755676844` | Additive game rules |
| Vanilla RGO / Columbian Exchange | Economy articles |
| RGO Conversion / Sire | Live engineering + product OKF |
| Tradeable Maps | [Map knowledge diplomacy](/interactions/map-knowledge-diplomacy-pattern.md), economy diplomacy gold |
| Northern Crusade TEU | Early flavor foundation |

# Gaps after MnT GitHub extract (2026-07-20)

* Deep religion / casus belli / wargoal authoring — grep vanilla when needed
* Full `land` good implementation in MnT repo is commented out — pattern documented; verify before citing as shipped behaviour

# Gaps after ZMC extract

* Exhaustive refresh-counter enum (beyond Month / TopographyVegetationDatabaseUpdate / Year) — sample as needed from vanilla
* Literacy-style `every_pop` mapmode performance ceiling — noted as lag risk only

# Citations

[1] Bundle `eu5-modding-knowledge/` v0.17 — [agent topic router](/references/agent-topic-router.md)
[2] Workshop ZMC `3697317887`
[3] Workshop CM `3736668860`, Glorp `3601047146`, Player Speed `3755676844`
