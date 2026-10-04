# EU5 Modding Knowledge (Engineering OKF)

**Open Knowledge Format (OKF) engineering bundle** for [*Europa Universalis V*](https://www.paradoxinteractive.com/games/europa-universalis-v) mod development.

Reusable **how-to** articles — file paths, script patterns, validation playbooks, and reference-mod techniques — so humans and coding agents can **build new mods** without re-mining the workshop each time.

| | |
|---|---|
| **Format** | [Open Knowledge Format v0.1](references/okf-format.md) |
| **Bundle version** | See [index.md](index.md) frontmatter (`bundle_version`) |
| **Changelog** | [log.md](log.md) |
| **Catalog** | [index.md](index.md) — full topic index |

---

## What this is (and is not)

**This bundle is:**

- **Engineering knowledge** — triggers, effects, GUI, on-actions, economy APIs, CMF, validation, tooling.
- **Agent-oriented** — capability router, stable `/section/article.md` links, playbooks with checklists.
- **Evidence-based** — patterns extracted from vanilla and reference mods (MnT, Glorp UI, Construction Manager, Sire, etc.).

**This bundle is not:**

- Player-facing mod documentation (that lives in each mod’s own README / Workshop page).
- Product requirements or design canon (e.g. Sire uses a separate **product OKF** under `development/Sire, Who Bound This Manor to a Single Merchandise/`).
- A substitute for the game install or Paradox wiki — always verify against your EU5 version.

---

## Where it lives

Default local path (Windows):

```text
Documents/Paradox Interactive/Europa Universalis V/mod/eu5-modding-knowledge/
```

Related paths:

| What | Typical location |
|------|------------------|
| Vanilla game | `Steam/steamapps/common/Europa Universalis V/game/` |
| Workshop mods | `Steam/steamapps/workshop/content/3450310/<id>/` |
| Local mods | `Documents/Paradox Interactive/Europa Universalis V/mod/<name>/` |
| EU5 logs | `Documents/Paradox Interactive/Europa Universalis V/logs/` |

Use quoted paths when `(x86)` appears in Steam directories.

---

## Quick start

### Humans

1. Open **[index.md](index.md)** for the full table of contents.
2. New mod → **[Building a mod with this knowledge base](getting-started/building-a-mod-with-this-kb.md)** (checklist by mod shape).
3. Know the capability you need → **[Agent topic router](references/agent-topic-router.md)** (settings, RGO, Glorp, mapmodes, CMF, validation).
4. Before shipping → **[Smoke testing checklist](validation/smoke-testing-checklist.md)** and **[Common pitfalls](validation/common-pitfalls.md)**.

### Coding agents (Cursor, etc.)

1. Read **[references/agent-topic-router.md](references/agent-topic-router.md)** first — do not invent APIs.
2. Follow **[getting-started/building-a-mod-with-this-kb.md](getting-started/building-a-mod-with-this-kb.md)** for scaffold → shape → implement → validate.
3. On `error.log` / telemetry → **[validation/script-logging-and-telemetry.md](validation/script-logging-and-telemetry.md)**.
4. Gaps → **[references/knowledge-coverage.md](references/knowledge-coverage.md)**; extend the bundle rather than guessing.

Cursor skill for **extracting** new patterns into this bundle: `eu5-mod-knowledge-extract` (personal skills).  
Cursor skill for **using** this bundle when coding mods: `eu5-okf-modding`.

---

## Bundle layout

```text
eu5-modding-knowledge/
├── README.md              ← you are here (human entry)
├── index.md               ← OKF bundle root + version frontmatter
├── log.md                 ← changelog (newest first)
├── getting-started/       ← scaffold, workflow, agent checklist
├── events/                ← events, DHE, hidden effects
├── on-actions/            ← pulses, orchestration, performance
├── economy/               ← RGO, markets, goods, AI decision patterns
├── gui/                   ← UI overrides, map modes, Glorp integration
├── community-mod-framework/
├── game-rules/
├── total-conversion/
├── validation/            ← pitfalls, error.log, telemetry, smoke tests
├── tooling/               ← codegen, TC toolchain, validators
└── references/            ← OKF spec, topic router, vanilla file index
```

Each **concept** is one `.md` file with YAML frontmatter (`type: Reference` or `Playbook`). Section folders have an `index.md` for progressive disclosure.

---

## High-value entry points

| I want to… | Start here |
|------------|------------|
| Scaffold a new mod | [mod-folder-structure.md](getting-started/mod-folder-structure.md) |
| Add campaign game rules | [game-rules/](game-rules/) |
| CMF / pause-menu settings | [community-mod-framework/](community-mod-framework/) |
| Change location RGO | [economy/change-raw-material.md](economy/change-raw-material.md) |
| Peace conference / dismantle forts | [military/custom-peace-treaties.md](military/custom-peace-treaties.md) |
| Event localization paths | [localization/main-menu-event-localization-mirror.md](localization/main-menu-event-localization-mirror.md) |
| AI convert using live prices | [economy/rgo-conversion-ai-market-decision.md](economy/rgo-conversion-ai-market-decision.md) |
| `price_in_market` in script | [economy/price-in-market-script-api.md](economy/price-in-market-script-api.md) |
| Glorp UI compatibility | [references/glorp-ui-as-reference.md](references/glorp-ui-as-reference.md) |
| Custom map modes | [gui/custom-map-modes.md](gui/custom-map-modes.md) |
| Debug via `error.log` | [validation/error-log-debugging.md](validation/error-log-debugging.md) |
| CI / BOM / LF on PRs | [tooling/github-ci-mod-hygiene.md](tooling/github-ci-mod-hygiene.md) |
| Dedupe noisy error.log | [tooling/error-log-cleaner-rotation.md](tooling/error-log-cleaner-rotation.md) |
| Scoped telemetry (pulses / AI) | [validation/script-telemetry-via-hidden-events.md](validation/script-telemetry-via-hidden-events.md) |
| Balance telemetry (save + picks) | [validation/rgo-balance-telemetry-pipeline.md](validation/rgo-balance-telemetry-pipeline.md) |
| Find vanilla data files | [references/vanilla-master-data-index.md](references/vanilla-master-data-index.md) |
| Known agent mistakes | [validation/known-issues.md](validation/known-issues.md) |

---

## Contributing new knowledge

When you learn a **reusable** pattern from a mod or playtest:

1. Check **[references/knowledge-coverage.md](references/knowledge-coverage.md)** and grep the bundle — **extend** existing articles before adding duplicates.
2. Add `section/my-topic.md` with frontmatter (`type`, `title`, `description`, `tags`, `timestamp`, `status`).
3. Link from the section `index.md` and from **[references/agent-topic-router.md](references/agent-topic-router.md)** if it is a new capability path.
4. Append **[log.md](log.md)** and bump `bundle_version` in **[index.md](index.md)** when the change is substantial.

Follow **[references/okf-format.md](references/okf-format.md)** for link style (`/section/file.md` inside concept bodies).

Extract workflow: Cursor skill **`eu5-mod-knowledge-extract`**.

---

## Reference mods cited in this bundle

Workshop IDs and local names appear throughout; common anchors:

| Mod | Workshop ID | Role in KB |
|-----|-------------|------------|
| Community Mod Framework | `3692202776` | Shared UI / settings APIs |
| Glorp UI | `3601047146` | Location/window UI patterns |
| Construction Manager | `3736668860` | Automation + deep GUI |
| Zorange Mapmode Collection | `3697317887` | Additive map modes |
| MEIOU and Taxes | `3735059838` / [GitHub MnT-EU5](https://github.com/MEIOU-and-Taxes/MnT-EU5) | Total-conversion reference + CI/tooling |
| Sire (RGO conversion) | local | Live economy / telemetry patterns |

---

## Link conventions

| Audience | Links |
|----------|--------|
| **Inside OKF articles** | Root-absolute: `/economy/change-raw-material.md` (agent-stable) |
| **This README** | Relative: `economy/change-raw-material.md` (browser / GitHub friendly) |

Full rules: [references/okf-format.md](references/okf-format.md).

---

## License and game files

This bundle documents **techniques** for modding Paradox games. It does not redistribute game assets. You need a legal copy of Europa Universalis V and must comply with Paradox’s modding terms.

---

## See also

- **[index.md](index.md)** — canonical OKF catalog and `bundle_version`
- **[log.md](log.md)** — what changed and when
- **Sire product OKF** — `development/Sire, Who Bound This Manor to a Single Merchandise/` (requirements, design, implementation; not duplicated here)
