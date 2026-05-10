---
name: bitwarden-credentials
description: Use Bitwarden CLI with Vaultwarden/Bitwarden to retrieve API tokens, SSH keys, passwords, and other credentials when a task needs a secret and the user has not provided it. Trigger when the user asks to use Cloudflare, GitHub, RainYun, server, database, or API credentials and the required token/password/key is missing.
---

# Bitwarden Credential Retrieval

Use this skill whenever a task needs a credential and the user has not already provided it.

## Policy

- Never ask the user to paste a secret before checking whether Bitwarden CLI is available.
- Never print a secret, token, password, private key, or full `bw get item` JSON to the user.
- Prefer short-lived environment variables for command execution.
- Do not save retrieved secrets to repo files, shell profiles, logs, or chat.
- If a command may echo environment variables or verbose HTTP headers, mask or avoid that output.
- If Bitwarden is unavailable, locked, or unauthenticated, tell the user the exact next command to run.

## Quick Workflow

1. Check whether Bitwarden CLI exists:

```bash
bw --version
```

If missing, tell the user to install it:

```bash
npm install -g @bitwarden/cli
```

2. Check login/unlock status:

```bash
bw status --raw
```

Interpret status:

- `unauthenticated`: ask the user to run `bw login --server <vault-url>` or `bw login`
- `locked`: ask the user to run `bw unlock` and export/set `BW_SESSION`
- `unlocked`: continue

3. Search likely items by service name:

```bash
bw list items --search "cloudflare" --session "$BW_SESSION"
bw list items --search "github" --session "$BW_SESSION"
bw list items --search "rainyun" --session "$BW_SESSION"
```

On Windows PowerShell, use:

```powershell
bw list items --search "cloudflare" --session $env:BW_SESSION
```

4. Retrieve only the needed secret field.

For a password-style item:

```bash
bw get password "<item name or id>" --session "$BW_SESSION"
```

For custom fields, get the item JSON and parse locally, without displaying it:

```bash
bw get item "<item name or id>" --session "$BW_SESSION"
```

Use a structured parser (`jq`, PowerShell JSON conversion, Node, or Python) to extract the field.

## Common Field Names

Prefer these item names and fields when they exist:

- Cloudflare DNS token:
  - item: `Cloudflare - yueyanc.cn DNS Token`
  - fields: `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ZONE`
- GitHub token:
  - item: `GitHub - Yueyanc PAT`
  - fields: `GITHUB_TOKEN`, `GH_TOKEN`
- RainYun API key:
  - item: `RainYun API Key`
  - fields: `RAINYUN_API_KEY`
- Server SSH/root credential:
  - item contains server IP or provider name
  - fields: `SSH_PRIVATE_KEY`, `ROOT_PASSWORD`, `SSH_USER`, `SSH_HOST`

If multiple matching items exist, ask the user which item name to use. Do not reveal item secrets.

## Using Retrieved Secrets

For a single command, inject the secret into that command's environment only.

PowerShell example:

```powershell
$token = bw get password "Cloudflare - yueyanc.cn DNS Token" --session $env:BW_SESSION
$env:CLOUDFLARE_API_TOKEN = $token
# run the needed command
Remove-Item Env:\CLOUDFLARE_API_TOKEN
```

Bash example:

```bash
CLOUDFLARE_API_TOKEN="$(bw get password "Cloudflare - yueyanc.cn DNS Token" --session "$BW_SESSION")" command-that-needs-it
```

When using a retrieved token in scripts, ensure the script does not log request headers or environment values.

## User-Facing Recovery Messages

If `bw` is missing:

```text
I need a credential, but Bitwarden CLI is not installed. Install it with `npm install -g @bitwarden/cli`, then run `bw login --server <your Vaultwarden URL>`.
```

If unauthenticated:

```text
Bitwarden CLI is installed but not logged in. Run `bw login --server <your Vaultwarden URL>`, then ask me to continue.
```

If locked:

```text
Bitwarden is locked. Run `bw unlock` and set the printed `BW_SESSION` value in this terminal, then ask me to continue.
```

If the item is missing:

```text
I could not find a Bitwarden item for <service>. Create one named `<suggested item name>` with the needed token, then ask me to continue.
```

