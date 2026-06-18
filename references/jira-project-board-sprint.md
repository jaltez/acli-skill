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
| `--lead-email` | `-l` | Project lead |
| `--from-json` / `--generate-json` | | JSON template support |

**update**
| Flag | Short | Description |
|------|-------|-------------|
| `--project-key` | `-p` | Key of project to update |
| `--key` | `-k` | New key |
| `--name` | `-n` / `--description` `-d` / `--lead-email` `-l` / `--url` `-u` | Fields |
| `--from-json` / `--generate-json` | | JSON template support |

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

## board search / list-sprints

**search**
| Flag | Description |
|------|-------------|
| `--name` | Partial name match |
| `--project` | Filter by project key |
| `--type` | `scrum`, `kanban`, `simple` |
| `--orderBy` | `name`, `+name`, `-name` |
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

## sprint list-workitems

| Flag | Description |
|------|-------------|
| `--sprint` | Sprint ID (required) |
| `--board` | Board ID (required) |
| `--fields` | Comma-separated fields |
| `--jql` | Additional JQL filter |
| `--limit` / `--paginate` / `--json` / `--csv` | See [common-flags.md](common-flags.md) |

```bash
acli jira sprint list-workitems --sprint 1 --board 6 --json
```
