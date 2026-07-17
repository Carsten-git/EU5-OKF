---
type: Reference
title: CMF overview and dependency
description: What the Community Mod Framework provides and how to declare it as a mod dependency.
tags: [cmf, dependency, metadata, multi-mod]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: community_mod_framework
resource: https://github.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework
---

The **Community Mod Framework (CMF)** is a shared EU5 mod that other mods depend on for conflict-safe UI and hooks: Community Mod Menu (CMM), action bar, alerts, banners, action log, on-action hooks, and utilities.

# Philosophy

- Invisible when no dependent mods are active.
- Preserve base-game behavior by default.
- Give mods shared ways to show content / hook the game without each shipping a full menu.

# Declare dependency

In your mod’s `.metadata/metadata.json`:

```json
"relationships": [
  {
    "rel_type": "dependency",
    "id": "community_mod_framework",
    "display_name": "Community Mod Framework",
    "resource_type": "mod",
    "version": "2.*"
  }
]
```

Also add CMF as a **required item** on your Steam Workshop page. Workshop id: `3692202776`.

# Example mod

Reference integration: [submods/cmf-example-mod](https://github.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/tree/main/submods/cmf-example-mod).

# Related

* [Registration hooks](/community-mod-framework/registration-and-on-action-hooks.md)
* [Mod metadata](/getting-started/mod-metadata-and-descriptor.md)
* [Dependency check popup](/community-mod-framework/dependency-check-popup.md)

# Citations

[1] https://github.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/blob/main/README.md
[2] https://raw.githubusercontent.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/main/docs/wiki/cmf.wiki
[3] Steam Workshop `3692202776`
