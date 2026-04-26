# Loom

**A personal knowledge fabric for evidence-based, value-anchored thinking across work and life.**

This is the workspace root. Loom is structured as a multi-repo project. Each subdirectory is its own git repository with its own dependency tree, release cadence, and verification gates.

## Repos

| Repo | Language | Purpose | Status |
|---|---|---|---|
| [`loom-meta/`](./loom-meta/) | Markdown / Bash | Planning, design docs, PRD, issue tracking, Ralph agent scripts | **active** |
| [`loom-core/`](./loom-core/) | Python 3.13 | FastAPI daemon — sole writer to SQLite + Obsidian vault | **active** |
| [`loom-mcp/`](./loom-mcp/) | Python 3.13 | MCP server — Claude Desktop integration (thin wrapper over Loom Core) | **active** |
| `loom-apple-ai/` | Swift 6 / Vapor | Apple Foundation Models sidecar — on-device LLM | **deferred** (W12) |
| `loom-ui/` | SwiftUI | Triage and migration-review surface | **deferred** (W11) |

The Swift repos are scaffolded when their first issue opens, not before — staying disciplined about not spinning up infrastructure ahead of the slice that needs it.

## Process topology

```
Claude Desktop ──stdio──► loom-mcp ──HTTP/localhost:9100──► loom-core ──HTTP/localhost:9101──► loom-apple-ai
                                                                │
                                                                ├──► SQLite (~/Library/Application Support/Loom/db/)
                                                                │
                                                                └──► Obsidian vault (~/Documents/Loom/)
                                                                          ▲
                                                                          └── Obsidian Mac/iOS read fallback (iCloud-synced)

                            Loom UI (SwiftUI) ──HTTP/localhost:9100──► loom-core
```

## Where to start

- **Read this first:** [`loom-meta/issues/prd.md`](./loom-meta/issues/prd.md) — the v1 PRD.
- **The constitution:** [`loom-meta/docs/`](./loom-meta/docs/) — design, blueprint, system design, schema, API spec, projections, style guide.
- **Methodology:** `../ai-engineer-workshop-2026-project/PROJECT_SETUP_GUIDE.md` — the workshop methodology this project follows.
- **Issue list:** [`loom-meta/issues/`](./loom-meta/issues/) — `NNN-*.md` vertical slices (decomposed from the PRD).

## v1 scope

**Work domain only**, **macOS 26 Tahoe**, **Apple Silicon**. Other domains (finance, content, code, health, personal) are deferred to v2+.

## Methodology

The workshop's four-phase methodology applies here:

```
EXPLORE → PLAN → IMPLEMENT → COMMIT
```

With Python adaptations to the verification gates (per-repo):

```bash
# loom-core, loom-mcp
uv run ruff check
uv run ruff format --check
uv run mypy --strict
uv run pytest
```

A task is not complete until all gates relevant to its repo pass with zero errors.

## Issue tagging

Default: **AFK** (Ralph-runnable autonomously). HITL exceptions are listed in `loom-meta/issues/prd.md` §9.

---

*April 2026 · Phani*
