# rovodev / feedback

Canonical references:
- rovodev: https://developer.atlassian.com/cloud/acli/reference/commands/rovodev/
- rovodev auth: https://developer.atlassian.com/cloud/acli/reference/commands/rovodev-auth/
- feedback: https://developer.atlassian.com/cloud/acli/reference/commands/feedback/

Rovo Dev is Atlassian's AI coding agent (Beta). Shared flags: [`common-flags.md`](common-flags.md).

---

## rovodev auth login / logout / status

**login**
| Flag | Short | Description |
|------|-------|-------------|
| `--email` | `-e` | User email |
| `--token` | | Read token from stdin |

```bash
acli rovodev auth login
acli rovodev auth login --email you@example.com --token < token.txt
```

**logout**: `acli rovodev auth logout`
**status**: `acli rovodev auth status`

---

## rovodev run

Launches the Rovo Dev AI coding agent (interactive, Beta). Not scriptable — starts an interactive
session, so do not run it inside automation pipelines. Requires prior `rovodev auth login`.

```bash
acli rovodev run
```

---

## feedback

Submit a request or report a problem.

| Flag | Short | Description |
|------|-------|-------------|
| `--summary` | `-s` | Short summary |
| `--details` | `-d` | Details |
| `--email` | `-e` | Email for a response |
| `--attachments` | `-a` | Files to attach |
| `--time` | `-t` | When it occurred (e.g. `1h`, `15m`) |

```bash
acli feedback --summary "Unexpected auth error" --details "Token expired mid-session" --email you@example.com
```
