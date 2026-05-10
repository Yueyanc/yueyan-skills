---
name: rainyun-cli
description: Use rainyun-cli for working with RainYun cloud resources from the terminal, including checking account/auth status, listing and inspecting RainYun products, managing safe read-only operations, and using CLI help or official/public API references to discover exact command parameters. Use when the user asks to operate RainYun servers, cloud apps, object storage, CDN, domains, SSL certificates, firewall rules, renewals, or wants automation around RainYun resources.
---

# RainYun CLI

Use `rainyun-cli` when the task is about RainYun cloud resources and a terminal workflow is appropriate.

## First Checks

1. Check whether the CLI is available:

```bash
rainyun --help
```

2. If it is not installed, use the project source or npm package when available:

```bash
npm install -g rainyun-cli
```

If the package is not published yet, clone or open the source repository and run it from the project:

```bash
git clone https://github.com/Yueyanc/rainyun-cli.git
cd rainyun-cli
bun install
bun run dev -- --help
```

For a local production-style build:

```bash
npm run build
node dist/index.js --help
```

## How To Use

- Prefer `rainyun <group> --help` and `rainyun <group> <command> --help` to discover exact arguments and flags.
- Use `--json` for data that will be parsed, summarized, compared, or fed into another command.
- Use `rainyun doctor --json` before troubleshooting auth, config, or environment issues.
- Use `rainyun auth status` to check whether credentials are configured. Do not print or store full API keys in responses.
- Prefer read-only commands first when inspecting real resources.
- Treat create, delete, renew, buy, expand, power, firewall mutation, certificate replacement, and similar operations as sensitive.
- Do not run destructive, billable, or state-changing operations unless the user explicitly asks for that exact action and has identified the target resource.

## Common Command Areas

RainYun CLI groups commands by product area:

- `auth` and `config` for credentials and local settings
- `doctor`, `status`, and `raw` for diagnostics, platform status, and direct API calls
- `rcs` for cloud servers and firewall rules
- `rca` for cloud apps, projects, raindrops, IPs, SFTP, backups, and app templates
- `rgs`, `rvh`, and `rbm` for game servers, virtual hosts, and bare metal products
- `ros` for object storage instances and buckets
- `cdn` for RainYun CDN operations
- `domain` for domains and DNS
- `ssl` for certificate management
- `renew` for renewal workflows

When exact flags are uncertain, ask the CLI for help or consult the repository/docs instead of guessing.

## Safety Defaults

- Keep business data on stdout and explanations in the assistant response.
- Use `--json` for reliable parsing.
- Never echo full API keys, private keys, or certificate secrets.
- Avoid saving real user secrets unless the user explicitly requests local login/configuration.
- For real accounts, validate with safe read-only commands before attempting any write operation.
