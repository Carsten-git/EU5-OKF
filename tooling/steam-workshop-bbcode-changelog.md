---
type: Reference
title: Steam Workshop BBCode changelog
description: Format Workshop update notes with h1/h2, lists, and plain fallback when bullets break.
tags: [steam, workshop, bbcode, release-notes]
timestamp: 2026-07-27T07:55:00+10:00
status: complete
source_mod: tradeable_maps
source_version: "0.1"
---

Store **`.bbcode` files in the mod repo** — not markdown — for paste into Steam Workshop description or changelog fields.

# Tags (Steam-supported)

| Tag | Use |
|-----|-----|
| `[h1]` … `[h2]` | Title / section |
| `[b]` | Emphasis |
| `[list]` `[*]` item `[*]` … `[/list]` | Bullets (compact one-line lists work well) |
| `[url=https://…]` | Links (optional) |

Preview in the Workshop editor before publishing.

# Compact list pattern

```text
[list][*] First point.
[*] Second point.[/list]
```

If preview shows empty `[]` bullets, Steam ate the `*` in `[*]` — use a **PLAIN** variant: `[h2]` sections + paragraph breaks, no `[list]`.

# Example files

| Mod | Files |
|-----|--------|
| Sire RGO conversion | `STEAM_UPDATE_0.2.0.bbcode`, `STEAM_UPDATE_0.2.0_PLAIN.bbcode` |
| Tradeable Maps | `STEAM_UPDATE_0.1.0.bbcode`, `STEAM_UPDATE_0.1.0_PLAIN.bbcode` |

Player-docs skill (`sire-player-docs`) — generate from canonical design; for small mods, ship changelog `.bbcode` beside `README.md`.

# Related

* [UTF-8 BOM requirement](/localization/utf8-bom-requirement.md) — loc files for in-game strings
* [Map knowledge diplomacy pattern](/interactions/map-knowledge-diplomacy-pattern.md) — example changelog content

# Citations

[1] Steam Workshop BBCode (editor preview)
[2] `mod/Sire, Who Bound This Manor to a Single Merchandise/STEAM_UPDATE_*.bbcode`
[3] `mod/Tradeable Maps/STEAM_UPDATE_0.1.0.bbcode`
