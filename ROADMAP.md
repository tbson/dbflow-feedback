# Schemity Roadmap

What we recently shipped, what we are building, and what we are considering. This page exists so you can see where [Schemity](https://schemity.com) is heading - and influence it: if something in "Considering" matters to you, [open a feature request](https://github.com/tbson/schemity-feedback/issues/new?assignees=&labels=enhancement&template=feature_request.md&title=%5BFEATURE%5D+) or vote with a 👍 on the existing issue.

## Recently shipped

- **Context Map** (v2.8.0) - a bird's-eye view that renders each context view as a single node with dependency arrows between them, foreign-key count badges, and curved arrows that expose circular dependencies at a glance; the AI chat can re-arrange the map and analyze it for indirect dependency cycles
- **ON DELETE CASCADE indicator** (v2.8.1) - cascading deletes are drawn with a bold crow's foot at the child end, visible on the canvas without opening any dialog
- **Mermaid export** (v2.8.0) - export a diagram or the Context Map as a Mermaid file that renders natively on GitHub, GitLab, Notion, and Obsidian
- **Markdown descriptions** (v2.7.0) - entities, legends, and context views carry markdown descriptions, opened from a note icon on the canvas
- **DBML import and export** - move schemas to and from dbdiagram.io and the wider DBML toolchain in one step
- **Re-sync** - reopening a reverse-engineered diagram pulls the latest schema from the live database; existing entities keep their layout
- **Merge-aware copy/paste between diagrams** - pasting an entity that already exists in the target transfers only its position, size, and color, so layouts move between diagrams safely
- **Local AI via Ollama** - the diagram-editing AI chat against models on your own machine, zero network egress

## Building now

- **Database views and materialized views** - introspected across all supported dialects and displayed as read-only entities, visually distinct from base tables (italic names, a view/mview token in the entity footer), importable into context views like any other entity

## Considering

If any of these matter to you, open a feature request or vote with a 👍 on the existing issue - that is exactly what moves them up this list.

- **Headless CLI** - render SVG/PNG, emit SQL or DBML from the diagram file, and a CI check that fails when the committed ERD drifts from a target database
- **Large-schema abstraction** - collapse entities to title-only or keys-only, a minimap, and a focus mode that dims everything not connected to the selected entity
- **Comments and data dictionary** - import `COMMENT ON` during reverse engineering, and export a static data dictionary you can commit next to your code
- **Pre-sync drift preview** - before a re-sync applies, a reviewable summary of what will be added, changed, and removed
- **File-vs-file schema diff** - diff two versions of a diagram file (or two git commits) into a visual changelog and generated ALTER statements
- **Notation switching** - numeric/min-max cardinalities as an alternative to crow's foot

---

This roadmap is directional, not a commitment - priorities shift based on the feedback in this repository. Release details land in the [changelog](CHANGELOG.md) when features ship.
