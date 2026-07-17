---
type: Reference
title: Paradox wiki and tools
description: External EU5 modding links — wiki, forum, community toolkits, and helpers.
resource: https://eu5.paradoxwikis.com/Europa_Universalis_V_Wiki
tags: [references, external, wiki, tools]
timestamp: 2026-07-06T08:45:00+10:00
status: complete
---

Community-maintained resources complement this bundle. Prefer **vanilla file grep** for syntax truth; use the wiki for load order, metadata, and system overviews.

# Official Paradox

| Resource | URL | Use for |
|----------|-----|---------|
| EU5 Modding hub | [eu5.paradoxwikis.com/Modding](https://eu5.paradoxwikis.com/Modding) | Getting started, encoding, launcher |
| Mod structure | [eu5.paradoxwikis.com/Mod_structure](https://eu5.paradoxwikis.com/Mod_structure) | `.metadata/metadata.json`, folder layout |
| Mod load order | [eu5.paradoxwikis.com/Mod_files_load_order](https://eu5.paradoxwikis.com/Mod_files_load_order) | Overrides, filename conflicts |
| Paradox forums — EU5 modding | [forum.paradoxplaza.com](https://forum.paradoxplaza.com/forum/forums/europa-universalis-v-modding.1170/) | Questions, WIP mods, patch notes |

# Community toolkits

| Project | URL | Use for |
|---------|-----|---------|
| EU5 Modding Co-op — community mod toolkit | [GitHub wiki — Mod Template](https://github.com/Europa-Universalis-5-Modding-Co-op/community-mod-toolkit/wiki/Mod-Template) | Starter metadata, submod layout, upload tooling |
| Community Mod Framework (CMF) | [GitHub](https://github.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework) · [wiki](https://github.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/wiki) · [Steam](https://steamcommunity.com/sharedfiles/filedetails/?id=3692202776) | Shared mod menu, alerts, action bar, hooks — see [community-mod-framework/](/community-mod-framework/) |
| Paradox wiki — CMF quick ref | [eu5.paradoxwikis.com/Community_Mod_Framework](https://eu5.paradoxwikis.com/Community_Mod_Framework) | Short API overview |
| sinjako/EU5-Modhelper | [GitHub](https://github.com/sinjako/EU5-Modhelper) | Browse/edit game defs, generate mod folders with BOM |

# When to use what

| Question | Start here | Then |
|----------|------------|------|
| Where does X file live in vanilla? | [Vanilla file locations](vanilla-file-locations.md) | `rg` in game install |
| How do I register my mod? | [Mod metadata and descriptor](/getting-started/mod-metadata-and-descriptor.md) | Wiki mod structure |
| What does this term mean? | [Glossary](glossary.md) | Wiki or vanilla example |
| Is my modifier key valid? | [Modifier stat keys](/modifiers/modifier-stat-keys.md) | `validate_mod.py` |
| Silent failure in-game | [Error log debugging](/validation/error-log-debugging.md) | This bundle's topic index |

# See also

* [OKF format](okf-format.md)
* [Reading vanilla examples](/getting-started/reading-vanilla-examples.md)
* [Workflow and tools](/getting-started/workflow-and-tools.md)

# Citations

[1] [EU5 Modding hub](https://eu5.paradoxwikis.com/Modding) — Paradox Wiki
[2] [Mod structure](https://eu5.paradoxwikis.com/Mod_structure) — `.metadata/metadata.json`, folder layout
[3] [Mod files load order](https://eu5.paradoxwikis.com/Mod_files_load_order) — overrides and filename conflicts
[4] [Paradox forums — EU5 modding](https://forum.paradoxplaza.com/forum/forums/europa-universalis-v-modding.1170/)
[5] [EU5 Modding Co-op — Mod Template](https://github.com/Europa-Universalis-5-Modding-Co-op/community-mod-toolkit/wiki/Mod-Template)
[6] [sinjako/EU5-Modhelper](https://github.com/sinjako/EU5-Modhelper)
