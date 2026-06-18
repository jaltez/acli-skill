# jira filter / dashboard / field

Canonical references:
- filter: https://developer.atlassian.com/cloud/acli/reference/commands/jira-filter/
- dashboard: https://developer.atlassian.com/cloud/acli/reference/commands/jira-dashboard/
- field: https://developer.atlassian.com/cloud/acli/reference/commands/jira-field/

Shared flags: [`common-flags.md`](common-flags.md).

---

## filter list / search / view / update / add-favourite / change-owner / list-columns / reset-columns

**list**: `--my` (owned by me) / `--favourite` / `--json`

**search**
| Flag | Short | Description |
|------|-------|-------------|
| `--name` | `-n` | Partial name (case-insensitive) |
| `--owner` | `-e` | Owner email |
| `--limit` | `-l` | Max results (default 30) |
| `--paginate` / `--json` / `--csv` | | See [common-flags.md](common-flags.md) |

**view**: `--id <filter-id> [--json] [--web]`

**update**
| Flag | Description |
|------|-------------|
| `--id` | Filter ID to update |
| `--name` / `--description` / `--jql` | Fields to change |
| `--share-permissions` | JSON array of SharePermission objects (who can view) |
| `--edit-permissions` | JSON array of SharePermission objects (who can edit) |
| `--json` | JSON output |

**add-favourite**: `--filter-id <id>`

**change-owner**
| Flag | Short | Description |
|------|-------|-------------|
| `--id` | | Comma-separated filter IDs |
| `--owner` | | New owner email |
| `--from-file` | `-f` | Read filter IDs from file |
| `--ignore-errors` / `--json` | | See [common-flags.md](common-flags.md) |

**list-columns**: `--key <filter-id-or-key> [--json]` — configured columns for a filter.
**reset-columns**: `--id <filter-id>` — reset columns to defaults.

```bash
acli jira filter list --my --json
acli jira filter search --name "report" --paginate --csv
acli jira filter view --id 10001 --json
acli jira filter update --id 10001 --name "New" --jql "project = TEST AND status = Open"
acli jira filter change-owner --id 1,2 --owner you@example.com
acli jira filter list-columns --key FILTER-123 --json
```

---

## dashboard search

| Flag | Short | Description |
|------|-------|-------------|
| `--name` | `-n` | Partial name (case-insensitive) |
| `--owner` | `-e` | Owner email |
| `--limit` | `-l` | Max results (default 30) |
| `--paginate` / `--json` / `--csv` | | See [common-flags.md](common-flags.md) |

```bash
acli jira dashboard search --owner you@example.com --name "report"
```

---

## field create / update / delete / restore

**create**
| Flag | Description |
|------|-------------|
| `--name` | Field name |
| `--type` | Field type, e.g. `com.atlassian.jira.plugin.system.customfieldtypes:textfield` |
| `--description` | Description |
| `--searcher-key` | Searcher key |
| `--json` | JSON output |

**update**
| Flag | Description |
|------|-------------|
| `--id` | Custom field ID to update |
| `--name` / `--description` / `--searcher-key` | Fields to change |
| `--from-json` | Read update payload from JSON |
| `--json` | JSON output |

**delete — CAUTION (reversible):** moves a custom field to trash. Confirm first; restore with `restore`.

**restore**: restore a field from trash. `--id <customfield-id>`

> `field cancel-delete` is **DEPRECATED** (removal 2026-12-01) — use `field restore` instead.

```bash
acli jira field create --name "Customer Name" --type "com.atlassian.jira.plugin.system.customfieldtypes:textfield"
acli jira field update --id customfield_12345 --name "New Name"
acli jira field delete --id customfield_12345
acli jira field restore --id customfield_12345
```
