# Local Filesystem Webservice

## Overview

The **Local Filesystem** webservice accesses local paths for file I/O. It supports read/write/list operations and directory management via a lightweight wrapper around Symfony Filesystem.

## When to Use

Use the Local Filesystem webservice when you need to:

- Work with files on the SyncEngine host
- Stage temporary directories for pipelines

## Settings

### Authentication

**Required** | **Type**: Mixed

- **Root path**: Absolute path or relative path.
- **Relative path?**: Makes `root` relative to installation root (`dir.root`).

## How It Works

1. **Root Resolution**: Builds absolute path based on `relative` flag.
2. **Client**: Provides `get/put/nlist/scandir/mkdir/remove/rename` operations.
3. **Connect**: Ensures root exists; creates if missing.
4. **I/O**: `_get` writes content to temp resource; `_put` reads from temp resource when provided.

## Use Cases

### Cache Intermediate Artifacts

**Configuration:**
- **Root**: `/var/sync/cache`

Result: Writes temporary files for later tasks.

## Best Practices

**Permissions**: Ensure process has read/write access on target paths.

**Path Hygiene**: Normalize separators; avoid trailing slashes in `root`.

## Notes

- Uses Symfony Filesystem under the hood
