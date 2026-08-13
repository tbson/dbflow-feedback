# Schemity Feedback and Community

Welcome to the official community feedback repository for [Schemity](https://schemity.com) - the **offline desktop ERD tool for software engineers**. This is the best place to report bugs, request new features, and provide feedback to help us improve the application.

## How to Contribute

We use GitHub Issues to track all feedback. This is the primary way to reach the development team.

- **Report a bug:** [create a new bug report](https://github.com/tbson/schemity-feedback/issues/new?assignees=&labels=bug&template=bug_report.md&title=%5BBUG%5D+)
- **Request a feature:** [submit a feature request](https://github.com/tbson/schemity-feedback/issues/new?assignees=&labels=enhancement&template=feature_request.md&title=%5BFEATURE%5D+)
- **General feedback and questions:** [open a new issue](https://github.com/tbson/schemity-feedback/issues/new) or join the [Discord community](https://discord.gg/DVgaXN52ex)

Before creating a new issue, please search the existing issues to see if your feedback has already been reported.

Also in this repository:

- **[Roadmap](ROADMAP.md)** - what recently shipped, what we are building, and what your feedback can influence
- **[Changelog](CHANGELOG.md)** - release notes for every version
- **[Example schemas](examples/)** - importable sample workspaces, in Schemity's plain-JSON format plus DBML and SQL

## What is Schemity?

**Schemity** is an offline ERD tool for software engineers who work with relational databases. Design your data model visually, create relationships by drag and drop, reverse engineer a live database into a diagram, and generate SQL migrations for review - in a lightweight native desktop app that works fully offline. Your diagrams are plain JSON files on your own machine, so this is a Git-native ERD tool by design: commit the ERD next to your code and review schema changes in pull requests.

Supported databases: **PostgreSQL, Supabase, MySQL, MariaDB, SQL Server, and SQLite**. Runs on macOS, Windows, and Linux.

- Website: [schemity.com](https://schemity.com)
- Documentation: [schemity.com/doc](https://schemity.com/doc)
- Blog: [schemity.com/blog](https://schemity.com/blog)
- FAQ: [schemity.com/faq](https://schemity.com/faq)
- Free online version (design and share only): [lite.schemity.com](https://lite.schemity.com)

## What makes Schemity different?

**Offline by architecture, not by feature flag.**
No cloud, no account, no server component. A local ERD tool that works on a plane, behind a VPN, or fully air-gapped - which also makes it an ERD tool that IT departments approve without a security review, and a safe choice for NDA projects and client work.

**Context views: one schema, many perspectives.**
Break a large schema into focused sub-diagrams - auth, billing, analytics - while the main diagram stays the single source of truth. Context views are read-only by design, an orange dot marks entities with relationships outside the view, and you can create as many views as the schema deserves. This is how a 300-table database becomes diagrams people actually read. Then zoom out further with the Context Map: every context becomes a single node with dependency arrows between them, so the architecture itself is a diagram too.

**Git-native, not cloud-locked.**
Every diagram is a plain JSON file in a workspace folder you choose. Commit it, diff it, review it in pull requests, and let `git log` be your schema's history. No vendor between you and your own work.

**Migrations that respect production.**
Change the ERD and Schemity generates the exact SQL migration diff for review - nothing runs against the connected database until you explicitly apply it. Reverse engineer an existing database into an ERD, and re-sync keeps the diagram current as the schema evolves: existing entities keep their layout, dropped ones disappear, new ones appear ready to place.

**AI on your own key - or no cloud at all.**
A built-in AI chat with bring-your-own-key (BYOK) support for OpenAI, Claude, Gemini, Grok, and DeepSeek: describe a subsystem and it creates entities and relationships on the canvas. Prompts go directly from your desktop to your chosen provider - no middleman server. Where no cloud is allowed, add Ollama and run the same chat against local models with zero egress.

**Built for how engineers actually work.**
Keyboard-first editing down to Vim-style navigation, entity templates so every table starts from your conventions, auto junction tables for N:N relationships, convention-aware field placement, and copy/paste between diagrams. It feels like a code editor, not a drawing tool.

**Constraints without the guesswork.**
Check constraints, composite unique constraints, indexes, defaults, and not-null rules are part of the visual design - displayed as badges directly on the entities, not buried in migration files. Clear crow's foot notation describes the full meaning of every relationship.

**A schema linter that reads facts, not verdicts.**
Seventeen classes of schema problem, checked entirely offline and reported on the diagram itself - a strip in the margin marking the entity and the field row concerned, not a list you have to translate back into the picture. Findings are grouped by what they actually cost you rather than scored on a severity scale, and per-diagram ignores and rule switches are saved in the file, so the conventions your team agreed on get reviewed in Git like the rest of the schema.

**Lightweight. No Electron. No JVM.**
Built with a native WebView and Rust. Fast to download, instant to launch - a lightweight ERD tool, not a database IDE.

## Who is Schemity for?

Schemity is for any software engineer who works with relational databases:

- **Starting a new project** - model your domain visually before writing a single migration
- **Joining an existing codebase** - reverse engineer PostgreSQL (or any supported database) into an ERD and understand the schema fast
- **Documenting production** - connect through an SSH tunnel with credentials in the OS keychain, read the schema, and share the diagram read-only without sharing database access
- **Evolving a growing schema** - design changes visually, generate precise migrations, keep the ERD in sync with re-sync
- **Consulting and client work** - one workspace folder per client, physically isolated, handed over or deleted as a folder

## How does Schemity compare to other ERD tools?

Honest, detailed comparisons with the tools people usually evaluate alongside Schemity:

- [Schemity vs ChartDB](https://schemity.com/blog/schemity-vs-chartdb) - the offline desktop alternative to the cloud schema visualizer
- [Schemity vs dbdiagram.io](https://schemity.com/blog/schemity-vs-dbdiagram-io) - visual canvas and offline files instead of a DSL in the browser
- [Schemity vs DbSchema](https://schemity.com/blog/schemity-vs-dbschema) - the lightweight alternative to a 100+ engine database IDE

## Feature List

### Workspaces & Storage
- Multiple workspaces, each a folder on disk; every diagram is a plain local JSON file you can version control with Git
- Manage multiple database connections and diagrams per workspace; reorder by drag and drop
- Import an existing workspace from anywhere on your machine; open the workspace folder in the native file manager with one click
- Workspaces are marked in the list for what they are: a Git branch icon when the folder sits inside a Git repository (at any depth), and its own icon when the workspace was imported from outside the default `~/schemity` folder - the two markers are independent and stack
- Open diagrams in read-only mode - explore a diagram created from a connection without access to that connection

### Database Connections
- PostgreSQL, Supabase, MySQL, MariaDB, SQL Server, and SQLite; multi-schema support on PostgreSQL
- Direct or SSH tunnel connections; test a connection before saving it
- Passwords stored in the OS keychain, never in plain text; SSH keys stored by reference only
- Environment tags (Local, Staging, Production) keep the connection list honest

### Reverse Engineering & Re-sync
- Design from scratch, or connect to a real database and reverse engineer its schema into an ERD
- Re-sync keeps a reverse-engineered ERD current: reopening the diagram pulls the latest schema, existing entities keep their layout, and a Reset ERD action does the same on demand
- Import SQL (CREATE TABLE statements or a full dump) to generate entities and relationships automatically
- Import and export DBML, so schemas move to and from dbdiagram.io and the wider DBML toolchain in one step
- Database views and materialized views are introspected on every supported dialect and shown as read-only entities - italic name, a view or mview label in the footer - and excluded from migration, DBML, and Mermaid output
- A diagram stays editable when its database is unreachable: moving or recoloring an entity still saves, and re-sync reports the unreachable database instead of prompting, leaving your work in progress and its undo history untouched

### Diagram Design
- Dark mode and light mode; entities auto-resize to fit content; smart snapping and marquee multi-select
- Legends group related entities visually - drag, resize, rename, recolor; lock a legend to move its entities with it; right-click a legend to export the SQL of everything inside it
- Two predefined layouts (alphabetical or relationship-based) plus Reset Layout
- Realtime fuzzy search across entity, field, and legend names - type any fragment and jump straight to the match, with each result prefixed by what it is and ordered by match quality
- A toggleable minimap shows the whole diagram in miniature with a rectangle marking your viewport: click to jump, drag to pan. Each view carries its own, and entities not yet confirmed by the database are the one thing drawn in color
- An empty canvas points at the way in - right-click and the create-entity shortcut on the main view, import on a context view - and the hint never appears in exports
- Legends and entities support markdown descriptions - a small triangle in the top-right corner opens the rendered description in a modal; context views carry their own, opened from the context view list
- Export diagrams and context views as JPG, PNG, or SVG, export the full SQL, or export a Mermaid erDiagram that renders natively on GitHub, GitLab, Notion, and Obsidian
- SVG exports are true vector documents, not a screenshot wearing an .svg extension: shapes are real shapes grouped per entity and names stay live text, so a diagram opens as editable artwork in Figma, Affinity Designer, Illustrator, or Inkscape

### Context Views
- Focused sub-diagrams of the main ERD: import just the entities for one subject area and arrange them freely
- Read-only by design - the main view stays the single source of truth
- An orange dot marks entities with relationships outside the view, so a focused diagram never hides that it is incomplete
- Right-click a legend and choose "Import to context views" - a legend drawn around a domain becomes a context view in one step, without adding entities one by one

### Context Map
- A bird's-eye view that renders each context view as a single node, with arrows for the dependencies between contexts and a badge showing how many foreign keys flow in each direction
- Arrow shape encodes dependency health: a straight arrow is a one-way dependency, a curved arrow means two contexts depend on each other - circular dependencies stand out at a glance
- Click a context to highlight all of its dependency arrows; double-click an arrow to see every underlying foreign key behind it
- Each context's color carries over to its node and outgoing arrows, and fuzzy search focuses any context instantly, even on a busy map
- Export the Context Map as JPG, PNG, or SVG, or as a Mermaid diagram

### Fields & Constraints
- Distinct icons for primary keys, foreign keys, and regular fields; nullable and unique badges and default values shown directly on the entity
- Check constraints, composite unique constraints, and indexes managed visually; fields covered by an IN check constraint are underlined, with their allowed values one keystroke away
- Convention-aware placement: new fields land above timestamp fields, new foreign keys below the primary key
- Entity templates pre-populate every new table with the fields your team always adds
- Array type support for PostgreSQL; smart default values picked from special values or check constraints

### Relationships & Foreign Keys
- Create foreign keys by dragging a field to another entity - 1:N, 1:1, and N:N with auto-generated junction tables; self-referencing keys supported
- Clear crow's foot notation with configurable cardinality, ON DELETE, and ON UPDATE
- Relationships with ON DELETE CASCADE are drawn with a bold crow's foot at the child end, so cascading deletes are visible on the canvas without opening any dialog
- Entity colors carry to relationship lines; click a relationship to highlight it together with both connected fields
- Custom waypoints with rounded corners, line hops where lines cross, and double-click gestures to reshape or reset a line
- Foreign key naming convention (snake_case or camelCase) configurable per connection

### Migrations
- Change the ERD and Schemity generates the SQL migration diff for review; it runs against the connected database only when you explicitly apply it
- Dashed borders distinguish draft entities that do not exist in the database yet
- Exported SQL creates tables with their constraints inline - primary keys, unique and check constraints, and foreign keys inside CREATE TABLE, emitted in dependency order, with ALTER statements only where a deferred foreign key needs one

### Schema Lint
- Seventeen classes of schema problem, checked offline against the diagram and reported on the diagram itself: a colored strip in the margin marks the exact entity and the exact field row, with a count beside entities carrying more than one
- Findings are grouped by consequence - fails at runtime, constraint unenforced, permanent cost, convention worth confirming - rather than graded on a severity scale, and each one jumps to its entity on the canvas or opens the dialog that resolves it
- It knows a correct link table from a broken one: a composite primary key over the foreign key pair and a surrogate id plus a unique constraint on that pair are both accepted, while a multi-column unique containing a nullable column - which enforces nothing, because NULLs compare as distinct - is caught
- A live count badge on the Lint button, colored by the most serious finding, works whether or not the mode is open
- Per-finding ignores and per-rule switches are saved in the diagram file, so the conventions your team agreed on travel with the schema and get reviewed in Git

### AI Assistant
- Built-in AI chat with BYOK support: OpenAI, Claude, Gemini, Grok, DeepSeek - requests go directly from your desktop to your provider
- Ollama support for local models: the same diagram-editing chat with zero network egress
- The chat interacts with the diagram itself - it creates and modifies entities and relationships, with full undo
- It works with the Context Map too: ask it to re-arrange the contexts, or to analyze the map for circular dependencies - including indirect cycles spanning several contexts that no visual scan can reveal

### Keyboard & Productivity
- Shortcuts for nearly every action; Vim-style navigation (h/j/k/l) across entities and fields
- Multiple tabs with isolated undo/redo history; switch with Cmd/Ctrl + 1-9
- Copy/paste entities and fields between diagrams - pasting onto an existing entity transfers layout and color only, so arrangements move between diagrams safely
- Move entities by keyboard in 1 px or 10 px steps

## Pricing

**$129 one-time** - a one-time purchase ERD tool, not a subscription. Includes 1 year of updates; $69/year to keep receiving updates after that, and the app keeps working forever even if you never renew.

**Free for education** (email support@schemity.com with your .edu address) and a **2-week full trial** for everyone - the app stays usable for visual design after the trial ends, while licensed features such as the minimap and schema lint need an active licence. Details on the [pricing page](https://schemity.com/pricing).

---

👉 [Download Schemity at schemity.com](https://schemity.com/#platforms)

## What to Expect

We do our best to respond to new issues in a timely manner. Please provide as much detail as possible when creating an issue - it helps us understand and address your feedback effectively.

Thank you for helping us make Schemity better!
