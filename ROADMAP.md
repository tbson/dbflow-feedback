# Schemity Roadmap

What we recently shipped, what we are building, and what we are considering. This page exists so you can see where [Schemity](https://schemity.com) is heading - and influence it: if something in "Considering" matters to you, [open a feature request](https://github.com/tbson/schemity-feedback/issues/new?assignees=&labels=enhancement&template=feature_request.md&title=%5BFEATURE%5D+) or vote with a 👍 on the existing issue.

## Recently shipped

- **DBML import and export** - move schemas to and from dbdiagram.io and the wider DBML toolchain in one step
- **Re-sync** - reopening a reverse-engineered diagram pulls the latest schema from the live database; existing entities keep their layout
- **Draft entities** - entities not yet confirmed by the database render with a dashed border, so designs-in-progress are distinguishable from real tables
- **Merge-aware copy/paste between diagrams** - pasting an entity that already exists in the target transfers only its position, size, and color, so layouts move between diagrams safely
- **Local AI via Ollama** - the diagram-editing AI chat against models on your own machine, zero network egress
- **Relationship line clarity** - line hops where lines cross, rounded waypoint corners, and click-to-highlight for both connected fields

## Building now

<!-- TODO: fill in what is actively in development -->

- ...

## Considering

<!-- TODO: fill in ideas under consideration - link related feedback issues -->

- ...

---

This roadmap is directional, not a commitment - priorities shift based on the feedback in this repository. Release details land in the [changelog](CHANGELOG.md) when features ship.
