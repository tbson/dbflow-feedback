# Example Schemas

Real, importable example workspaces for [Schemity](https://schemity.com) - and living proof of its storage format: every Schemity diagram is a plain JSON file you can read right here on GitHub. No export step, no proprietary binary - what you see in these folders is exactly what Schemity saves to disk.

## How to use an example

1. Download or clone this repository
2. In Schemity, import the example workspace folder (an ERD-only workspace needs no database connection)
3. Open the diagram and explore - context views, legends, constraints, and relationships included

Each example ships in up to three formats:

- **Schemity workspace** (`workspace/`) - the native plain-JSON files, ready to import
- **DBML** (`schema.dbml`) - for dbdiagram.io and the DBML toolchain
- **SQL** (`schema.sql`) - CREATE TABLE statements you can run or import anywhere

## Examples

<!-- TODO: export sample workspaces from Schemity and list them here, e.g.:

| Example | Entities | Shows off |
|---|---|---|
| [multi-tenant-rbac](multi-tenant-rbac/) | ~10 | Tenants, users vs members, N:N role-permission with junction tables, composite unique constraints |
| [ecommerce](ecommerce/) | ~15 | Orders, payments, inventory; check constraints as domain rules; context views per subdomain |

Companion reading: https://schemity.com/blog/design-data-model-for-multi-tenant-rbac
-->

*Examples are being prepared - watch this repository to be notified when they land.*
