# acli — Atlassian CLI agent skill

An importable agent skill that teaches AI coding agents how to drive the
[Atlassian CLI (`acli`)](https://developer.atlassian.com/cloud/acli/guides/introduction/) from
the terminal: Jira Cloud work items, projects, boards, sprints, filters, dashboards, and custom
fields, Confluence Cloud spaces, blog posts and pages, plus Atlassian organization administration
and Rovo Dev.

This is a **reference skill** — static markdown only. It does not bundle or install the `acli`
binary.

## What's inside

```
SKILL.md                                      Goal-first guide + command catalog + safety checklist
references/
├── common-flags.md                           Shared output/pagination/automation/bulk-targeting flags
├── jira-workitem.md                          create/edit/search/transition/comment/link/...
├── jira-project-board-sprint.md              project / board / sprint
├── jira-filter-dashboard-field.md            filter / dashboard / field
├── confluence.md                             confluence auth / space / blog / page
├── admin.md                                  org auth + user management (org-wide blast radius)
└── rovodev.md                                rovodev auth + feedback
README.md
LICENSE
```

## Prerequisites

The binary is **not** bundled. You must:

1. **Install acli** — follow the [official install guide](https://developer.atlassian.com/cloud/acli/guides/install-acli/).
2. **Authenticate** — OAuth, API token, or admin API key:
   ```bash
   # Interactive (browser-based) — recommended
   acli jira auth login --web

   # API token (non-interactive / CI) — read from env, never inline
   echo "$API_TOKEN" | acli jira auth login --site "example.atlassian.net" --email "you@example.com" --token
   ```
3. **Verify** — `acli jira auth status`

Optional environment variables: `API_TOKEN` (Jira API token) and `API_KEY` (Admin API key) for
non-interactive use. Interactive OAuth (`--web`) is the default and needs neither.

## Install / import

The skill is a folder named `acli` containing `SKILL.md`. Clone or symlink it into any of the
discovery roots below. It works with **OpenCode**, **Claude Code**, and **GitHub Copilot**.

### Global (every project)

```bash
# OpenCode
git clone <this-repo-url> ~/.config/opencode/skills/acli

# Claude Code
git clone <this-repo-url> ~/.claude/skills/acli

# GitHub Copilot
git clone <this-repo-url> ~/.copilot/skills/acli
```

### Single project

```bash
git clone <this-repo-url> .opencode/skills/acli      # OpenCode
# or .claude/skills/acli (Claude Code) / .agents/skills/acli (Copilot-compatible)
```

Update later with `git -C ~/.config/opencode/skills/acli pull` (or the path you chose).

> The imported folder **must** be named `acli` so it matches the `name` field in `SKILL.md`.

## Usage

Once imported, just ask your agent in natural language:

- *"Create a bug in PROJECT-KEY about the login timeout"*
- *"Show my in-progress issues"*
- *"Transition WORK-123 to Done"*
- *"List the active sprints on board 1"*
- *"Create a Confluence space TEAM with name 'Engineering'"*
- *"Deactivate stale-account@example.com"* (the agent will confirm and enumerate targets first)

The agent loads `SKILL.md` on demand, picks the right command from the goal map, and pulls exact
flags from the linked `references/` file.

## Security

- Token/keys are referenced via environment variables or stdin redirection only — never inlined.
- Destructive commands (`workitem delete`, `project delete`, `admin user delete`, `field delete`)
  require explicit confirmation in the current turn.
- Bulk destructive runs must be previewed with a `search` using the same query first.
- Org-wide `admin user *` commands require fresh per-turn confirmation and target enumeration, and
  must never run with `--ignore-errors` for `delete`/`deactivate`.

## References

- [Atlassian CLI introduction](https://developer.atlassian.com/cloud/acli/guides/introduction/)
- [Install acli](https://developer.atlassian.com/cloud/acli/guides/install-acli/)
- [Command reference](https://developer.atlassian.com/cloud/acli/reference/commands/)

## License

MIT — see [LICENSE](LICENSE).
