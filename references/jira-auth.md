# jira auth

Jira Cloud authentication. Separate from the global `acli auth` router (which authenticates all
products at once) and from `confluence` / `admin` / `rovodev` auth.

Canonical reference: https://developer.atlassian.com/cloud/acli/reference/commands/jira-auth/
> The official reference page can lag the binary. If a flag here differs from your installed
> version, trust `acli jira auth login --help`.

---

## login

| Flag | Short | Description |
|------|-------|-------------|
| `--web` | `-w` | Authenticate via browser (OAuth — interactive default) |
| `--email` | `-e` | User email (required for token auth) |
| `--site` | `-s` | Atlassian site, e.g. `example.atlassian.net` (required for token auth) |
| `--token` | | Read API token from stdin |

```bash
acli jira auth login --web
echo "$API_TOKEN" | acli jira auth login --site "example.atlassian.net" --email "you@example.com" --token
acli jira auth login --site "example.atlassian.net" --email "you@example.com" --token < token.txt
```

## logout / status / switch

**logout** / **status**: standard — no flags.

**switch**: select the account to switch to.

| Flag | Short | Description |
|------|-------|-------------|
| `--site` | `-s` | Atlassian account site |
| `--email` | `-e` | Atlassian account email |

```bash
acli jira auth status
acli jira auth switch
acli jira auth switch --site "example.atlassian.net" --email "you@example.com"
```
