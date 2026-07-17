---
type: Reference
title: Event ID rules
description: EU5 rejects or mishandles event IDs ending in .0 — use .100 or higher.
resource: mod/northern_crusade_teu/in_game/events/DHE/flavor_teu_nc_purpose.txt
tags: [events, pitfall, ids]
timestamp: 2026-07-06T00:00:00+10:00
status: draft
---

# Rule

**Do not use event IDs ending in `.0`** (e.g. `flavor_teu_nc_purpose.0`).

Observed behavior: event may fire but **title and desc loc keys display literally** (`flavor_teu_nc_purpose.0.title`), or the event fails to register correctly.

# Workaround

Use a high suffix for onboarding / tutorial events:

```txt
flavor_teu_nc_purpose.100 = { … }
```

Vanilla uses the same pattern — see `institution_events` comments in the game files.

# Validation

Reject `.0` IDs in mod tooling:

```python
bad = [i for i in ids if re.search(r"\.0$", i)]
```

# Migration flags

When fixing a broken intro event, bump the "seen" variable (e.g. `teu_nc_purpose_guide_v3_seen`) so existing saves receive the fixed popup once.

# See also

* [Triggering events](triggering-events.md)
* [Event localization naming](/localization/event-localization-naming.md)

# Citations

[1] `northern_crusade_teu` — `.0` intro showed raw loc keys; `.100` resolved.
