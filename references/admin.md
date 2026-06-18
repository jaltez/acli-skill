# admin

Organization administration. Canonical references:
- admin auth: https://developer.atlassian.com/cloud/acli/reference/commands/admin-auth/
- admin user: https://developer.atlassian.com/cloud/acli/reference/commands/admin-user/

> **ORG-WIDE BLAST RADIUS.** Every command here affects the whole Atlassian organization, not a
> single project or site. Apply the admin tier of the safety checklist before running anything
> in this file:
> 1. **Fresh confirmation this turn** — approval for an earlier admin command does not carry over.
> 2. **Enumerate targets first** — resolve `--email`/`--id`/`--from-file` to a concrete list
>    (count + each entry) and show it to the user.
> 3. **Never** combine `--ignore-errors` with `admin user delete` or `admin user deactivate`.

Shared flags: [`common-flags.md`](common-flags.md).

---

## auth login / logout / status / switch

**login** (requires an org API key)
| Flag | Short | Description |
|------|-------|-------------|
| `--email` | `-e` | Admin email (required) |
| `--token` | | Read API key from stdin |

```bash
echo "$API_KEY" | acli admin auth login --email you@example.com --token
# or, avoiding shell history:
acli admin auth login --email you@example.com --token < key.txt
```

**logout**: `acli admin auth logout`
**status**: `acli admin auth status`
**switch**: `--org <org-name>` (`-o`)

---

## user activate / deactivate / delete / cancel-delete

All `admin user` commands share these targeting flags:

| Flag | Short | Description |
|------|-------|-------------|
| `--email` | `-e` | Comma-separated emails |
| `--id` | | Comma-separated account IDs |
| `--from-file` | `-f` | Read emails/IDs from file |
| `--ignore-errors` | | Continue past failures (forbidden with `delete`/`deactivate`) |
| `--json` | | JSON output |

**activate** (reversible)
```bash
acli admin user activate --email you@example.com,teammate@example.com
acli admin user activate --from-file users.txt
```

**deactivate — CAUTION (reversible):** revokes access; reverse with `activate`. Confirm and enumerate first.
```bash
acli admin user deactivate --id 5b10ac8d82e05b22cc7d4ef5
```

**delete — DESTRUCTIVE (irreversible):** deletes managed user accounts. Confirm in the current
turn, enumerate targets, and **do not** add `--ignore-errors`. Pending deletions can be cancelled
with `cancel-delete`.
```bash
acli admin user delete --email stale-account@example.com
```

**cancel-delete**: cancel a pending account deletion.
```bash
acli admin user cancel-delete --email stale-account@example.com
```
