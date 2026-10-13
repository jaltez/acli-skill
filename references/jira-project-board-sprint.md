# jira project / board / sprint

Canonical references:
- project: https://developer.atlassian.com/cloud/acli/reference/commands/jira-project/
- board: https://developer.atlassian.com/cloud/acli/reference/commands/jira-board/
- sprint: https://developer.atlassian.com/cloud/acli/reference/commands/jira-sprint/

Shared flags: [`common-flags.md`](common-flags.md).

---

## project list / view

**list**
| Flag | Short | Description |
|------|-------|-------------|
| `--limit` | `-l` | Max projects (default 30) |
| `--paginate` | | Fetch all pages |
| `--recent` | | Up to 20 recently viewed |
| `--json` | | JSON output |

**view**: `--key PROJECT-KEY [--json]`

```bash
acli jira project list --recent
acli jira project view --key PROJECT-KEY --json
```

## project create / update

**create**
| Flag | Short | Description |
|------|-------|-------------|
| `--name` | `-n` | Project name |
| `--key` | `-k` | Project key |
| `--from-project` | `-f` | Clone from existing (company-managed only) |
| `--description` | `-d` | Description |
| `--lead-email` | `-l` | Project lead (defaults to parent project's lead) |
| `--url` | `-u` | Project URL |
| `--from-json` `-j` / `--generate-json` `-g` | | JSON load / template |

**update**
| Flag | Short | Description |
|------|-------|-------------|
| `--project-key` | `-p` | Key of project to update |
| `--key` | `-k` | New key |
| `--name` | `-n` / `--description` `-d` / `--lead-email` `-l` / `--url` `-u` | Fields |
| `--from-json` `-j` / `--generate-json` `-g` | | JSON template support |

```bash
acli jira project create --key NEW --name "New Project"
acli jira project create --from-project PROJECT-KEY --key NEW --name "Cloned"
acli jira project update --project-key OLD --key NEW --name "Renamed"
```

## project delete — DESTRUCTIVE (irreversible)

> Permanently deletes the project **and all its work items**. Confirm with the user first.

```bash
acli jira project delete --key PROJECT-KEY
```

## project archive / restore — reversible

```bash
acli jira project archive --key PROJECT-KEY
acli jira project restore --key PROJECT-KEY
```

---

## board create / view / delete

**create**
| Flag | Description |
|------|-------------|
| `--name` | Board name (< 255 chars, required) |
| `--type` | `scrum` or `kanban` |
| `--filter-id` | Filter ID the board is built on |
| `--location-type` | `project` or `user` |
| `--project` | Project key/ID (required if `--location-type project`) |
| `--json` | JSON output |

**view**: `--id <board-id> [--json]`

> `acli jira board get` is **DEPRECATED** (removed 2026-12-01) — use `board view`.

**delete — DESTRUCTIVE**: deletes one or more boards.
| Flag | Description |
|------|-------------|
| `--id` | Comma-separated board IDs (required) |
| `--yes` | Skip confirmation |

```bash
acli jira board create --name "My Scrum Board" --type scrum --filter-id 10040 --location-type project --project PROJ
acli jira board view --id 123 --json
acli jira board delete --id 123,456           # confirm first
```

## board list-projects

| Flag | Description |
|------|-------------|
| `--id` | Board ID (required) |
| `--limit` / `--paginate` / `--json` / `--csv` | See [common-flags.md](common-flags.md) |

```bash
acli jira board list-projects --id 123 --json
```

## board search / list-sprints

**search**
| Flag | Description |
|------|-------------|
| `--name` | Partial name match |
| `--project` | Filter by project key |
| `--type` | `scrum`, `kanban`, `simple` |
| `--filter` | Filter ID (boards relevant to a filter) |
| `--order-by` | `name`, `+name`, `-name` |
| `--limit` / `--paginate` / `--private` | See [common-flags.md](common-flags.md) |
| `--json` / `--csv` | |

**list-sprints**
| Flag | Description |
|------|-------------|
| `--id` | Board ID (required) |
| `--state` | `future`, `active`, `closed` (comma-separated) |
| `--limit` / `--paginate` / `--json` / `--csv` | See [common-flags.md](common-flags.md) |

```bash
acli jira board search --project PROJECT-KEY --json
acli jira board list-sprints --id 1 --state active,closed
```

---

## sprint create / update / view / delete

**create**
| Flag | Description |
|------|-------------|
| `--name` | Sprint name (required) |
| `--board` | Origin board ID (required) |
| `--start` / `--end` | ISO 8601 dates (e.g. `2025-01-14` or `2025-01-14T09:00:00Z`) |
| `--goal` | Sprint goal |
| `--json` | JSON output |

**update**
| Flag | Description |
|------|-------------|
| `--id` | Sprint ID (required) |
| `--name` / `--goal` / `--state` | `--state`: `future`, `active`, `closed` |
| `--start` / `--end` / `--complete-date` | ISO 8601 dates |
| `--board` / `--json` | Board ID / JSON output |

**view**: `--id <sprint-id> [--json]`

**delete — DESTRUCTIVE**: deletes one or more sprints. `--id` comma-separated IDs (required), `--yes` to skip confirmation.

```bash
acli jira sprint create --name "Sprint 1" --board 5 --start 2025-01-01 --end 2025-01-14
acli jira sprint update --id 37 --state closed
acli jira sprint view --id 123 --json
acli jira sprint delete --id 37,42                  # confirm first
```

## sprint list-workitems

| Flag | Description |
|------|-------------|
| `--sprint` | Sprint ID (required) |
| `--board` | Board ID (required) |
| `--fields` | Comma-separated fields (default `key,issuetype,summary,assignee,priority,status`) |
| `--jql` | Additional JQL filter |
| `--limit` / `--paginate` / `--json` / `--csv` | See [common-flags.md](common-flags.md) |

```bash
acli jira sprint list-workitems --sprint 1 --board 6 --json
```
