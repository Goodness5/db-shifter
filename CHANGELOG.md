# Changelog

All notable changes to this project will be documented in this file.

## 0.1.8 - 2026-01-18

### Added
- **Column-based sync**: New `--columns` flag allows you to specify which columns to sync (e.g., `--columns id,name,email`). Primary key is always included automatically.
- **Circular foreign key handling**: Automatic detection and handling of circular foreign key dependencies. The tool now:
  - Detects circular FK relationships between tables
  - Temporarily disables FK constraints for tables in circular dependencies during sync
  - Provides optimal sync order based on FK dependencies
  - Warns about circular dependencies and provides guidance

### Improved
- Better foreign key dependency analysis with topological sorting
- Enhanced sync order optimization to minimize FK constraint violations
- More informative output about FK relationships and sync order

## 0.1.7 - 2025-xx-xx

- Version bump for previous features.

## 0.1.5 - 2025-09-05

- Fix: properly handle Python dict/list while inserting by wrapping with psycopg2.extras.Json.
- Docs: add changelog and project URLs in package metadata.

## 0.1.4 - 2024-xx-xx

- Initial public release.
