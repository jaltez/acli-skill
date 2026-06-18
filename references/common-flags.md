# Common flags

Shared flags that recur across many `acli` commands. Referenced from the main guide.

Canonical reference: https://developer.atlassian.com/cloud/acli/reference/commands/

## Output format

| Flag | Description |
|------|-------------|
| `--json` | Emit JSON. Best for piping/verification before destructive changes. |
| `--csv` | Emit CSV (list/search commands). |

Default output is a human-readable table.

## Pagination

| Flag | Description |
|------|-------------|
| `--limit N` | Max items to return (defaults vary, ~30–50). |
| `--paginate` | Fetch every page automatically (overrides `--limit`). |

## Automation

| Flag | Description |
|------|-------------|
| `--yes`, `-y` | Skip confirmation prompts. Do **not** combine with destructive bulk runs unless the user explicitly wants unattended execution. |
| `--ignore-errors` | Continue past failures in a bulk run. Never combine with `admin user delete` / `admin user deactivate`. |

## Bulk targeting

Many edit/assign/transition/delete commands accept more than one target. The same targeting
inputs repeat across commands:

| Flag | Description |
|------|-------------|
| `--key "K-1,K-2,K-3"` | Comma-separated work-item keys. |
| `--jql "project = PROJECT-KEY AND ..."` | JQL query selecting the target set. |
| `--filter 10001` | Saved filter ID. |
| `--from-file items.txt` | File of keys/IDs (comma, whitespace, or newline separated). |

> **Bulk safety rule:** before any destructive or impactful command that uses `--jql`,
> `--filter`, `--from-file`, or a multi-value `--key`, first run a `search` with the **same**
> query and show the user the affected set. Confirm, then execute. Prefer `--json` for the
> preview so the count and members are unambiguous.

## JSON templates (create/edit)

```bash
acli jira workitem create --generate-json > template.json   # scaffold
# ...edit template.json...
acli jira workitem create --from-json template.json         # consume
```
