# confluence

Confluence Cloud commands: `auth`, `space`, `blog`, `page`.

Canonical reference: https://developer.atlassian.com/cloud/acli/reference/commands/
> Confluence commands ship in `acli` ≥ v1.3 but the official reference page lags the binary. If a
> flag here differs from your installed version, trust `acli confluence <command> --help`.

Shared flags: [`common-flags.md`](common-flags.md).

---

## auth login / logout / status / switch

Authenticate for Confluence (separate from Jira/admin auth).

**login**
| Flag | Short | Description |
|------|-------|-------------|
| `--email` | `-e` | User email (required for token auth) |
| `--site` | `-s` | Atlassian site, e.g. `example.atlassian.net` (required for token auth) |
| `--token` | | Read API token from stdin |
| `--web` | `-w` | Authenticate via browser (OAuth) |

```bash
acli confluence auth login --web
echo "$API_TOKEN" | acli confluence auth login --site "example.atlassian.net" --email "you@example.com" --token
```

**logout** / **status** / **switch**: standard — see the auth pattern in [`admin.md`](admin.md).

---

## space list / view

**list**
| Flag | Short | Description |
|------|-------|-------------|
| `--keys` | | Comma-separated space keys to filter by |
| `--type` | | `global` or `personal` |
| `--status` | | `current` (default) or `archived` |
| `--expand` | | `description,homepage,permissions` |
| `--limit` | `-l` | Max spaces (default 50) |
| `--json` | | JSON output |

**view — by ID, not key**
| Flag | Description |
|------|-------------|
| `--id` | Space ID (required) |
| `--icon` / `--labels` / `--permissions` / `--operations` / `--properties` / `--role-assignments` | Include extra details |
| `--include-all` | Include all extras |
| `--desc-format` | `plain` or `view` |
| `--json` | JSON output |

> `view` takes `--id`; `create`/`update`/`archive`/`restore` take `--key`. Find the ID first with
> `space list --keys SPACEKEY --json`.

```bash
acli confluence space list --json
acli confluence space list --status archived
acli confluence space view --id 123456 --json
```

## space create / update

**create**
| Flag | Description |
|------|-------------|
| `--key` | Space key (required) |
| `--name` | Space name (required) |
| `--description` | Description |
| `--alias` | Identifier used in page URLs |
| `--private` | Create as private |
| `--template-key` | Template to use |
| `--json` | JSON output |

**update**
| Flag | Description |
|------|-------------|
| `--key` | Space key to update (required) |
| `--name` / `--description` / `--status` / `--type` | Fields to change |
| `--json` | JSON output |

```bash
acli confluence space create --key SPACEKEY --name "Team Space"
acli confluence space create --key SPACEKEY --name "Team Space" --description "..." --private
acli confluence space update --key SPACEKEY --name "Renamed Space"
```

## space archive / restore — reversible

Archive a space by key; restore brings it back. Reversible-but-impactful (see safety tier 4).

```bash
acli confluence space archive --key SPACEKEY
acli confluence space restore --key SPACEKEY
```

---

## blog create / list / view

**create**
| Flag | Short | Description |
|------|-------|-------------|
| `--space-id` | | Space ID (required) |
| `--title` | | Title (required) |
| `--body` | | Content in Confluence storage format (XHTML) |
| `--from-file` | | Read body from file (plain text or HTML) |
| `--from-json` / `--generate-json` | | JSON payload support |
| `--status` | | `current` (published, default) or `draft` |
| `--private` | | Create as private |
| `--created-at` | | ISO 8601 timestamp |
| `--json` | `-j` | JSON output |

**list**
| Flag | Short | Description |
|------|-------|-------------|
| `--space-id` | | Comma-separated space IDs |
| `--id` | | Comma-separated blog post IDs |
| `--status` | | `current`, `deleted`, `trashed` (comma list) |
| `--title` | | Title filter |
| `--body-format` | | `storage`, `atlas_doc_format`, ... |
| `--sort` | | Sort order |
| `--cursor` | | Pagination cursor from a previous call |
| `--limit` | `-l` | Max per page (default 25) |
| `--json` | `-j` / `--csv` | Output |

**view**
| Flag | Short | Description |
|------|-------|-------------|
| `--id` | | Blog post ID (required) |
| `--body-format` | | Body representation |
| `--version` | | Specific version number |
| `--draft` | | Retrieve the draft version |
| `--include` | | `labels,properties,operations,likes,versions,...,all` |
| `--status` | | `current`, `trashed`, `deleted`, `historical`, `draft` |
| `--json` | `-j` | JSON output |

```bash
acli confluence blog create --space-id 12345 --title "Release Notes" --body "<p>...</p>"
acli confluence blog list --space-id 12345 --status current --json
acli confluence blog view --id 98765 --json
```

---

## page view

Retrieve a page by ID. (No create/edit/delete in this group — view only.)

| Flag | Description |
|------|-------------|
| `--id` | Page ID (required) |
| `--body-format` | `storage`, `atlas_doc_format`, `view` |
| `--version` | Specific version number |
| `--status` | Comma list: `current`, `draft`, `archived` |
| `--get-draft` | Return the draft version if accessible |
| `--include-labels` / `--include-versions` / `--include-version` / `--include-operations` / `--include-properties` / `--include-likes` / `--include-collaborators` / `--include-direct-children` / `--include-webresources` / `--include-favorited-by-current-user-status` | Include extra details |
| `--json` | JSON output |

```bash
acli confluence page view --id 123456789 --json
acli confluence page view --id 123456789 --body-format storage --version 2
```
