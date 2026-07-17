---
type: Reference
title: OKF format
description: Open Knowledge Format v0.1 — how this bundle is structured.
tags: [meta, okf]
timestamp: 2026-07-06T09:00:00+10:00
resource: https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md
status: complete
---

This bundle conforms to **Open Knowledge Format (OKF) v0.1** [1].

# Version fields

| Field | Location | Meaning |
|-------|----------|---------|
| `okf_version` | Root [index.md](/index.md) frontmatter only | OKF spec version this bundle follows (`"0.1"`) |
| `bundle_version` | Root [index.md](/index.md) frontmatter only | Producer extension — content revision of this bundle (`"0.8"`) |

`okf_version` is **not** repeated on concept files or section indexes. Only the bundle root `index.md` may declare `okf_version` and `bundle_version`.

# Rules used here

- Each concept = one `.md` file with YAML frontmatter (`type` required).
- `index.md` in each directory = progressive disclosure (no frontmatter except bundle root).
- `log.md` at bundle root = changelog.
- Cross-links use bundle-root paths per OKF §5.1: `/events/triggering-events.md`.

# Link strategy

Concept and index cross-links in this bundle use **root-absolute paths** (`/section/concept.md`) as specified in OKF §5.1 for stable agent consumption.

When browsing the bundle as plain files on GitHub, relative links (`section/concept.md`) can be more convenient for click navigation. We intentionally keep `/`-prefixed paths for agent stability and consistent resolution regardless of the reader's current directory.

# Bundle root

Root [index.md](/index.md) frontmatter declares:

```yaml
okf_version: "0.1"
bundle_version: "0.8"
```

# Contributing a new concept

1. Create `section/my-topic.md` with frontmatter (`type`, `title`, `description`, `tags`, `timestamp`, `status: complete` when finished).
2. Add optional `source_mod:` / `source_version:` when knowledge is extracted from a specific mod.
3. Add a bullet to `section/index.md`.
4. Link from related concepts using `/section/my-topic.md`.
5. Append [log.md](/log.md).

# Concept types in this bundle

| type | Usage |
|------|--------|
| Reference | Factual how-to, syntax, file paths |
| Playbook | Step-by-step workflow or pattern |

# Citations

[1] [Open Knowledge Format (OKF) Specification](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) — Google Cloud Platform knowledge-catalog, OKF v0.1
