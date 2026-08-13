# Schemity Changelog

Release notes for [Schemity](https://schemity.com), the offline desktop ERD tool. Newest first. Download the latest version at [schemity.com](https://schemity.com/#platforms).

## v2.9.2 - 2026-08-13
- ✨ Schema lint: a mode that checks the diagram against seventeen classes of schema problem and shows every finding on the diagram itself - a colored strip in the margin marks the exact entity and the exact field row concerned, with a count beside entities carrying more than one. Findings are grouped by consequence (fails at runtime, constraint unenforced, permanent cost, convention worth confirming) rather than graded on a severity scale, and each one can jump to its entity on the canvas or open the dialog that resolves it.
- ✨ The lint knows the difference between a link table that is correct and one that is not: a junction table with a composite primary key over its foreign key pair, and one with a surrogate id plus a unique constraint on that pair, are both accepted, so neither keying convention is reported as a mistake. It also catches the constraint that looks right in a schema dump and enforces nothing - a multi-column unique containing a nullable column, where NULLs compare as distinct and the same combination can repeat without limit.
- ✨ A count badge on the Lint button stays live whether or not the mode is open, colored by the most serious finding, so a schema carrying only naming conventions stays quiet while one that will fail at runtime does not.
- ✨ Lint findings can be ignored one at a time, and individual rules switched off, per diagram. Both are saved in the diagram file, so the conventions a team agreed on travel with the schema and get reviewed in Git instead of being re-dismissed by everyone who opens it.
- ✨ A diagram stays editable when its database is unreachable: recoloring or moving an entity still saves, because the migration diff is computed from the ERD and the schema already on hand rather than by querying the database. Re-sync checks the connection before asking anything and reports an unreachable database instead of prompting, leaving the diagram and its full undo history untouched.
- ✨ Fuzzy search now finds legends alongside entities and fields. Every row is prefixed with what it is ([E] entity, [F] field, [L] legend), results are ordered by match quality first so equally good hits never interleave, and selecting a legend selects it on the canvas and centres the viewport on it. Each view searches its own legends.
- ✨ An empty canvas now tells you the way in: the main view points at right-click and the create-entity shortcut, while a context view points at import instead. The hint is click-through, so a right-click on the text still opens the very menu it teaches, and it never appears in exports.
- 🔧 The minimap and schema lint require a licence. F2 toggles Lint mode and F8 toggles the minimap.
- 🔧 Shortcut keys in the help dialog render in upper case, joined to their modifier, without brace markers.
- 🔧 A new context view now defaults to a color from the entity palette.
- 🔧 The canvas no longer re-rasterizes in full after every entity drag.
- 🔧 Upgraded dependencies, including @dbml/core to 10.0.0, antd to 6.6.0, and posthog-js.

## v2.9.1 - 2026-08-08
- 🔧 The ERD visual system was rebuilt on measured, theme-aware tokens, so entities, badges, and relationship lines stay consistent across light and dark mode.
- 🔧 The canvas grid was removed.
- 🔧 The Context Map dependency drawer now opens on a single click.
- 🔧 The connection manager uses consistent icons and button weights.
- 🔧 Upgraded dependencies.

## v2.9.0 - 2026-08-05
- ✨ Minimap: a toggleable map at the bottom right of the canvas shows the shape of the whole diagram at a glance - every legend and entity in miniature, plus a rectangle marking the part the viewport covers. Click anywhere on it to jump straight there, or drag the rectangle to pan continuously, so inspecting a distant corner no longer means zooming all the way out and back in. The main view and each context view carry their own minimap. Toggle it from the footer or with Ctrl/Cmd + Shift + M.
- ✨ The minimap draws only legends and entities, never relationships, and ignores custom colors: it is a map of where things are, not a shrunken second copy of the diagram, so it stays readable no matter how dense the ERD is. Color is used in exactly one place - entities not yet confirmed by the database are drawn in orange, so tables pulled in by a re-sync, or entities imported into a context view, are easy to find on a large canvas.
- ✨ Database views and materialized views are introspected on every supported dialect and shown as read-only entities: an italic name and a view or mview label in the entity footer set them apart from base tables. Their schema cannot be edited, new relations cannot be dropped onto them, and they are excluded from migration, DBML, and Mermaid output. SQL Server indexed views are detected as materialized, with their indexes shown.
- ✨ Workspaces whose folder sits inside a Git repository show a Git branch icon next to their name, so which of your diagrams are actually under version control is visible at a glance. Detection walks up the directory tree, so the icon appears whether the workspace folder is the repository root itself or a subfolder nested anywhere inside a larger Git project.
- 🔧 SQL export now creates tables with their constraints inline: primary keys, unique constraints, check constraints, and foreign keys are written into CREATE TABLE on PostgreSQL, MySQL, and SQL Server, with statements emitted in dependency order and ALTER statements only where a deferred foreign key needs one.
- 🔧 The entity description marker now appears as soon as the description changes.
- 🔧 The legend drawer's submit button moved to the footer, and its Cancel button was dropped.
- 🔧 The export watermark is smaller and tucked into the corner.
- 🐛 The inline rename overlay stays attached to its entity while the canvas pans.
- 🐛 Dot-prefixed folders no longer appear in the workspace list.
- 🐛 MySQL: migration foreign key additions are now ordered after new-table creation.

## v2.8.1 - 2026-08-01
- ✨ ON DELETE CASCADE made visible: relationships whose foreign key cascades on delete are drawn with a bold crow's foot at the child end, on both the canvas and SVG export. The signal is deliberately weight, not color - a cascade is a design decision that deserves attention, not an error to fix.
- ✨ The field form now offers a `<NULL>` default value for nullable fields covered by a check constraint or enum-like values.
- 🔧 Upgraded dependencies, including the DBML library and Vite, with fixes to keep DBML import/export working exactly as before.
- 🐛 MySQL: string DEFAULT values are now quoted correctly for types outside the allowlist.
- 🐛 Fixed license removal handling.

## v2.8.0 - 2026-07-20
- ✨ Context Map: a bird's-eye view that renders each context view as a single node and draws arrows for the dependencies between contexts, with a badge on each arrow showing how many foreign keys flow in that direction. Click a context to highlight all of its dependency arrows; double-click an arrow to see every underlying foreign key behind it (source field, target entity, and constraint name).
- ✨ Arrow shape encodes dependency health: a straight arrow is a one-way dependency, a curved arrow means the two contexts depend on each other - so circular dependencies stand out at a glance. Every arrow starts with a circle at its source end, so where a line begins is never ambiguous.
- ✨ Each context's color carries over to its Context Map node and outgoing arrows. Fuzzy search focuses any context instantly, and the footer shows how many contexts and dependencies the map contains. Export the map as JPG, PNG, or SVG, or as a Mermaid diagram.
- ✨ The AI chat works with the Context Map: ask it to re-arrange the contexts, or to analyze the map for circular dependencies - including indirect cycles that span several contexts (A -> C -> B -> A), which no visual scan of pairwise arrows can reveal.
- ✨ Right-click a legend and choose "Import to context views" to bring every entity inside that legend into a context view in one step - a legend drawn around a domain becomes a context view without adding entities one by one.
- ✨ Export a diagram as a Mermaid erDiagram file, so the schema renders natively as a diagram on Mermaid-enabled platforms such as GitHub, GitLab, Notion, and Obsidian.

## v2.7.0 - 2026-07-17
- ✨ Markdown descriptions for entities and legends: write a description in a markdown editor with write/preview tabs. When the description is not empty, a note icon appears at the bottom-right corner of the entity or legend - on the canvas and in SVG exports - and clicking it opens the rendered description in a modal.
- ✨ Context view descriptions: each context view can carry its own markdown description, opened from a file icon in the context view list.
- 🔧 Entity descriptions are pure ERD data: they survive database re-introspection and re-sync.
- 🐛 The description modal now stacks above the context view drawer, and drawer keyboard shortcuts are guarded while modals are open.
- 🐛 Auto-resize and other control button clicks no longer activate the enclosing legend.

## v2.6.0 - 2026-07-15
- ✨ New entities stand out: entities created since your last save show a dashed outline, so you can spot unsaved work at a glance. The outline clears when you save and never appears in exported images, SVGs, or thumbnails. Pasted, duplicated, and imported entities are marked too.
- ✨ Sync layout across connections: copy entities in one connection and paste them into another. Entities that already exist there (same name) get their position, size, and color updated in place (schema untouched, relations re-route automatically), and missing ones are created at the exact copied coordinates. Pasting within the same connection still duplicates as before.
- ✨ Context view: right-click the empty canvas to import entities into the current context view, same as the Cmd+Shift+I shortcut.
- 🔧 Cleaner toolbar: the SQL/DBML import button now uses a proper import icon, the export button is simply labeled "Export" with a new icon (it exports images, SQL, or DBML), and the redundant import-entity button was removed (the right-click menu and shortcut cover it).
- 🔧 Template manager is disabled while a context view is active: templates only apply to the main view.
- 🔧 The monthly license tier has been retired; existing licenses are unaffected.
- 🐛 Fixed a "Maximum call stack size exceeded" console error storm when the entity editor and the SQL dialog were open at the same time.
- 🐛 A quick click followed by a right-click no longer counts as a double-click, so the context menu no longer accidentally opens the entity editor.

## v2.5.1 - 2026-07-13
- ✨ Import DBML: paste or upload a .dbml file (the dbdiagram.io format), preview the parsed tables, and import them into your diagram. Types are translated to your connection's database dialect automatically (for example, varchar becomes NVARCHAR(255) on SQL Server and TEXT on SQLite), enums and refs come along, and parse errors show the exact line. Shortcut: Cmd+Shift+D.
- ✨ Export DBML: export the whole diagram as a .dbml file (tables, columns, enums, indexes, and refs) from the export drawer, ready to open in dbdiagram.io or any DBML tool.
- 🔧 Exported DBML uses generic types (varchar, integer, timestamp) so files open cleanly in dbdiagram.io and other DBML tools; re-importing them into the same connection restores the exact dialect types.
- 🔧 One Import button: Import SQL and Import DBML now live in a single toolbar dropdown, with their shortcuts (Cmd+Shift+I / Cmd+Shift+D) shown right in the menu.
- 🔧 Imports stay in memory until you save: importing SQL or DBML no longer writes to disk immediately. Undo it, or reload to get back the last saved state; press Save to keep it.
- 🔧 The Parse button now shows its loading spinner while large SQL or DBML files are being processed, instead of briefly freezing with no feedback.
- 🔧 Rearranged the toolbar menu.
- 🐛 PostgreSQL: the field dialog no longer forces a Length for VARCHAR, CHARACTER, and BIT (bare varchar is valid in Postgres and means unlimited).
- 🐛 Lite: the app now reloads itself once when a new deploy invalidates already-open lazy route chunks, instead of failing to navigate.
- 🐛 "Edit entity" in the context menu opened the add field dialog instead of the entity editor.
- 🐛 The first click outside an entity after dragging it now deselects it as expected.

## v2.5.0 - 2026-07-13
- ✨ Import DBML: paste or upload a .dbml file (the dbdiagram.io format), preview the parsed tables, and import them into your diagram. Types are translated to your connection's database dialect automatically (for example, varchar becomes NVARCHAR(255) on SQL Server and TEXT on SQLite), enums and refs come along, and parse errors show the exact line. Shortcut: Cmd+Shift+D.
- ✨ Export DBML: export the whole diagram as a .dbml file (tables, columns, enums, indexes, and refs) from the export drawer, ready to open in dbdiagram.io or any DBML tool.
- 🔧 Exported DBML uses generic types (varchar, integer, timestamp) so files open cleanly in dbdiagram.io and other DBML tools; re-importing them into the same connection restores the exact dialect types.
- 🔧 One Import button: Import SQL and Import DBML now live in a single toolbar dropdown, with their shortcuts (Cmd+Shift+I / Cmd+Shift+D) shown right in the menu.
- 🔧 Imports stay in memory until you save: importing SQL or DBML no longer writes to disk immediately. Undo it, or reload to get back the last saved state; press Save to keep it.
- 🔧 The Parse button now shows its loading spinner while large SQL or DBML files are being processed, instead of briefly freezing with no feedback.
- 🔧 Rearranged the toolbar menu.
- 🐛 PostgreSQL: the field dialog no longer forces a Length for VARCHAR, CHARACTER, and BIT (bare varchar is valid in Postgres and means unlimited).
- 🐛 Lite: the app now reloads itself once when a new deploy invalidates already-open lazy route chunks, instead of failing to navigate.
- 🐛 "Edit entity" in the context menu opened the add field dialog instead of the entity editor.
- 🐛 The first click outside an entity after dragging it now deselects it as expected.

## v2.4.0 - 2026-07-10
- ✨ Ollama support: chat with local AI models by adding Ollama as a provider (with a configurable base URL), including model listing and live streaming responses.
- ✨ Line jumps: where relation lines cross, one line now hops over the other with a small arc, so you can always tell which line goes where. Works on canvas and SVG export.
- ✨ Rounded relation corners: relation lines now turn with smooth rounded corners at their waypoints, matching the entity and legend corner style. Works on canvas and SVG export.
- 🔧 Cleaner relation lines: waypoint dots now appear only while a relation is selected, and are no longer drawn in SVG exports. Dragging and removing waypoints works as before.
- 🔧 Upgraded dependencies.

## v2.3.1 - 2026-07-07
- ✨ AI chat: explanations now stream in live as they are generated, instead of appearing all at once.
- 🐛 Fixed unique index "U" markers not appearing until reload: they now show immediately after creating a unique index.
- 🐛 The entity footer uniqueness summary now counts unique indexes correctly.
- 🐛 Hardened AI chat streaming, message dispatch, and model handling for more reliable responses.
- 🐛 Removed a stray blue focus border on the AI chat input.

## v2.3.0 - 2026-07-01
- ✨ [Lite] Embed mode: share public diagrams anywhere with a chromeless viewer, oEmbed support for rich previews, and an embedded footer that links back to the full diagram with an attribution badge.
- ✨ View SQL for a whole legend: right-click a legend (or press Ctrl+Shift+S with it selected) to see the combined SQL for every entity inside it.
- ✨ Custom colors for indexes, including a "U" marker for unique indexes.
- 🔧 [Lite] Public links now open in a new tab instead of being copied to the clipboard.
- 🔧 Ctrl+Shift+L now edits the selected legend (and still creates one when none is selected).
- 🔧 Diagrams open with no entity pre-selected, so the view starts clean while arrow-key navigation still begins on the first entity.
- 🔧 Updated dependencies.
- 🐛 Fixed migration generation failing when a field's original name was missing.
- 🐛 Reduced Sentry noise: dropped transport-level fetch failures and fixed a spurious max-change-limit error.

## v2.2.0 - 2026-06-22
- ✨ SQLite support: connect to SQLite databases with live schema introspection, migration generation and apply, SQL import, and offline DDL generation.
- 🔧 Unique indexes now show as "unique together" on the diagram, alongside unique constraints, so you can see uniqueness whatever its source. The index still stays in the index list.
- 🔧 Switching between open diagram tabs is smoother: a brief loading indicator shows instead of a frozen pause on large diagrams.
- 🐛 Fixed several issues when multiple diagrams are open at once: dragging an entity to the viewport edge to auto-scroll, snap-to-guide, window resize, legend snapping, and new entity/legend placement now all act on the diagram you are viewing.
- 🐛 Reset ERD now re-reads the live database, so tables or columns created by other tools show up (it could previously show a cached schema).
- 🐛 Closing a diagram tab only warns about unsaved changes when that tab actually has them, instead of when a different tab is unsaved.
- 🐛 Right-clicking a connection and picking a menu item (Edit, Duplicate, and so on) no longer also opens the diagram.
- 🐛 Fixed a crash on some older browsers caused by performance autocapture.

## v2.1.0 - 2026-06-20
- ✨ New users get a ready-made "Sample Blog Diagram" on first launch, so you can explore the editor right away. Delete it anytime and it will not come back.
- ✨ Duplicate entities from the right-click menu or with Ctrl/Cmd+Shift+D.
- ✨ Copy, paste, and duplicate now keep foreign keys, including self-referential ones, instead of dropping them.
- ✨ Open a diagram with a single click in the connection list.
- ✨ Import SQL is now available on every tier.
- 🔧 Faster, smoother entity selection and deselection on large diagrams.
- 🔧 The Lite home banner is now a compact single line with a "More" link.
- 🔧 Restored keyboard shortcuts in the Lite (web) version.
- 🔧 Added anonymous, privacy-preserving error monitoring and product analytics (no personal data; disclosed in the privacy policy).
- 🔧 Updated dependencies.
- 🐛 Long entity header names are now truncated and centered instead of overflowing.
- 🐛 Copying an entity no longer applies the wrong field type icon.
- 🐛 Auto-resize now fits the full default value width.
- 🐛 Pasting a self-relation keeps its original line geometry.

## v2.0.3 - 2026-06-17
- ✨ Schemity Lite: a free, browser-based version at lite.schemity.com, so you can design ERDs online with no install.
- ✨ Cloud sharing: sign in with Google to save your diagrams to the cloud and share them with public links that show a preview when posted.
- ✨ Public viewer: open a shared diagram full-screen and duplicate it into your own workspace in one click.
- ✨ Import existing workspace folders, and give workspaces custom names shown across the app.
- 🔧 Expired trials now relax into a usable offline tier, so you can keep designing schemas, with premium features clearly marked.
- 🔧 Cloud diagrams are encrypted at rest.
- 🐛 Database connection failures now show the real error instead of a masked timeout.
- 🐛 Fixed inconsistent first-entity selection between initial focus and keyboard navigation.
- 🐛 Smoother relation dragging on large diagrams.

## v2.0.2 - 2026-06-17
- ✨ Schemity Lite: a free, browser-based version at lite.schemity.com, so you can design ERDs online with no install.
- ✨ Cloud sharing: sign in with Google to save your diagrams to the cloud and share them with public links that show a preview when posted.
- ✨ Public viewer: open a shared diagram full-screen and duplicate it into your own workspace in one click.
- ✨ Import existing workspace folders, and give workspaces custom names shown across the app.
- 🔧 Expired trials now relax into a usable offline tier, so you can keep designing schemas, with premium features clearly marked.
- 🔧 Cloud diagrams are encrypted at rest.
- 🐛 Database connection failures now show the real error instead of a masked timeout.
- 🐛 Fixed inconsistent first-entity selection between initial focus and keyboard navigation.
- 🐛 Smoother relation dragging on large diagrams.

## v2.0.1 - 2026-06-17
- ✨ Schemity Lite: a free, browser-based version at lite.schemity.com, so you can design ERDs online with no install.
- ✨ Cloud sharing: sign in with Google to save your diagrams to the cloud and share them with public links that show a preview when posted.
- ✨ Public viewer: open a shared diagram full-screen and duplicate it into your own workspace in one click.
- ✨ Import existing workspace folders, and give workspaces custom names shown across the app.
- 🔧 Expired trials now relax into a usable offline tier, so you can keep designing schemas, with premium features clearly marked.
- 🔧 Cloud diagrams are encrypted at rest.
- 🐛 Database connection failures now show the real error instead of a masked timeout.
- 🐛 Fixed inconsistent first-entity selection between initial focus and keyboard navigation.
- 🐛 Smoother relation dragging on large diagrams.

## v2.0.0 - 2026-06-17
- ✨ Schemity Lite: a free, browser-based version at lite.schemity.com, so you can design ERDs online with no install.
- ✨ Cloud sharing: sign in with Google to save your diagrams to the cloud and share them with public links that show a preview when posted.
- ✨ Public viewer: open a shared diagram full-screen and duplicate it into your own workspace in one click.
- ✨ Import existing workspace folders, and give workspaces custom names shown across the app.
- 🔧 Expired trials now relax into a usable offline tier, so you can keep designing schemas, with premium features clearly marked.
- 🔧 Cloud diagrams are encrypted at rest.
- 🐛 Database connection failures now show the real error instead of a masked timeout.
- 🐛 Fixed inconsistent first-entity selection between initial focus and keyboard navigation.
- 🐛 Smoother relation dragging on large diagrams.

## v1.9.0 - 2026-06-05
- ✨ Added a frontier indicator: in a context view, entities with at least one relation to an entity outside the view show a small theme-aware circle on the right of their footer.
- ✨ Enabled editing `ON DELETE` and `ON UPDATE` actions on existing relations.
- 🔧 Switching to a large context view now shows animated loading dots on the picked row so you can tell the app is working, not frozen.
- 🐛 Fixed arrow/hjkl keyboard navigation in context views — selection movement, Ctrl+arrow nudges, Ctrl+A and Esc-to-legend all now operate on the visible entity set instead of the full main schema.
- 🐛 Fixed broken relation lines when exporting selected entities into a context view: waypoints now translate with the entities when both endpoints land in the batch, and fall back to a clean two-point attach otherwise.

## v1.8.6 - 2026-06-03
- 🔧 Promoted JSONB into the "most used" types list and allowed an empty default for JSON/JSONB columns.
- 🔧 Added the Ctrl/Cmd+Enter shortcut hint to the "Update cardinality" item in the relation context menu.
- 🐛 Cardinality dialog now highlights the selected button — the displayed value is normalized to the relType-matching family, and mismatched stored values are repaired on load.
- 🐛 Fixed invalid SQL generated for PostgreSQL migrations: indexes with no explicit method now default to `btree`, and imported tables with unnamed primary keys synthesize `<table>_pkey` so the next migration no longer fails on `CONSTRAINT ""`.

## v1.8.5 - 2026-05-26
- 🔧 Tightened snap-to-guide when dragging relation points in crowded areas so neighbouring relations no longer pull the cursor off course.
- 🔧 Updated dependencies.
- 🐛 Dragging a relation point inside a legend no longer accidentally selects the legend on release.

## v1.8.4 - 2026-05-19
- 🔧 Tightened snap-to-guide when dragging relation points in crowded areas so neighbouring relations no longer pull the cursor off course.
- 🔧 Updated dependencies.
- 🐛 Dragging a relation point inside a legend no longer accidentally selects the legend on release.

## v1.8.3 - 2026-05-13
- ✨ Relation points are now removed when an entity is moved above them.
- 🐛 Straight relations no longer change location when their connected entity is moved.
- 🐛 Fixed inconsistent relation shapes between dragging and finish dragging entity.

## v1.8.2 - 2026-05-07
- ✨ Export schema to SQL.
- ✨ Foreign key ON DELETE and ON UPDATE actions are now configurable.
- 🔧 Check constraints now update automatically when a column's type changes.
- 🔧 License query updated.
- 🐛 Moving entities with hotkeys now moves their legends along with them.
- 🐛 Legends now snap to guides correctly while being resized.

## v1.8.1 - 2026-05-03
- ✨ Monthly subscription payment option.
- 🐛 Workspace and connection table headers no longer show a hand cursor over the empty header area — only the buttons themselves do.

## v1.8.0 - 2026-05-01
- 🔧 Session tabs now display the connection's name (with a `[1]`–`[9]` index prefix on the first nine tabs) instead of its raw ID; the application window title uses the same name for consistency.
- 🔧 Selecting a field inside an entity now raises that entity to the front, matching the behavior of clicking its header or footer.
- 🔧 Relation cardinality (1:1 vs 1:N) now stays in sync with the foreign-key column's UNIQUE constraint — toggling uniqueness updates the rendered marker automatically.
- 🔧 Editing non-critical connection attributes (e.g. name, naming convention) no longer requires re-entering the stored password.
- 🔧 Clearer migration error messages for MySQL.
- 🔧 Internal stability pass: removed redundant `id` fields from entities and fields (now derived from `name`), tightened type discipline around schema-vs-view writes, deduplicated the entity-rename machinery, fixed several listener-attach storms during drag, and added regression coverage for the connection-conversion lifecycle, the new collinear-waypoint cleanup, and the entity-rename invariants.
- 🐛 Renaming an entity no longer emits a destructive `DROP TABLE` + `CREATE TABLE` migration; it now correctly emits `ALTER TABLE … RENAME TO` so existing data is preserved.
- 🐛 Renaming a parent entity now propagates to every child entity's foreign-key target so the generated migration's `REFERENCES` clauses point at the new name instead of the now-nonexistent old one.
- 🐛 Duplicating (or paste-and-renaming) a DB-introspected entity now emits `CREATE TABLE` for the copy instead of treating it as a rename of the original, preventing data corruption on the live source table.
- 🐛 Relation waypoints that disappear mid-drag (when they become collinear with their neighbours) now stay gone after the drag ends — they no longer reappear once the mouse is released.

## v1.7.0 - 2026-04-26
- ✨ Drag to create new relation and drag relation point to scroll the stage.
- ✨ Locked legends automatically move along when all their contained entities are selected and dragged.
- ✨ Marquee selection auto-scrolls when dragging near the viewport edge.
- ✨ Legend dragging auto-scrolls the stage near viewport edges.
- 🔧 Rename Sub Diagram to Context View.

## v1.6.0 - 2026-04-23
- ✨ Inline rename legend by double-clicking the header.
- ✨ Right-click context menu on legend with Edit and Delete actions.
- ✨ Enable context menu for relation in sub-diagram.
- 🔧 Double-click relation to straighten instead of opening cardinality popup.
- 🔧 Underline both field name and default value for option type with check constraint.
- 🔧 Standardize the width of drawer.
- 🔧 Show constraint/index name in edit dialog for readability.
- 🔧 Truncate long constraint/index names in table with tooltip on hover.
- 🐛 Fix relation endpoints disappearing when dragging entities to the opposite side.
- 🐛 Fix relation endpoints disappearing when multi-select dragging entities.
- 🐛 Fix Delete hotkey not working for legend in sub-diagram.
- 🐛 Fix context menu on legend showing wrong menu (New entity/New legend).
- 🐛 Fix check constraint not underlining the field name.
- 🐛 Fix unable to open new tab for another connection.
- 🐛 Fix duplicate connection not duplicating data.
- 🐛 Fix unstable issues when editing sub-diagram.
- 🐛 Fix long startup time caused by frontend timeout.

## v1.5.2 - 2026-04-20
- 🔧 Tune logic for creating N:N table in AI mode.
- 🔧 Rename legend drag mode to lock mode.
- 🔧 Update tooltip configuration.

## v1.5.1 - 2026-04-19
- ✨ Fuzzy search now finds field names in addition to entity names.
- 🐛 Fix search dialog not opening when pressing Ctrl/Cmd+F or clicking the search button.

## v1.5.0 - 2026-04-19
- ✨ AI chat assistant with support for OpenAI, Claude, Gemini, Grok, and DeepSeek.
- ✨ Import SQL to create entities from existing database dumps.
- ✨ N:N junction tables inherit color from active template.
- ✨ Disable PK/Unique for MySQL TEXT and SQL Server NVARCHAR(MAX) fields.
- ✨ Add error boundary for graceful crash recovery.
- 🔧 Optimize loading time for PostgreSQL and SQL Server connections.
- 🔧 Improve migration speed.

## v1.4.4 - 2026-04-15
- 🔧 Refactor structure of Cloudflare Worker.
- 🔧 Improve splash screen experience.
- 🔧 Update version check rule for license validation.

## v1.4.3 - 2026-04-14
- 🔧 Improve license logic and processing of trial period.
- 🐛 Fixed: Control Bar overlaying top part of diagram.

## v1.4.2 - 2026-04-13
- ✨ Add "Reset Layout" button with two layout options: Auto Layout (hierarchical) and Alphabet Layout (alphabetical masonry grid).
- ✨ Auto-scroll to the nearest top-left entity when entering ERD, switching diagrams, or resetting layout.
- ✨ Discard & Switch from main diagram now reverts changes and switches to the target diagram instead of navigating away.
- 🐛 Fixed: relation lines snapping back to original positions after dragging an entity in sub-diagram mode.
- 🐛 Fixed: cardinality not saving properly.

## v1.4.1 - 2026-04-12
- ✨ Update cardinality for relations (1:1 ↔ 0..1, 1:N ↔ 0..* / 1..*) via context menu or double-click.
- ✨ Default state of N part in 1:N relations set to 0..* for more accurate schema representation.
- ✨ Add "Update cardinality" option to relation right-click context menu.
- 🔧 Improved stability and performance: fixed regex recompilation, type safety, pool cache eviction.
- 🐛 Fixed: display wrong zero notation when FK is nullable.
- 🐛 Fixed: update template PK didn't re-calculate other constraints.
- 🐛 Fixed: unhandled promise rejection error on app startup caused by event listener race condition.

## v1.4.0 - 2026-04-11
- ✨ Support camelCase naming convention for FK columns and N:N junction table names, configurable per connection.
- ✨ Newly created N:N junction table is now automatically selected after creation.
- ✨ Inline entity rename now preserves the active/selected state.
- 🐛 Fixed: relations jumped to different attach faces on mouse-up after dragging an entity.
- 🐛 Fixed: collinear middle waypoints were not auto-removed when entities overlapped vertically or horizontally.
- 🐛 Fixed: relation lines changed shape after applying a migration, caused by relation IDs not updating when renaming a new entity.
- 🐛 Fixed: N:N composed PK marked as single PK on frontend side caused unnecessary migrations.
- 🐛 Fixed: loading screen didn't close if user refreshed the app but refused to process the popup.

## v1.3.7 - 2026-04-10
- ✨ Export diagrams as JPEG images.
- ✨ Unsaved sessions now show an asterisk on their tab title so you can tell at a glance which sessions have pending changes.
- 🐛 Fixed: opening multiple database sessions (e.g. PostgreSQL, MySQL, SQL Server) and then creating entities or editing fields in a non-first session would silently write to the wrong session's store.
- 🐛 Fixed: dragging relation waypoints in a non-first session had no effect on that session — changes were applied to the first session instead, corrupting its relations.
- 🐛 Fixed: applying a migration on a remote MySQL server left the loading spinner stuck and the unsaved-changes indicator on permanently.
- 🐛 Fixed: templates were not persisted when saving with a remote database connection, causing them to disappear after a save-and-reload cycle.
- 🐛 Fixed: some MySQL server configurations returned binary-typed metadata from `information_schema`, causing a panic during schema introspection.
- 🐛 Fixed: inconsistent entity right margin when auto-resizing.
- 🔧 Upgraded dependencies.
- 🔒 Fixed some security issues.

## v1.3.6 - 2026-04-09
- ✨ Trial users now get a Schemity watermark in the bottom-right of exported PNG, JPEG, and SVG diagrams.
- ✨ Relation lines now render in front of legend headers, so it's easy to follow a relation that crosses a legend.
- 🐛 Fixed: an entity with two or more self-referencing foreign keys (e.g. `parent_id` and `root_parent_id` both pointing to the same table) only showed one self-relation. All self-relations now fan out down the left side of the entity with matching width.
- 🐛 Fixed: clicking a self-reference highlighted only one of its fields. Both the FK column and the referenced column are now highlighted on the entity.

## v1.3.5 - 2026-04-09
- ✨ New ERD sessions now consistently open at the top-left of the viewport for a predictable starting position.
- ✨ Closing a session or switching diagrams with unsaved work now prompts for confirmation; confirming discards all unsaved changes (memory and disk), cancelling keeps you in place.
- 🧹 Internal code tidy and cleanup.
- 🐛 Fixed snap-to-guide in sub-diagrams snapping against main-diagram coordinates instead of the entities visible in the current sub-diagram.

## v1.3.4 - 2026-04-09
- ✨ Inline rename for entity and field names — double-click an entity header or a field row to edit the name in place. Enter commits, Esc or clicking elsewhere cancels. Field renames reuse the full dialog cascade (FK / CC / PK / UC / IDX / related relations / selection).
- ✨ Larger interactive zones for relation endpoints, with clearer visibility for both relation points and end points so they're easier to grab.
- ✨ Tuned dark-mode colors for resize handles and the new-relation / auto-resize buttons.
- ✨ Use crow's-foot notation more strictly.
- ⬆️ Upgraded dependencies.
- 🐛 Fix default values being displayed in their raw, pre-formatted form in SVG export.

## v1.3.3 - 2026-04-08
- 🐛 Fix the title bar not following the in-app theme — switching between light and dark now updates the native window chrome too.
- 🐛 Fix the auto-created junction table for N:N relationships landing on top of a parent when the two tables are placed diagonally — it now drops below both parents and uses its real height for centering.
- 🐛 Fix entity border glitches around N:N creation and multi-select — the new-relation-target / multi-select highlight now appears uniformly around the entity perimeter, with no thicker rails above the footer and no highlight bleeding onto the body↔footer divider.

## v1.3.2 - 2026-04-07
- ✨ Export to SVG — true vector output (selectable text, infinite zoom) with crow's-foot endpoints, per-entity field clipping, and dark-mode-aware colors.
- ✨ New unified Export drawer — `Ctrl/Cmd + E` (or the toolbar button) opens a drawer to pick PNG or SVG; navigate with ↑/↓ and Enter, or click.
- 🐛 Fix entity widths silently snapping back to 240px on every load — user-resized widths are now preserved down to the resize lower bound.
- 🐛 Fix the ERD jumping to the top-left after saving — the pan position is now preserved across the save/reload cycle, with no intermediate flash.

## v1.3.1 - 2026-04-07
- 🐛 Fix using wrong front for entities.

## v1.3.0 - 2026-04-07
- ✨ Implement multiple diagrams support.
- ✨ Add command palette (Ctrl/Cmd + K or Ctrl/Cmd + Shift + P) to discover and run any action with a single shortcut.
- 🚀 Major performance improvements for large ERDs.
- 🚀 Rewrite the initial entity layout to a force-directed algorithm: connected entities cluster, no overlap, roughly square bounding box.
- 🐛 Fix entity-field highlight not clearing when clicking the background or another entity.
- 🐛 Preserve the active field selection after renaming a field.

## v1.2.11 - 2026-04-03
- 📖 Improve shortcut keys help readability.
- 📦 Update dependencies.
- 🐛 Fix flaws in logic of copy/paste entity/field.

## v1.2.10 - 2026-04-03
- 🖱️ Allow selecting a legend by clicking anywhere on its body, not just the header.
- 📖 Add read-only ERD mode.
- 🔄 Fix schema merge preserving layout after external DB changes.
- ↩️ Fix undo not reverting relation shape changes.

## v1.2.9 - 2026-04-02
- 📐 Windows: Use MSI instead of NSIS.

## v1.2.8 - 2026-04-01
- 📐 Allow user to input connection name instead of connection ID, with ID auto-derived from name.

## v1.2.7 - 2026-04-01
- 📐 Implement snap to guide for legend moving/resizing.
- 📐 Hide all relations of entities in a legend when moving legend.
- 📐 Tune create new relation line to start from the center of the button instead of top right of the source entity.
- 📐 Tune create new relation icon to use crow's foot icon.
- 📐 Update style of new relation button and lock status button.
- 📐 Improve visibility of entity and legend handle squares.
- 📐 Improve consistency of grid display.
- 📐 Tune colors for more consistent display.
- 📐 Reduce plus sign density of grid layer.
- 📐 Ignore grid from export image.
- 🐛 Fix wrong behaviour of auto entity resize.
- 🐛 Fix exported image having light background in dark mode.

## v1.2.6 - 2026-03-31
- 🚀 Offload entity layout computation to Rust for faster initial load on large schemas.
- 🚀 Improve select/deselect/drag performance with fine-grained Zustand selectors and reduced re-render cascades.
- 🚀 Build entity-to-relations index for O(1) lookup, replacing O(N) scans on every drag/move operation.
- 🔍 Implement fuzzy search for entity search and shortcut key help dialogs.
- 📐 Show relations following the entity during single-entity drag instead of hiding them.
- 📐 Split help table into Description and Shortcut columns, sorted alphabetically.
- 🐛 Fix relation endpoints not attaching to entities after drag by clamping endpoints to entity boundaries.
- 🐛 Fix self-relation forming weird shapes during single-entity drag (stale deltaPoints from wrong origin).
- 🐛 Fix entity position gap at drag end by syncing from Konva node's actual final position.

## v1.2.5 - 2026-03-31
- 📐 Hierarchical auto layout on first connect: parent tables at top, children below, with edge-crossing minimization.
- 📐 Visual cluster separation with dynamic column sizing based on database size.
- 🧠 Improve memory usage by merging layers.

## v1.2.4 - 2026-03-31
- 🔍 Navigate search results using arrow keys, Enter to select.
- 🔍 Search always highlights the top result on open and on every query change.
- 🧠 Reduce memory usage.
- 🐛 Fix: Cannot get entity's SQL on ERD-only connection type.
- 🐛 Fix: Missing default schema for connections created before schema support.

## v1.2.3 - 2026-03-30
- 🎨 Re-styling connection table to save space.
- 🐛 Fix Charset and Collation not available for offline MySQL ERDs.

## v1.2.2 - 2026-03-30
- 🐛 Fix modal dialog rendering behind table empty state on Windows.

## v1.2.1 - 2026-03-29
- ✨ Double-click a relation line to straighten it (reset to 2 points).
- ✨ Click a template field row to open edit modal directly.
- ✨ Display field type with length on ERD entity (e.g. NVARCHAR(128) instead of NVARCHAR).
- 🐛 Fix SQL syntax highlighting not working (mono color) in migration and entity SQL dialogs.
- 🐛 Fix entity footer label inconsistency (fields → field).

## v1.2.0 - 2026-03-29
- ✨ Implement dark theme with system/light/dark toggle.
- ✨ SQL Server: use SYSDATETIME() as default value for DATETIME2/DATETIMEOFFSET fields.
- ✨ SQL Server: use NEWSEQUENTIALID() as default value for UNIQUEIDENTIFIER fields, with NEWID() also available.
- ✨ SQL syntax highlighting adapts to dark/light theme.
- ✨ Move displayDefaultValue logic to per-database field rules for db-specific display.
- ✨ Display field type with length on ERD entity (e.g. NVARCHAR(128) instead of NVARCHAR).
- 🐛 Fix SQL Server migration failing when dropping tables referenced by foreign key constraints.
- 🐛 Fix relation colors not updating when toggling dark/light theme.
- 🐛 Fix template check icon always appearing white in dark mode.
- 🐛 Fix theme toggle button being partially unclickable due to z-index overlap with footer bar.

## v1.1.0 - 2026-03-28
- ✨ Dragging a legend now moves all entities inside it (toggle via lock/unlock icon).
- ✨ Legend keyboard movement (Ctrl+Arrow/hjkl) is now consistent with entity movement.
- ✨ Press Enter on an active legend to edit it.
- ✨ Press ESC to navigate: Field → Entity → Legend → Deselect.
- ✨ Add license command.
- ✨ Update style of new version badge.
- 🐛 Fix legend rename not updating its ID, causing "not found" error.
- 🐛 Fix entity rename not updating relation references.
- 🐛 Fix deprecated TypeScript baseUrl warning.

## v1.0.6 - 2026-03-27
- ✨ Tuning UI/UX for connection page.

## v1.0.5 - 2026-03-27
- ✨ Tuning UI/UX for connection page.

## v1.0.4 - 2026-03-27
- ✨ Tuning UI/UX for connection page.

## v1.0.3 - 2026-03-27
- ✨ Tuning UI/UX for connection page.

## v1.0.2 - 2026-03-27
- ✨ Tuning UI/UX for connection page.

## v1.0.1 - 2026-03-27
- ✨ Remove redundant ping operations.

## v1.0.0 - 2026-03-27
- ✨ Implement license check/validate and styling version update.
- ✨ Optimize reduce bundle size.
- 🐛 Fix error when connect SQL Server in tunnel mode.
- 🐛 Fix behaviour of trial expired popup.

## v0.11.1 - 2026-03-27
- ✨ Implement license check/validate and styling version update.
- ✨ Optimize reduce bundle size.
- 🐛 Fix error when connect SQL Server in tunnel mode.
- 🐛 Fix behaviour of trial expired popup.

## v0.11.0 - 2026-03-26
- ✨ Support SQL Server (MSSQL) as a new database type.
- ✨ New connection manager page look.

## v0.10.3 - 2026-03-17
- ✨ Adjust relation width including their endpoints when entity is selected.

## v0.10.2 - 2026-03-15
- ✨ Fast move shortcut: use Ctrl+Shift+Arrow (or Ctrl+Shift+h/j/k/l) to move entities 20px at a time.
- ✨ Check constraint default values are now underlined on the ERD canvas to distinguish them from regular defaults.
- ✨ When a field has check constraint values, the default value dropdown shows only those values.
- ✨ Add margin to exported image.

## v0.10.1 - 2026-03-14
- ✨ Clone to offline ERD: share live ERD diagrams with your team via Git, no database access required.
- ✨ Entity name and legend header font size increased for better readability on large ERDs.
- 🐛 Fixed: right-click on relation line was showing wrong context menu.
- 🐛 Fixed: renaming a connection lost its stored passwords.

## v0.10.0 - 2026-03-14
- ✨ Drag to reorder workspaces and connections, order persisted in meta.json.
- ✨ Default selected workspace is now the top workspace in the list.
- ✨ Auto focus on custom default value field when CUSTOM is selected.
- ✨ Reduced app bundle size from ~13 MB to ~8 MB.

## v0.9.19 - 2026-03-13
- ✨ Allow moving relation point to snap to vertical/horizontal segments of nearby relations.
- ✨ Equal space snap when moving an entity between two nearby entities.
- ✨ Keyboard move step reduced to 1 px for finer control.
- 🐛 Fixed: failed to save new position for 2-point relations after reshaping.
- 🐛 Fixed: equal space snap firing at wrong position.
- 🐛 Fixed: equal space snap interfering with other snap types at large gaps.
- 🐛 Fixed: moving multiple selected entities by keyboard shortcut only moved one.
- 🐛 Fixed: some relations not displayed on first ERD load.

## v0.9.18 - 2026-03-11
- 🐛 Fixed: search input places text near bottom.
- 🐛 Fixed: error when renaming target field of check constraint.
- 🐛 Fixed: cannot create 2 self-relations on one entity.
- 🐛 Fixed: broken self-relation shape when resizing entity.

## v0.9.17 - 2026-03-11
- ✨ Display default value gen_random_uuid() as uuid().
- 🐛 Ensure NOT NULL attribute for primary key.
- 🐛 Fixed: Losing active state after resizing entity or legend.

## v0.9.16 - 2026-03-09
- ✨ Display default value gen_random_uuid() as uuid().
- 🐛 Ensure NOT NULL attribute for primary key.
- 🐛 Fixed: Losing active state after resizing entity or legend.

## v0.9.15 - 2026-03-08
- ✨ Allow to resize entity height to smaller limit.
- 🐛 Fixed: Equal space snap didn't work when moving most top entity.
- 🐛 Fixed: Entity's relations didn't change position when moving entity by hot keys.
- 🐛 Fixed: Legend prevent interacting with relations within their area.
- 🐛 Fix error when renaming field of an unique constraint and can not save legend.

## v0.9.14 - 2026-03-08
- 🐛 Fix small typos.

## v0.9.13 - 2026-03-08
- 🐛 Fix small typos.

## v0.9.12 - 2026-03-07
- 🐛 Fix error when creating N:N table then renaming one of its fields.
- 🐛 Fix connect to DB didn't check schema.
- 🐛 Fix rename field didn't rename FK.
- 🔧 Update logo.

## v0.9.11 - 2026-03-07
- 🐛 Fix error when deleting source table didn't change the foreign key to normal field.
- 🐛 Fix no duplicate global loading when migrating.
- 🐛 Ensure highlight other field after deleting a field.
- 🐛 Fix logic error when creating self reference N:N relationship.
- 🐛 Fix NaN error for default value.
- 🐛 Fix crash when releasing a new relation drag over a missing entity.
- 🐛 Fallback NaN default value to 0 instead of null.

## v0.9.10 - 2026-03-06
- ✨ Add right-click context menu on empty ERD canvas to create new entity or legend.
- 🐛 Fix entity resize handles not following entity after dragging when returning from connection page.
- 🐛 Fix Sentry breadcrumb error appearing in dev tools on local environment.

## v0.9.9 - 2026-03-05
- 🐛 Fix NULL default value out of sync with nullable checkbox in Field Settings.

## v0.9.8 - 2026-03-04
- ⬆️ Upgrade dependencies.
- ✨ Initial entity layout: cap maximum entity height and place the most-connected entity at top-left.
- ⚡ Optimize snap-to-guide to avoid unnecessary re-renders during drag.
- ⚡ Hide relations during entity drag for smoother performance on large diagrams.
- 🐛 Ensure primary key is created before foreign key.

## v0.9.7 - 2026-03-03
- 🐛 Fix can not access offline ERD.

## v0.9.6 - 2026-03-03
- ✨ Arrange new connect database in masonry layout.
- ✨ Display waiting indicator when connecting and migrating.
- 🐛 Fix race condition that caused entity handle to remain visible when moving.
- 🐛 Fix error when using Ed25519 SSH key for tunnel connection.

## v0.9.5 - 2026-03-01
- ✨ Improve migration: smarter diff-based SQL generation for PostgreSQL and MySQL.
- 🐛 Fix silent error when testing connection.
- 🐛 Fix entity lost focus and display wrong existing value after editing.
- 🐛 Fix issue: ERD attributes (position, size, color) lost after renaming an entity.
- 🐛 Fix issue: cannot move entity using Ctrl + arrow / hjkl keys.
- 🐛 Fix laggy migration dialog after submitting.
- 🐛 Fix issue: error message didn't hide after closing the migration form.
- ✨ Ensure new entity created from a template or N:N table has enough height to display all fields.
- 🐛 Fix duplicate primary key error when creating N:N intermediate table in MySQL.
- 🐛 Fix incorrect default value for reserved keyword `USER` in PostgreSQL migrations.
- ✨ Send migration errors (including SQL) to Sentry for monitoring.

## v0.9.4 - 2026-02-28
- ✨ Re-brand to "Schemity" and update the logo.
- ✨ Add "Open data folder" button to the control bar with keyboard shortcut `Ctrl+Shift+O` (⌘⇧O on macOS).
- 🐛 Fix error when opening an offline ERD (pure drawing) caused by redundant backend calls attempting to parse missing connection fields.

## v0.9.3 - 2026-02-27
- ✨ Re-brand to "Schemity" and update the logo.

## v0.9.2 - 2026-02-27
- ✨ Clicking a foreign key field now highlights the associated relation line and the corresponding source field in the source entity.
- 🛠 Fixed relation color being overridden to black when selected — relations now keep their original color at all times.
- 🛠 Fixed 1:1 relationship endpoint always rendering in black instead of the relation's color.
- 🎨 Fields are now sorted on first load.
- 🛠 Fixed timestamp ordering rule to ensure correct sort order.
- 🛠 Increased minimum entity height for better readability.

## v0.9.1 - 2026-02-26
- ✨ Clicking a foreign key field now highlights the associated relation line and the corresponding source field in the source entity.
- 🛠 Fixed relation color being overridden to black when selected — relations now keep their original color at all times.
- 🛠 Fixed 1:1 relationship endpoint always rendering in black instead of the relation's color.
- 🎨 Fields are now sorted on first load.
- 🛠 Fixed timestamp ordering rule to ensure correct sort order.
- 🛠 Increased minimum entity height for better readability.

## v0.9.0 - 2026-02-25
- ✨ Added entity footer showing field count, index count, unique constraint count, and check constraint count at a glance.
- ✨ Added equal-spacing snap guide: when dragging an entity near others, a guide snaps it to an equal gap position and shows bracket indicators.
- 🛠 Fixed entity height not increasing correctly when creating a new relation (foreign key fields now expand the target entity properly).
- 🎨 Default values starting with `nextval` now display as `nextval`, and those starting with `uuid_` display as `uuid`, for a more compact view.
- 🛠 Fixed inconsistent entity border thickness around field rows.
- 🛠 Entities are now brought to the top when resized or auto-resized, preventing them from being hidden behind other entities.
- 🛠 Fixed a race condition that prevented snap guides from clearing after finishing a drag.
- 🛠 Fixed performance degradation when moving an entity near the stage edges.
- 🛠 Fixed an error that prevented expanding the stage by dragging an entity to its edge.
- 🛠 Fixed inconsistent z-ordering of entities when dragging.
- 🎨 Updated the new relation button style for a more prominent appearance.
- ⚡ Improved performance when dragging multiple selected entities.
- ⚡ Improved schema fetching performance.
- 🛠 Relations between selected entities are now hidden while moving for cleaner drag feedback.

## v0.8.2 - 2026-02-21
- ✨ Legend header now displays a colored background matching the legend color, with auto-contrasting text.
- 🛠 Moving multiple selected entities now preserves the shape of relations between them.
- ✨ Legend resize handles are now rendered above entities, making them always accessible without moving entities out of the way.
- 🎨 Default legend color is now pre-filled when creating a new legend.
- 🛠 Disabled text selection on the canvas to prevent accidental highlighting during drag.
- 🛠 Improve release notes styling.

## v0.8.1 - 2026-02-10
- 🛠 Improve release notes styling.

---

Releases before v0.8.1 (v0.5.6 through v0.8.0, late 2025 to early 2026) predate in-app release notes and are not listed individually.
