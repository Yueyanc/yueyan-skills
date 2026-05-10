---
name: infisical-credentials
description: Use Infisical CLI or API to retrieve missing credentials such as Cloudflare, GitHub, RainYun, server, database, and API tokens. Trigger when a task needs a secret and the user has not provided it, or when the user asks how to configure AI agents to access credentials through Infisical.
---

# Infisical Credential Retrieval

Use this skill whenever a task needs a credential and the user has not already provided it.

## Core Rules

- Prefer Infisical as the source of truth for secrets.
- Never ask the user to paste a secret before checking whether Infisical access is already configured.
- Never print tokens, passwords, private keys, full secret values, or full secret JSON to the user.
- Use short-lived access tokens and command-scoped environment variables when possible.
- Do not write retrieved secrets into repo files, shell profiles, logs, command history, or chat.
- If a command may echo environment variables, HTTP headers, or verbose debug data, mask or avoid that output.
- If Infisical is not configured, guide the user to inject the minimum environment variables needed.

## Expected Environment

Check these first:

```bash
INFISICAL_API_URL
INFISICAL_CLIENT_ID
INFISICAL_CLIENT_SECRET
```

`INFISICAL_API_URL` should point to the user's Infisical instance, for example:

```text
https://secrets.example.com
```

`INFISICAL_CLIENT_ID` and `INFISICAL_CLIENT_SECRET` should belong to a Machine Identity with the smallest read-only scope that can satisfy the task.

## Quick Workflow

1. Check whether Infisical CLI exists:

```bash
infisical --version
```

If missing, tell the user how to install it for their OS:

```powershell
scoop bucket add org https://github.com/Infisical/scoop-infisical.git
scoop install infisical
```

```bash
brew install infisical/get-cli/infisical
```

2. Check whether the required environment variables are present.

PowerShell:

```powershell
$env:INFISICAL_API_URL
$env:INFISICAL_CLIENT_ID
$env:INFISICAL_CLIENT_SECRET
```

Bash:

```bash
printenv INFISICAL_API_URL INFISICAL_CLIENT_ID INFISICAL_CLIENT_SECRET
```

3. If they are missing, give the user copy-paste setup commands. Do not invent secret values.

PowerShell user-level persistence:

```powershell
[Environment]::SetEnvironmentVariable("INFISICAL_API_URL", "https://secrets.example.com", "User")
[Environment]::SetEnvironmentVariable("INFISICAL_CLIENT_ID", "paste-client-id-here", "User")
[Environment]::SetEnvironmentVariable("INFISICAL_CLIENT_SECRET", "paste-client-secret-here", "User")
```

PowerShell current session only:

```powershell
$env:INFISICAL_API_URL="https://secrets.example.com"
$env:INFISICAL_CLIENT_ID="paste-client-id-here"
$env:INFISICAL_CLIENT_SECRET="paste-client-secret-here"
```

Bash current session only:

```bash
export INFISICAL_API_URL="https://secrets.example.com"
export INFISICAL_CLIENT_ID="paste-client-id-here"
export INFISICAL_CLIENT_SECRET="paste-client-secret-here"
```

4. Login with Universal Auth and keep the token out of output.

PowerShell:

```powershell
$env:INFISICAL_TOKEN = infisical login --method=universal-auth --client-id="$env:INFISICAL_CLIENT_ID" --client-secret="$env:INFISICAL_CLIENT_SECRET" --silent --plain
```

Bash:

```bash
export INFISICAL_TOKEN="$(infisical login --method=universal-auth --client-id="$INFISICAL_CLIENT_ID" --client-secret="$INFISICAL_CLIENT_SECRET" --silent --plain)"
```

5. Read only the needed secret by name, project, environment, and path. Prefer exact names from the user's Infisical project conventions.

Example:

```bash
infisical secrets get CLOUDFLARE_API_TOKEN --plain --env=prod --path=/cloudflare
```

When the project id is required by the local Infisical CLI version or workspace layout, ask the user for the project or infer it from local configuration if available.

## API Fallback

If the CLI is missing and installing it is not appropriate, use the API directly:

```http
POST {INFISICAL_API_URL}/api/v1/auth/universal-auth/login
```

Body:

```json
{
  "clientId": "<INFISICAL_CLIENT_ID>",
  "clientSecret": "<INFISICAL_CLIENT_SECRET>"
}
```

Store the returned access token only in memory or in a short-lived process environment variable, then call the secrets API for the specific secret needed. Do not display the access token.

## Naming Conventions

Prefer these secret names and paths when they exist:

- Cloudflare:
  - path: `/cloudflare`
  - names: `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ZONE_ID`, `CLOUDFLARE_ACCOUNT_ID`
- GitHub:
  - path: `/github`
  - names: `GITHUB_TOKEN`, `GH_TOKEN`
- RainYun:
  - path: `/rainyun`
  - names: `RAINYUN_API_KEY`, `RAINYUN_API_SECRET`
- Servers:
  - path: `/servers/<name>`
  - names: `SSH_HOST`, `SSH_PORT`, `SSH_USER`, `SSH_PRIVATE_KEY`, `ROOT_PASSWORD`, `HOSTKEY_ED25519_SHA256`
- Databases:
  - path: `/databases/<name>`
  - names: `DATABASE_URL`, `DB_HOST`, `DB_USER`, `DB_PASSWORD`

If multiple projects, environments, or paths could match, ask a short clarifying question without revealing secret values.

## How To Use Retrieved Secrets

Inject secrets into the specific command that needs them, then clear them.

PowerShell:

```powershell
$env:CLOUDFLARE_API_TOKEN = infisical secrets get CLOUDFLARE_API_TOKEN --plain --env=prod --path=/cloudflare
# run the needed command
Remove-Item Env:\CLOUDFLARE_API_TOKEN
```

Bash:

```bash
CLOUDFLARE_API_TOKEN="$(infisical secrets get CLOUDFLARE_API_TOKEN --plain --env=prod --path=/cloudflare)" command-that-needs-it
```

For private keys, write to a temporary file with restrictive permissions only when a tool requires a file path, and delete the file immediately after use.

## User Guidance

When the user asks how an unfamiliar AI should get credentials, give them this minimal pattern:

```text
Give the AI only:
INFISICAL_API_URL
INFISICAL_CLIENT_ID
INFISICAL_CLIENT_SECRET

The AI should use Universal Auth to obtain a short-lived token, read only the needed secret from Infisical, and never print the secret.
```

Recommend one Machine Identity per agent or environment:

```text
ai-codex-local       read /ai/codex
ai-github-actions    read /ci/github
ai-server-ops        read /servers
```

For personal machines, user-level environment variables are acceptable if the Machine Identity has small read-only scope. For shared machines, CI, or untrusted scripts, prefer the platform's secret store and inject credentials only at runtime.

## Recovery Messages

If Infisical CLI is missing:

```text
I need a credential, but Infisical CLI is not installed. Install it first, then ask me to continue.
```

If environment variables are missing:

```text
Infisical is not configured for this shell. Set INFISICAL_API_URL, INFISICAL_CLIENT_ID, and INFISICAL_CLIENT_SECRET, then ask me to continue.
```

If login fails:

```text
Infisical login failed. Check that the Machine Identity client id/secret are correct, the identity is enabled, and it has access to the target project/environment/path.
```

If the secret is missing:

```text
I could not find the required secret in Infisical. Add `<SECRET_NAME>` under `<path>` for the target environment, then ask me to continue.
```
