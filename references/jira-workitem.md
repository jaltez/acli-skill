# jira workitem

Work-item (issue) commands. Canonical reference:
https://developer.atlassian.com/cloud/acli/reference/commands/jira-workitem/

Shared bulk-targeting flags (`--key`, `--jql`, `--filter`, `--from-file`) and output/automation
flags are documented in [`common-flags.md`](common-flags.md).

---

## create

Create one work item.

```
acli jira workitem create [flags]
```

| Flag | Short | Description |
|------|-------|-------------|
| `--summary` | `-s` | Title (required) |
| `--project` | `-p` | Project key |
| `--type` | `-t` | `Epic`, `Story`, `Task`, `Bug`, `Subtask`, ... |
| `--assignee` | `-a` | Email, account ID, `@me`, or `default` |
| `--description` | `-d` | Plain text or ADF |
| `--description-file` | | Read description (plain text or ADF) from file |
| `--editor` | `-e` | Open editor to specify summary + description |
| `--from-file` | `-f` | Read summary + description from file |
| `--label` | `-l` | Comma-separated labels |
| `--parent` | | Parent work-item key |
| `--from-json` | | Read fields from JSON file |
| `--generate-json` | | Print a JSON template |
| `--json` | | JSON output |

```bash
acli jira workitem create --summary "Fix login timeout" --project PROJECT-KEY --type Bug
acli jira workitem create --summary "Spike" --project PROJECT-KEY --type Task --assignee "@me" --label "exploration,p1"
acli jira workitem create --from-json workitem.json
```

## create-bulk

Bulk create from structured data.

| Flag | Description |
|------|-------------|
| `--from-csv` | CSV (columns: summary, projectKey, issueType, description, label, parentIssueId, assignee) |
| `--from-json` | JSON array of issue objects |
| `--generate-json` | Print example structure |
| `--ignore-errors` | Continue past failures |
| `--yes` | Skip confirmation |

```bash
acli jira workitem create-bulk --from-json issues.json
acli jira workitem create-bulk --from-csv issues.csv
```

## edit

Modify one or many work items.

| Flag | Short | Description |
|------|-------|-------------|
| `--key` | `-k` | Comma-separated keys (or use `--jql`/`--filter`) |
| `--summary` | `-s` | New title |
| `--description` | `-d` | New description |
| `--description-file` | | Read description (plain text or ADF) from file |
| `--assignee` | `-a` | Email / account ID / `@me` / `default` |
| `--remove-assignee` | | Unassign |
| `--type` | `-t` | Change type |
| `--labels` | `-l` | Set labels |
| `--remove-labels` | | Remove labels |
| `--filter` | | Filter ID of work items to edit |
| `--from-json` / `--generate-json` | | JSON load / template |
| `--ignore-errors` / `--yes` / `--json` | | See [common-flags.md](common-flags.md) |

```bash
acli jira workitem edit --key WORK-1,WORK-2 --summary "Updated title"
acli jira workitem edit --jql "project = PROJECT-KEY" --assignee "you@example.com"
```

## view

```
acli jira workitem view <key> [flags]
```

| Flag | Short | Description |
|------|-------|-------------|
| `--fields` | `-f` | Fields to return; supports `*all`, `*navigable`, `-field` to exclude |
| `--json` | | JSON output |
| `--web` | `-w` | Open in browser |

```bash
acli jira workitem view WORK-123
acli jira workitem view WORK-123 --fields "*all" --json
```

## search

JQL search.

| Flag | Short | Description |
|------|-------|-------------|
| `--jql` | `-j` | JQL query |
| `--filter` | | Saved filter ID |
| `--fields` | `-f` | Comma-separated fields |
| `--limit` | `-l` | Max results |
| `--paginate` | | Fetch all pages |
| `--count` | | Return only the count |
| `--json` / `--csv` / `--web` | | See [common-flags.md](common-flags.md) |

```bash
acli jira workitem search --jql "project = PROJECT-KEY AND assignee = currentUser()" --json
acli jira workitem search --jql "project = PROJECT-KEY AND status = 'In Progress'" --fields "key,summary,status" --csv
```

> Use `search` (with `--count` or `--json`) as the **preview** before any bulk destructive run
> targeting the same `--jql`/`--filter`.

## assign

| Flag | Short | Description |
|------|-------|-------------|
| `--key` | `-k` | Keys (or `--jql`/`--filter`/`--from-file`) |
| `--assignee` | `-a` | Email / account ID / `@me` / `default` |
| `--remove-assignee` | | Unassign |
| `--filter` | | Filter ID of work items to assign |
| `--from-file` | `-f` | Read keys/IDs from file |
| `--ignore-errors` / `--yes` / `--json` | | See [common-flags.md](common-flags.md) |

```bash
acli jira workitem assign --key WORK-1 --assignee "@me"
acli jira workitem assign --jql "project = PROJECT-KEY" --assignee "you@example.com"
```

## transition

| Flag | Short | Description |
|------|-------|-------------|
| `--key` | `-k` | Keys (or `--jql`/`--filter`) |
| `--status` | `-s` | Target status name |
| `--filter` | | Filter ID of work items to transition |
| `--ignore-errors` / `--yes` / `--json` | | See [common-flags.md](common-flags.md) |

```bash
acli jira workitem transition --key WORK-1,WORK-2 --status "Done"
acli jira workitem transition --jql "project = PROJECT-KEY AND sprint in openSprints()" --status "In Progress"
```

## delete  — DESTRUCTIVE (irreversible)

> Permanently deletes work items. **Confirm with the user in the current turn.** For bulk
> targets (`--jql`/`--filter`/`--from-file`/multi `--key`), run a `search` first and show the
> set. Do not add `--yes` to a destructive bulk run unless the user explicitly opts in.

| Flag | Short | Description |
|------|-------|-------------|
| `--key` | `-k` | Keys (or `--jql`/`--filter`/`--from-file`) |
| `--filter` | | Filter ID of work items to delete |
| `--from-file` | `-f` | Read keys/IDs from file |
| `--ignore-errors` / `--yes` / `--json` | | See [common-flags.md](common-flags.md) |

```bash
acli jira workitem delete --key WORK-1,WORK-2
acli jira workitem delete --jql "project = PROJECT-KEY AND status = Done"   # preview with search first!
```

## clone

| Flag | Short | Description |
|------|-------|-------------|
| `--key` | `-k` | Keys to clone (or `--jql`/`--filter`/`--from-file`) |
| `--to-project` | | Target project key |
| `--to-site` | | Target site (defaults to current) |
| `--filter` | | Filter ID of work items to clone |
| `--from-file` | `-f` | Read keys/IDs from file |
| `--jql` | | JQL query for work items to clone |
| `--ignore-errors` / `--yes` / `--json` | | See [common-flags.md](common-flags.md) |

```bash
acli jira workitem clone --key WORK-1,WORK-2 --to-project OTHER-KEY
```

## archive / unarchive  — reversible

Archive makes a work item read-only; `unarchive` restores it. Lighter confirmation; note reversibility.

| Flag | Short | Description |
|------|-------|-------------|
| `--key` | `-k` | Keys (or, for `archive`: `--jql`/`--filter`/`--from-file`) |
| `--from-file` | `-f` | Read keys/IDs from file (both `archive` and `unarchive`) |
| `--ignore-errors` / `--yes` / `--json` | | See [common-flags.md](common-flags.md) |

> `archive` accepts `--jql`/`--filter`; `unarchive` does **not** — it only targets `--key`/`--from-file`.

```bash
acli jira workitem archive --key WORK-1,WORK-2
acli jira workitem unarchive --key WORK-1,WORK-2
```

## comment create / update / delete / list / visibility

**create**
| Flag | Short | Description |
|------|-------|-------------|
| `--key` | `-k` | Keys (or `--jql`/`--filter`) |
| `--body` | `-b` | Comment text (plain or ADF) |
| `--body-file` | `-F` | Read body from file |
| `--editor` / `--edit-last` | | Open editor / amend your latest |
| `--ignore-errors` / `--json` | | See [common-flags.md](common-flags.md) |

**update**
| Flag | Description |
|------|-------------|
| `--key`, `--id` | Work item key + comment ID |
| `--body` / `--body-file` / `--body-adf` | New body |
| `--visibility-group` / `--visibility-role` | Restrict visibility |
| `--notify` | Notify watchers |

**list**
| Flag | Description |
|------|-------------|
| `--key` | Work item key (required) |
| `--limit` / `--paginate` / `--order` | `--order`: `created`, `+created`, `-created` |
| `--json` | JSON output |

**delete**: `--key <key> --id <comment-id>`
**visibility**: `--role --project PROJECT-KEY` or `--group`

```bash
acli jira workitem comment create --key WORK-1 --body "Looks good"
acli jira workitem comment list --key WORK-123 --json
acli jira workitem comment update --key WORK-1 --id 10001 --body "Updated" --visibility-role "Administrators"
```

## link create / delete / list / type

**create**
| Flag | Description |
|------|-------------|
| `--out` / `--in` | Outward / inward work-item IDs |
| `--type` | Link type: `Blocks`, `Cloners`, `Duplicate`, ... |
| `--from-json` / `--from-csv` / `--generate-json` | Bulk link mapping |
| `--ignore-errors` / `--yes` | See [common-flags.md](common-flags.md) |

**delete**: `--id <link-id>` (+ `--from-json`/`--from-csv`, `--ignore-errors`, `--yes`)
**list**: `--key <key> [--json]`
**type**: list available link types `[--json]`

```bash
acli jira workitem link create --out WORK-1 --in WORK-2 --type Blocks
acli jira workitem link type --json
```

## attachment list / delete

**list**: `--key <key> [--json]`
**delete — DESTRUCTIVE**: `--id <attachment-id>` (confirm first)

## list-watchers / watcher remove

List or remove watchers.

> `acli jira workitem watcher list` is **DEPRECATED** (removed 2026-12-01) — use `list-watchers`.

```bash
acli jira workitem list-watchers --key WORK-1                    # list (preferred)
acli jira workitem watcher list   --key WORK-1                   # DEPRECATED — use list-watchers
acli jira workitem watcher remove --key WORK-1 --user 5b10ac8d82e05b22cc7d4ef5
```
