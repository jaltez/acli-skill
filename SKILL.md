---
name: acli
description: "Terminal reference for the Atlassian CLI (acli). Use when the user wants to drive Jira Cloud, Confluence Cloud, or Atlassian org administration from the command line: create/edit/search/transition work items, manage projects, boards, sprints, filters, dashboards and custom fields, manage Confluence spaces, blog posts and pages, administer org users, or use Rovo Dev. Covers the jira, confluence, admin, rovodev and feedback command groups with goal-first recipes, a compact command catalog, and an explicit safety model for secrets, destructive operations, and org-wide admin blast radius. Requires an authenticated acli binary already installed on the system."
license: MIT
compatibility: "Requires the acli binary (https://developer.atlassian.com/cloud/acli/guides/install-acli/) installed and authenticated. Optional env vars: API_TOKEN (Jira API token for non-interactive auth), API_KEY (Admin API key for org-admin commands). Interactive OAuth via `acli jira auth login --web` is the default."
metadata:
  author: javier
  version: "1.1"
  acli_version: ">=1.3.19"
  last_verified: "2026-10-13 (acli 1.3.23-stable)"
---

# Atlassian CLI (`acli`) — agent guide

This skill helps you construct correct, safe `acli` commands. It does **not** bundle the
binary — the user must have `acli` installed and authenticated. Work goal-first: pick the
command from the recipe map, then pull exact flags from the linked `references/` file.

Canonical reference: https://developer.atlassian.com/cloud/acli/reference/commands/

## Prerequisites & authentication

Always verify availability before running anything:

```bash
acli --help            # binary present?
acli jira auth status  # jira authed?
acli admin auth status # admin authed?
```

If `acli` is missing, point the user to the install guide:
https://developer.atlassian.com/cloud/acli/guides/install-acli/

Three auth methods:

**OAuth (interactive, default):**
```bash
acli jira auth login --web
```

**API token (non-interactive / CI) — read from env, never inline:**
```bash
echo "$API_TOKEN" | acli jira auth login --site "example.atlassian.net" --email "you@example.com" --token
```

**Admin API key (admin commands only):**
```bash
echo "$API_KEY" | acli admin auth login --email "you@example.com" --token
```

Switch accounts with `acli jira auth switch` / `acli admin auth switch`.

Full `jira auth` flags: [`references/jira-auth.md`](references/jira-auth.md).

## Choose by goal

Find the goal, use the one-liner, then open the linked reference for full flags.

### I want to create something
```bash
acli jira workitem create --summary "..." --project PROJECT-KEY --type Task        # references/jira-workitem.md
acli jira workitem create-bulk --from-json issues.json                              # references/jira-workitem.md
acli jira project create --key NEW --name "New Project"                             # references/jira-project-board-sprint.md
```

### I want to find / list
```bash
acli jira workitem search --jql "project = PROJECT-KEY" --json                     # references/jira-workitem.md
acli jira workitem view WORK-123                                                    # references/jira-workitem.md
acli jira project list --paginate                                                  # references/jira-project-board-sprint.md
acli jira board search --project PROJECT-KEY                                       # references/jira-project-board-sprint.md
acli jira filter search --name "report"                                            # references/jira-filter-dashboard-field.md
acli jira dashboard search                                                         # references/jira-filter-dashboard-field.md
```

### I want to update / assign / comment
```bash
acli jira workitem edit --key WORK-123 --summary "..."                             # references/jira-workitem.md
acli jira workitem assign --key WORK-123 --assignee "@me"                          # references/jira-workitem.md
acli jira workitem comment create --key WORK-123 --body "..."                      # references/jira-workitem.md
acli jira project update --project-key PROJECT-KEY --name "..."                    # references/jira-project-board-sprint.md
```

### I want to move / link / organize
```bash
acli jira workitem transition --key WORK-123 --status "In Progress"               # references/jira-workitem.md
acli jira workitem link create --out WORK-1 --in WORK-2 --type Blocks             # references/jira-workitem.md
acli jira board list-sprints --id 1 --state active                                 # references/jira-project-board-sprint.md
acli jira sprint list-workitems --sprint 1 --board 6                               # references/jira-project-board-sprint.md
```

### I want to manage boards or sprints
```bash
acli jira board create --name "..." --type scrum --location-type project --project PROJ  # references/jira-project-board-sprint.md
acli jira board delete --id 12,34                                            (confirm)  # references/jira-project-board-sprint.md
acli jira sprint create --name "Sprint 1" --board 5 --start 2025-01-01 --end 2025-01-14   # references/jira-project-board-sprint.md
acli jira sprint update --id 37 --state closed                                          # references/jira-project-board-sprint.md
acli jira sprint delete --id 37,42                                            (confirm)  # references/jira-project-board-sprint.md
```

### I want to work in Confluence
```bash
acli confluence space list --json                                                    # references/confluence.md
acli confluence space create --key SPACEKEY --name "Team Space"                      # references/confluence.md
acli confluence space view --id 123456                                               # references/confluence.md (view is by --id)
acli confluence blog create --space-id 12345 --title "..." --body "<p>...</p>"       # references/confluence.md
acli confluence page view --id 123456789                                             # references/confluence.md
```

### I want to administer the org (HIGH blast radius — see Safety)
```bash
acli admin user activate --email you@example.com                                   # references/admin.md
acli admin user deactivate --email you@example.com                                 # references/admin.md  (confirm + enumerate)
acli admin user delete --email you@example.com                                     # references/admin.md  (destructive)
```

### I want Rovo Dev or to send feedback
```bash
acli rovodev auth login                                                            # references/rovodev.md
acli feedback --summary "..." --details "..." --email you@example.com             # references/rovodev.md
```

## Command catalog

One line per command. Open the linked file for flags and examples.

**jira auth** → [`references/jira-auth.md`](references/jira-auth.md)
`login` / `logout` / `status` / `switch`

**jira workitem** → [`references/jira-workitem.md`](references/jira-workitem.md)
`create` · `create-bulk` · `edit` · `view` · `search` · `assign` · `transition` · `delete` · `clone` · `archive` · `unarchive` · `comment (create/update/delete/list/visibility)` · `link (create/delete/list/type)` · `attachment (list/delete)` · `list-watchers` · `watcher remove` (`watcher list` deprecated → `list-watchers`)

**jira project / board / sprint** → [`references/jira-project-board-sprint.md`](references/jira-project-board-sprint.md)
`project (list/view/create/update/delete/archive/restore)` · `board (create/view/delete/list-projects/search/list-sprints)` (`get` deprecated → `view`) · `sprint (create/view/update/delete/list-workitems)`

**jira filter / dashboard / field** → [`references/jira-filter-dashboard-field.md`](references/jira-filter-dashboard-field.md)
`filter (list/search/view/update/add-favourite/change-owner/list-columns/reset-columns)` (`get` deprecated → `view`, `get-columns` deprecated → `list-columns`) · `dashboard (search)` · `field (create/update/delete/restore)` (`cancel-delete` deprecated → `restore`)

**confluence** → [`references/confluence.md`](references/confluence.md)
`auth (login/logout/status/switch)` · `space (list/view/create/update/archive/restore)` · `blog (create/list/view)` · `page (view)`

**admin** → [`references/admin.md`](references/admin.md)
`auth (login/logout/status/switch)` · `user (activate/deactivate/delete/cancel-delete)`

**rovodev / feedback** → [`references/rovodev.md`](references/rovodev.md)
`rovodev auth (login/logout/status)` · `rovodev run` (interactive agent) · `feedback`

Shared flags (`--json`, `--csv`, `--limit`, `--paginate`, `--yes`, `--ignore-errors`, bulk targeting):
[`references/common-flags.md`](references/common-flags.md)

## Safety checklist (read before destructive or admin commands)

Apply these before running. Details and rationale: [`references/common-flags.md`](references/common-flags.md) and each command's callout.

1. **Secrets** — tokens/keys come from env vars (`$API_TOKEN`, `$API_KEY`) or stdin (`< token.txt`). Never hardcode, echo, or log them. Prefer OAuth (`--web`) for interactive use.
2. **Irreversible deletes** — `jira workitem delete`, `jira project delete`, `jira field delete`, `admin user delete`: **confirm in the current turn** even if `--yes` is available.
3. **Bulk targeting** — before any destructive/impactful command using `--jql`/`--filter`/`--from-file`/comma `--key`, first run a `search` with the same query and show the affected set. Do not add `--yes` to destructive bulk runs unless the user explicitly wants unattended execution.
4. **Reversible-but-impactful** — `archive`/`unarchive`, `project restore`, `admin user cancel-delete`, `field restore`: lighter confirmation; note they are reversible.
5. **Org-wide admin tier** — `acli admin user *` is org-scoped, not project-scoped:
   - Require **fresh confirmation this turn**; earlier approval does not carry over.
   - **Enumerate targets first**: resolve `--email`/`--id`/`--from-file` to a concrete list (count + entries) and show it.
   - **Never** combine `--ignore-errors` with `admin user delete` or `admin user deactivate`.

## Command shape

```
acli <group> [<subgroup> ...] <command> {required flags} [optional flags]
```

Groups: `jira` (Cloud), `confluence` (Cloud), `admin` (org), `rovodev` (AI agent, Beta), `feedback`.
Other top-level commands: `auth` (router), `config`, `completion` (shell completions).

## Global commands

`acli auth` is a **global** OAuth router that authenticates/switches **all products at once**
(separate from the per-product `jira auth` / `confluence auth` / `admin auth` / `rovodev auth`):

```bash
acli auth login        # global OAuth (browser)
acli auth status       # show global account status
acli auth switch [--site X --email Y]   # switch all products at once
acli auth logout       # log out of all active OAuth accounts
```

`acli config gov-cloud` toggles the Atlassian Government Cloud environment
(`--enable` / `--enable=false` / `--status`). Note GovCloud support is limited.
