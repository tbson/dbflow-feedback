# Schemity Changelog

Release notes for [Schemity](https://schemity.com), the offline desktop ERD tool. Newest first. Download the latest version at [schemity.com](https://schemity.com/#platforms).

## v2.10.1 - 2026-09-02
- ✨ Selecting an entity draws every relation touching it at double weight, so its wiring is traceable across a dense diagram at a glance. Both ends count rather than only the end that owns the foreign key, and weight is used rather than a highlight colour, because entity colours are the designer's and any fixed hue eventually collides with one. The connected line keeps its own colour and stays solid, so solid against dotted is what separates it from the selected relation, and an ON DELETE CASCADE end stays one step heavier still. The emphasis is canvas state and reaches neither the SVG nor the image export.
- ✨ The new-relation dialog opens on the ON DELETE and ON UPDATE actions you picked last, so a schema that cascades most of the time stops re-picking CASCADE on every dialog. The memory is kept separately per relation type, so a 1:N habit cannot quietly redefine what 1:1 or N:N start with. It is a local preference rather than diagram data - nothing here travels with an export - and a remembered value is used only when the current dialect implements it, so a RESTRICT remembered from PostgreSQL falls back to the default on SQL Server. The AI is deliberately left out, since it chooses each relation's actions from what the child row means rather than from a habit.
- ✨ The blank space under the workspace list and under the diagram list is somewhere to right-click rather than dead area: New workspace and Import workspace on one, New diagram and Import diagram JSON on the other. The menu items are the same actions the header buttons carry and are gated by the same licence check, so an item is disabled exactly where its button is rather than quietly missing.
- 🔧 Entity and legend swatches carry each hue as far as the colour space allows instead of holding every hue down to the dullest one. Each tier used to give all ten hues one identical chroma, capped by whichever hue ran out of gamut first, so teal's ceiling held the blues and violets to a third of what they could carry. Every swatch now sits at 92 percent of the largest chroma its own hue can hold at that tier's lightness. Lightness stays fixed per tier, so header text still flips between black and white on the same side of the line, on the canvas and in the SVG export alike, and stored colours are untouched - the palette is only the set of suggestions the picker offers.
- 🔧 The canvas cursor matches the gesture it is about to perform: resize handles carry the direction they resize in, the new-relation anchor is a crosshair because it is dragged rather than clicked, a field row is a grab because it gets picked up and dropped into a new slot, and a relation waypoint keeps an open hand where it used to show a closed one over a point nobody was holding. The cursor is then locked for the length of a gesture, since a resize outruns its own handle box and a relation being bent crosses every entity in its way.
- 🔧 The splash screen names the version about to start, on a ground its logo can be seen against: the glyph is drawn in the brand blue and only the wordmark in white, so the mid blue behind both left half the logo effectively invisible at every launch. The version is stamped in at build time, so the splash stays a single document with nothing to fetch before it paints, and reduced-motion is honoured.
- 🔧 The workspace, diagram and template dialogs name their action on the confirm button - Create on a new row, Update on an existing one - read from the same flag the dialog title already follows, so the title and the button cannot disagree. This is the convention the ERD dialogs already used.
- 🐛 A boolean default value is recognized whatever case it arrives in. Reverse engineering from a live database and importing DBML both yield a lowercase true or false, while the field dialog offers TRUE and FALSE - and 1 and 0 for SQL Server's BIT - so nothing matched and the literal rendered as a custom expression rather than as the value it plainly is.
- 🐛 The template list keeps its add button when it is empty.

## v2.10.0 - 2026-08-28
- ✨ The interface speaks Japanese and Simplified Chinese, picked from a globe dropdown beside the theme toggle on both the connection screen and the diagram control bar. Every language names itself - English, 日本語, 简体中文 - because someone who has landed in a script they cannot read still has to recognise their own option. The app opens in English and the choice is explicit rather than sniffed from the operating system, and switching applies instantly with no reload: the diagram, the pan position and every open tab survive it. AI chat replies follow the chosen language, and the privacy and terms pages are written per language rather than fed through a string catalogue. Generated SQL, DDL, migrations and the exported data dictionary stay English on purpose, so two people exporting the same schema in different languages still produce the same file to diff. The canvas monospace stack gains CJK fallbacks so a Japanese or Chinese label is not tofu on the canvas or in the SVG export.
- ✨ A database password can come from a shell command instead of being stored. A credential source toggle in the connection dialog switches between Password and Command; on connect Schemity runs that command through your login shell and uses its trimmed stdout as the password, which is how one mechanism covers AWS RDS IAM tokens, Vault dynamic secrets and a password manager with no vendor-specific code for each. Nothing is persisted: the resolved credential lives in memory for five minutes, then it is fetched again on the next connect. A Test command button proves it before you save, reporting only how many characters came back and never the credential itself. The command runs from your home directory with stdin closed and sees the PATH from your shell profile rather than the app's own environment, so a Homebrew or nvm binary is found even though the app was launched from the Dock, and the shell is killed if it has not exited within thirty seconds. Output must be exactly one line after trimming, and every failure is prefixed "Password command failed: " so it is never mistaken for the database rejecting your credentials. Switching a connection to Command deletes its stored database password from the OS keyring. The command string itself is stored in cleartext beside the host and port, so it must call a credential helper rather than carry a secret inline.
- ✨ A shared diagram opens in the theme its author published it in, so a diagram designed in dark greets everyone in dark rather than following whatever the visitor's own app happened to be set to. The theme is stamped onto a copy of the data at publish time, so the author's file is never written and flipping theme cannot dirty their diagram, and the viewer applies it as a preview rather than as a setting: opening someone else's link never rewrites the visitor's saved preference, and their own toggle still wins for the rest of the visit.
- ✨ An embed can hide its footer link with `?hidelink=1`, for a host page that already links back or has no room for one. Only the outbound link goes: the database type, the minimap toggle and the theme toggle stay, so an embedded viewer can still be switched between light and dark. The flag is strict - an absent parameter, a typo, a bare `&hidelink` and `hidelink=0` all leave the link showing - and "Copy embed code" is unchanged and still keeps the link in every snippet it produces.
- ✨ A double click anywhere on a context map box opens that context view. It is the same action the enter icon on a selected box performs and goes through the same path, so the unsaved-changes rules and the cancel route are identical - the icon stays as the discoverable way in, the double click is the shortcut for people who already know it. Dragging a card is kept apart from selecting it, so a press that stays put still selects and one that travels does not.
- 🔧 Panning cost now tracks what is on screen rather than how big the schema is. Entity and relation groups outside the viewport are hidden as the view moves, with a pad so nothing pops in late, and the picking buffer nothing reads mid-gesture is skipped during a wheel pan and caught up on the next pointer press. On a 150-entity, 220-relation diagram a pan frame went from 7.3ms to about 1.0ms. Exports are untouched: anything that rasterizes the live canvas reveals the hidden shapes first.
- 🔧 The Context Map pans smoothly on a busy map: its dependency arrows were being rendered through a window-sized scratch canvas once per arrow per frame, which on a 14-box, 21-arrow map was the whole of the lag - a full draw measured 9.0ms with that on and 0.9ms with it off.
- 🔧 Image exports are encoded through a blob rather than a base64 data URL, which removes several full-size transient copies of the picture from every export, and the canvas background layer is gone because the colour it painted was already behind it in CSS - a layer that cost tens of megabytes at a typical window to paint nothing new.
- 🔧 The toolbar no longer acts on the canvas hidden underneath the Context Map. New, Reset ERD, Reset Layout, Paste, Import, Template, the context view manager, Lint and AI Chat all still edited a diagram nobody could see - Reset ERD worst of all, re-introspecting the database and discarding layout with nothing on screen to show for it. The nine are disabled rather than hidden, since this is a mode you leave in one click, and their tooltips say why. Search opens the map's own drawer, which is what Cmd/Ctrl + F already did.
- 🔧 The native menu bar is down to what Schemity actually uses. Windows and Linux draw that bar inside the app's own window, where a File / Edit / Window / Help strip sat between the title bar and the control bar with nothing of ours ever in it, so those platforms now get no menu at all and nothing is lost with it: the webview handles cut, copy, paste and select-all natively, the diagram owns its own undo and redo, and Ctrl+Q is caught and answered by the unsaved-work prompt. macOS keeps a bar because it cannot lose one, trimmed to Schemity, Edit and Window, and its About panel shows the app icon rather than a generic folder.
- 🔧 Every dialog is pinned near the top of the window and a tall one scrolls its own body, so a short screen never cuts off the Cancel and Save buttons. It is set once at the root rather than per dialog, so all twenty get it, confirm boxes included. A tall dialog's scrollbar also takes its own gutter now instead of painting over the right edge of every input, with a neutral grey thumb per theme.
- 🔧 Edge auto-scroll is a ramp rather than a switch. One pixel inside the zone used to be full speed, which grabbed the view and ran; speed now grows with the square of how far into the zone the pointer is - stationary at the inner edge, full only at the container edge - which takes one pixel in from 15 per tick to 0.015. The zone narrows from 50px to 32px, and entity drag and marquee selection share the same auto-pan.
- 🐛 A new foreign key lands below the key block rather than inside a composite primary key. Drawing a relation into a junction table used to drop the new column between its two primary keys, and the same fallback put a foreign key after the first plain column in an entity with no keys at all rather than at the top. The rule now reads primary keys, composite primary-foreign keys and foreign keys as one block and places below the last of them. An entity already carrying a misplaced column is not rewritten: field order is yours to arrange.
- 🐛 A new junction table lands at the nearest free spot, and in view. An N:N between two diagonally arranged entities used to drop the junction off the bottom of the frame with the whole midpoint area left empty, because any overlap at all relocated it below both parents. The search now works outward from the preferred spot ring by ring, axis directions before diagonals, with every entity counted as an obstacle, and the visible area is searched first since the junction is created selected.
- 🐛 A relation whose route collapses to two points runs straight again, instead of leaving a corner-to-corner diagonal between two entities that plainly overlap after an entity was dragged over one of its own waypoints. The fix is in the move itself rather than in context view logic, so an existing context view heals on the next drag rather than needing to be rebuilt.
- 🐛 Panning can reach a relation waypoint dragged below every entity. The world was measured from entities and legends only, so a waypoint dropped past the lowest of them sat outside anything scrollable, which meant the reshape could be made but not unmade.
- 🐛 Deleting a workspace leaves you where you were: removing a row you are not standing in no longer navigates away, and removing the one you are in lands on the top of what remains - which is user-ordered - rather than on the default workspace.
- 🐛 F9 resizes the window to 16:9 from anywhere in the app, the connection page included and with a dialog open, and can no longer leave the window at an invalid size: the ratio is measured across the outer frame while the app's minimum is a content height, so it now yields room for whatever chrome the platform draws. On the web it is not registered at all, leaving the key to the browser.
- 🐛 Both routes that import entities into a context view seed their relations by the same rule. The store action and the import dialog each carried their own copy and the two had drifted, mirroring what was already true of entity positions - relations were the half that had never been given the same treatment.

## v2.9.7 - 2026-08-23
- ✨ The toolbar and footer fold away together to hand the whole window to the canvas - F10, or the button beside Help - and an eye in the top right corner brings them back, as does F10 again. It is built for screen recording and presenting, where the diagram is the subject and the chrome is not, and it earns its place on a short laptop screen too. The eye is an ordinary button rather than a faded one, because it is the only way back from a mode that hides the way back, and it is half transparent so an entity or an arrow underneath still reads through it. The state is deliberately forgotten on restart, so the app never launches into a bare window nobody asked for.
- ✨ F9 squares the window off to 16:9, so a recording or a screenshot comes out at the ratio it will be published at without resizing by hand.
- ✨ Copy and paste the same copy as many times as you need: a copy stays on the clipboard rather than being consumed by the first paste, and repeated pastes cascade, each one stepping further from the last, so putting one entity down three times is one copy and three pastes. A fresh copy restarts the cascade from the new source rather than continuing where the previous run stopped.
- ✨ The new-relation handle stays available when a field is selected, so a relation can be dragged straight from the column you mean it to start at. Selecting a field used to hide the handle at exactly the moment you had pointed at the right column, which forced a detour through the entity's header or footer - and the instinct it blocked was the correct one, because the relation copies the source entity's primary key into the target, so dragging `users` onto `posts` produces `posts.user_id`.
- 🔧 Shift is the multi-select modifier on every platform: Shift+click adds one entity to the selection and Shift+drag box-selects an area, with no platform branch and no collision with a system gesture. The platform's own toggle key stays as an alias for people arriving from file lists - Cmd on macOS, Ctrl elsewhere - while Ctrl+click on macOS is left to the OS as the secondary click, where reading it as a modifier had opened an entity's context menu and quietly toggled that entity out of the selection the menu was about to act on. A right-click on a legend header no longer changes the selection before opening its menu either, and both the click and the box-select are now in the shortcut list.
- 🔧 Quitting never discards unsaved work without asking. Closing a tab prompted and closing the window prompted, but Cmd+Q went straight to termination, so Quit asks first and exits only once you accept - as the app's own menu item on macOS, and as a Ctrl+Q caught before the system sees it on Windows and Linux. That check reads every open tab rather than only the diagram on screen, so a dirty session sitting behind the current one is no longer lost silently, and a read-only session never counts as dirty.
- 🔧 A many-to-many relation opens at ON DELETE CASCADE and ON UPDATE CASCADE, because a junction row is nothing but the link between its two parents and has no meaning left once either is gone. Every cardinality used to open at NO ACTION. Switching the cardinality back to 1:N or 1:1 moves the actions back with it, and an action you set yourself is left alone for the rest of the dialog.
- 🔧 The round control buttons on an entity read as one family with the entity beside them: their ring is now heavier than the entity border rather than matched to it, and carries its own lighter value on the dark theme, where it had been less visible than the disc it outlines. The new-relation crow's foot in its chip is redrawn centred with its prongs separate, having read as a filled left-pointing arrow at button size.
- 🔧 Selection and dragging stay quick on a busy diagram. Five hand-rolled copies of the group-move relation recompute collapse into one that builds its index a single time, so committing a drag no longer costs the size of the selection multiplied by the relation count, and the dialogs and menus mounted behind the canvas are memoised - one click on an eleven-entity diagram goes from 177 component renders to 113.
- 🐛 Drawing a relation never writes a redundant UNIQUE constraint beside a single-column primary key: the derivation that creates unique constraints from fields flagged unique now skips a field that is the whole primary key, where an entity whose `id` still carried that flag used to grow a duplicate constraint - complete with a U badge and a migration to create it - the moment a relation landed on it. A column inside a composite primary key is still not skipped, since UNIQUE on one member of a composite key is a stricter constraint rather than a restatement, and constraints already stored are left alone.
- 🐛 The AI schema plan no longer proposes a unique constraint that the primary key already provides. Nothing had ever reached the diagram, since the unique flag is cleared on whichever field is promoted to primary key, but the plan shown for review is the thing you approve.

## v2.9.6 - 2026-08-21
- ✨ Diagrams move between the desktop app and Lite as JSON: Export gains a "Schemity JSON" row that writes the file exactly as a save would, and the diagram list gains an import button beside the Plus. An import picks a free id rather than overwriting, and carries the suffix into the name so two imports of one file do not read identically. A diagram imported into the web build arrives without its connection, which the browser could never reach anyway; desktop keeps the connection and asks for the password, which never travels in the file, on the first connect.
- ✨ Search can be asked for one kind of thing: a query starting with `[e`, `[f` or `[l` scopes results to entities, fields or legends before any matching happens, so a bare `[f` lists every field and `[fbath` reaches only fields. The closing bracket and the space are both optional - `[fbath`, `[f]bath` and `[F] bath` all mean the same - and anything that is not a valid sigil is still matched literally, so a field genuinely called `[status` is still findable. The sigil is the same letter the result rows have always been prefixed with.
- ✨ Fields match on their type as well as their name, using exactly the text the canvas shows: `VARCHAR(255)` matches on its length, `TEXT[]` on its array suffix, and `[feric` reaches every NUMERIC column in the diagram - which is how you find the columns that share a shape rather than a name, such as every money column stored at the wrong scale. Names still rank above types, so a search for `text` finds the things called text before the hundreds of columns that are one.
- ✨ A PostgreSQL enum column shows the values its type allows: introspection reads the labels in their declared order - the list a MySQL ENUM column has always carried - and the field dialog shows them as read-only tags, because the values belong to the type rather than to the column. An enum-typed column used to arrive with an empty value list, so the type looked like any other scalar.
- ✨ A legend becomes a context view in one step: "Import to context views" now leads with an option to create a context view named after that legend and coloured like it, in the same confirm, rather than sending you to the manager to create one, name it a second time, and come back to import. Other context views can still be ticked in the same pass, and if a context view already carries that name the option starts ticked with the reason shown, instead of offering a second entry that differs from the first only by a counter.
- ✨ AI-generated relations say what happens to a child row: the model chooses ON DELETE and ON UPDATE per relation - CASCADE for a child owned by its parent, SET NULL for an optional nullable link, RESTRICT for a record of what happened - and names the delete rule for each one in its explanation. Existing relations are described to the model with the actions they already carry, and an action the model invents is rejected rather than written into DDL the database will refuse.
- ✨ Get SQL covers the whole selection, whichever way it is reached. With five entities selected, the entity right-click menu, the keyboard shortcut and the command palette all read the same selection, and the multi-select menu offers Get SQL alongside duplicate, import and delete - so the SQL of an arbitrary group of tables is one gesture rather than a table at a time. It stays hidden for a single database view, which has no SQL of its own.
- 🔧 The selected relation is findable in a dense diagram because it changes in kind rather than degree: a fixed contrast colour, black on light and white on dark, drawn as dots, since nothing else on the canvas is dotted. Selection used to be a 1px to 2px step in the relation's own colour, which on a dark canvas left it indistinguishable from its neighbours in a bundle of parallel lines. The crow's feet and cardinality bars stay solid, because they are notation, and a selected relation stops hopping over the lines it crosses; the SVG export is untouched.
- 🔧 The AI assistant gets the output room its model allows. Every provider was capped at 8192 tokens, the shared ceiling when the assistant shipped and today the ceiling of only the oldest models, so a whole-diagram plan was cut off mid-JSON and rolled back. The budget is now generous by default and clamped only for the model families known to refuse it.
- 🐛 A one-to-many drawn between an existing primary key and a new entity no longer arrives as a one-to-one: the foreign key is unique when the relation is one-to-one and for no other reason, instead of inheriting uniqueness from the column it points at.
- 🐛 PostgreSQL, SQL Server and SQLite diagrams no longer accumulate a MySQL charset and collation on every field they create. The default now follows the connection.
- 🐛 NUMERIC and DECIMAL no longer demand a precision no engine asks for, so renaming or describing an unconstrained numeric column read in from the database no longer requires inventing one - and no longer generates an ALTER TABLE for a field you only meant to rename.
- 🐛 A default that is one of a field's allowed values shows as that value in the Default select, rather than falling back to a custom input, whether it arrived SQL-quoted or as a plain number.
- 🐛 A workspace created or imported inside a Git repository is marked as versioned straight away, rather than on the next reload.
- 🐛 Saving from the Context Map clears the unsaved marker, and Ctrl/Cmd + W there closes the diagram rather than the application.
- 🐛 A keydown dispatched at the window with no element target no longer throws in the shortcut handler.

## v2.9.5 - 2026-08-17
- 🔧 Release build only; no user-facing changes since v2.9.3.

## v2.9.4 - 2026-08-17
- 🔧 Release build only; no user-facing changes since v2.9.3.

## v2.9.3 - 2026-08-17
- ✨ Data dictionary export: the diagram exports as a document rather than a picture, in three formats for three readers - HTML to print or hand to someone who will never open Schemity, Markdown to commit beside the code, and an Excel workbook to filter and sort. Each covers every entity and column with its type, key, nullability, default and description, the unique, check and index constraints on each table, the relationships with their cardinality and delete rules, and the notes held in legends and context views. The export follows the active view, so exporting from a context view documents that context alone.
- ✨ Database views appear in the data dictionary, labelled as views rather than dropped. The SQL, DBML and Mermaid exports still leave them out because those formats describe tables you can create; a document read by someone checking what exists is a different job.
- ✨ The data dictionary closes with what is not written down yet: how many entities and fields carry a description, and the names of those that do not. It counts only what can be changed, so the read-only columns of a database view are never listed as missing anything.
- ✨ The Excel data dictionary is a six-sheet workbook - overview, entities, fields, constraints, relationships and notes. Every sheet is present even when it has no rows, so a spreadsheet built on top of one export still works on the next, and column counts are written as numbers rather than text so they total.
- ✨ Fields carry a description of their own, and a documented field is visible without opening it: a bar on the leading edge of the row marks any field that has one, the field-level answer to the corner triangle entities and legends already use. It is drawn in SVG exports too.
- ✨ Documenting a column is not a schema change: field descriptions live in the diagram and never reach the database, so a sentence of prose no longer produces a migration to review and apply. It also ends silent loss - descriptions used to be overwritten on every refresh, invisibly on PostgreSQL and MySQL and permanently on SQL Server and SQLite. Importing an already documented schema still arrives documented, so comments that exist in the database are read in, just never written back.
- ✨ Every field carries an icon for what it holds: T for text, a hash for whole numbers, digits around a point for fractional ones, a toggle for booleans, a calendar for dates and times, braces for JSON, and a dedicated glyph for UUID. Arrays draw their element's icon inside square brackets, so a `text[]` column no longer claims to be a string. Types are grouped by what the value is rather than by what an engine calls it, so INT and BIGINT share a glyph, as do DATE and TIMESTAMPTZ, while primary and foreign keys keep their own icons untouched.
- ✨ Paste a connection string and the connection dialog fills itself in: PostgreSQL, MySQL and SQL Server URIs are understood, along with a `jdbc:` prefix and the ADO.NET key/value form the Azure portal hands out. It fails closed rather than guessing - a malformed URL or a bad port is reported instead of being half-applied - and every parameter it did not use is listed rather than dropped in silence.
- ✨ TLS verification, not just TLS: verify-ca and verify-full sit alongside the existing modes in the encryption picker and check the server's certificate against a chain instead of only encrypting the link, with custom root CA and client certificate files passed through to the drivers. Existing workspace files load unchanged, and SQL Server connections saved as REQUIRE keep verifying rather than quietly dropping to unverified.
- ✨ The Context Map is a two-way door rather than a dead end: an enter icon on the selected box opens that context view, every context view carries a floating Context Map button to get back, and the map's exit button lands on Main, which nothing else on the map could reach. Escape closes the map in place and returns you to the view you were in.
- 🔧 A quick diagram never has to answer questions about database transports: connection setup hides behind a "Connect to a database" link, so the short path is name, database type, naming, save. The link is a single toggle that stays where you clicked it, and "Design only" gives it a visible undo that preserves anything already typed.
- 🔧 Opening a connection asks for the operating system keychain once instead of more than ten times. Anyone who pressed Allow rather than Always Allow used to be prompted again for every command, including the ones that went on to answer from cache without using the password at all.
- 🔧 Environment tags read better: the labels are short (Local, Stag, Prod), the picker has a blank entry so having no environment is a valid answer rather than an unfinished one, and the badge appears on a diagram only when there is a connection behind it.
- 🔧 The foreign key naming convention says what it actually applies to, in a label tooltip: only the names Schemity writes itself - the foreign key field added when a relation is drawn, and the composite keys of a junction table - never the names you type yourself.
- 🔧 The unique marker is violet, per theme, rather than one shared red that left the glyph meant to catch the eye as the faintest thing on the dark canvas. Violet also stops a correct design decision borrowing the vocabulary of a fault.
- 🔧 Both themes are legible on their own terms. Floating surfaces - dropdowns, context menus, select popups, popovers, modals and drawers - have a visible edge in dark mode, where a near-black shadow used to swallow the boundary; on the light theme an uncoloured entity has a visible header again instead of a white body with no head.
- 🔧 Diagrams are called diagrams everywhere the interface can be read, and the entity and field dialogs were tightened.
- 🔧 Upgraded dependencies.
- 🐛 Exports no longer cut off relationships routed around the entities.
- 🐛 A field can be dragged to the first position.
- 🐛 The collation name no longer truncates in the field dialog, and the charset and collation lists survive a refresh.
- 🐛 The field options subtitle stays legible in dark mode.
- 🐛 MySQL: a foreign key stays indexed when the unique constraint serving it is dropped, and a unique constraint's backing index is no longer reported as a separate index.
- 🐛 SQLite no longer reports a migration for an untouched diagram, and a save that does produce a migration logs why.
- 🐛 A brief licence-server outage no longer locks the app.

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
