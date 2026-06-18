# jira filter / dashboard / field

Canonical references:
- filter: https://developer.atlassian.com/cloud/acli/reference/commands/jira-filter/
- dashboard: https://developer.atlassian.com/cloud/acli/reference/commands/jira-dashboard/
- field: https://developer.atlassian.com/cloud/acli/reference/commands/jira-field/

Shared flags: [`common-flags.md`](common-flags.md).

---

## filter list / search / add-favourite / change-owner

**list**: `--my` (owned by me) / `--favourite` / `--json`

**search**
| Flag | Short | Description |
|------|-------|-------------|
| `--name` | `-n` | Partial name (case-insensitive) |
| `--owner` | `-e` | Owner email |
| `--limit` | `-l` | Max results (default 30) |
| `--paginate` / `--json` / `--csv` | | See [common-flags.md](common-flags.md) |

**add-favourite**: `--filterId <id>`
**change-owner**
| Flag | Short | Description |
|------|-------|-------------|
| `--id` | | Comma-separated filter IDs |
| `--owner` | | New owner email |
| `--from-file` | `-f` | Read filter IDs from file |
| `--ignore-errors` / `--json` | | See [common-flags.md](common-flags.md) |

```bash
acli jira filter list --my --json
acli jira filter search --name "report" --paginate --csv
acli jira filter change-owner --id 1,2 --owner you@example.com
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

## field create / delete / cancel-delete

**create**
| Flag | Description |
|------|-------------|
| `--name` | Field name |
| `--type` | Field type, e.g. `com.atlassian.jira.plugin.system.customfieldtypes:textfield` |
| `--description` | Description |
| `--searcherKey` | Searcher key |
| `--json` | JSON output |

**delete — CAUTION (reversible):** moves a custom field to trash. Confirm first; restore with `cancel-delete`.

**cancel-delete**: restore a field from trash.

```bash
acli jira field create --name "Customer Name" --type "com.atlassian.jira.plugin.system.customfieldtypes:textfield"
acli jira field delete --id customfield_12345
acli jira field cancel-delete --id customfield_12345
```
