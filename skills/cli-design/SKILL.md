---
name: cli-engineering
description: CLI design, implementation, review, and refactoring standards for human-friendly and automation-safe command-line tools. Use when creating a new CLI, improving an existing CLI's UX, auditing command behavior, writing command specs, or reviewing flags/output/errors/completions/progress handling for terminal tools.
---

# CLI Engineering

Use this skill to turn broad CLI UX principles into concrete implementation and review guidance.

## Core Rules

### Human-first interaction

- Return actionable errors, not naked error codes.
- Include a next step in every user-facing error.
- In TTY sessions, prompt for required missing input when that improves flow.
- In non-TTY environments, fail fast with exit code `2` for missing required arguments.

Error pattern:

```text
Error: <reason>. <how to fix it> (for example: run 'auth login' to refresh credentials)
```

### Machine-friendly automation

- Treat `stdout` as business data only.
- Send logs, spinners, warnings, and errors to `stderr`.
- Support `--json` for data-returning commands.
- Keep JSON output valid when piped or redirected.
- Support `-` as stdin/stdout where the command shape makes sense.

### Predictable runtime behavior

- Use XDG paths:
  - config: `XDG_CONFIG_HOME` or `~/.config/<app>`
  - cache: `XDG_CACHE_HOME` or `~/.cache/<app>`
  - data: `XDG_DATA_HOME` or `~/.local/share/<app>`
- Make commands idempotent where practical.
- Run pre-flight checks before destructive or expensive operations.
- Use atomic writes for config or persisted state updates: write to a temporary file first, then rename into place.
- Prefer environment variables over config files for CI/CD overrides.
- Mask secrets in verbose logs.
- Handle `SIGINT` and `SIGTERM` gracefully: stop spinners, clean up temporary files, and leave resumable state consistent.

### POSIX-friendly command shape

- Prefer `noun verb` command topology.
- Support grouped short flags like `-abc` when flags are boolean.
- Use descriptive long flags.
- Always implement `--help`, `--version`, `--verbose`, and `--quiet`.
- Provide a `completion` subcommand for bash, zsh, and fish.

### Lifecycle discipline

- Follow SemVer.
- Do not introduce breaking changes in patch or minor releases.
- Emit `[DEPRECATED]` warnings before removal.
- Keep deprecated behavior for at least two minor versions when possible.

## UX Requirements

### Feedback and progress

- Show a spinner for operations expected to exceed 500ms.
- Show a progress bar for tasks exceeding ~2s when total work is known.
- Use spinners only for unknown duration tasks.
- Label the current subtask clearly.

Semantic markers:

- `✔` success
- `✖` fatal error
- `⚠` warning
- `ℹ` info

### Scannability

- Use high-contrast colors.
- Avoid dark red or dark blue on dark terminals.
- Use tables for list data when rendering for humans.
- Auto-size columns to avoid jagged layouts.
- Use indentation for hierarchy.
- Hide noisy detail behind `--verbose`.

### Graceful degradation

- Honor `NO_COLOR`.
- Strip ANSI and control sequences when output is redirected or stdout is not a TTY.
- Keep non-interactive output plain, stable, and parseable.

## Error Handling Policy

Categorize failures before shaping the response.

### User error

- Cause: invalid flags, invalid syntax, missing input, unsupported values
- Response: concise explanation + hint
- Do not dump a stack trace
- Exit code: usually `2`

### System or external error

- Cause: network failure, remote 404/500, permission issue, unavailable dependency
- Response: explain what failed, suggest retry/remediation, include status/check URL when useful
- Exit code: usually `1`, `126`, or `127` depending on cause

### Internal bug

- Cause: unexpected exception or invariant violation
- Response: explain that the CLI crashed, include debug details if appropriate, and provide a GitHub issue link or bug-report path
- Include stack traces only for true internal failures

Whenever a file or dependency is missing, provide the exact command the user can run to fix it.

## Implementation Checklist

When designing or reviewing a CLI, check for all of the following:

- actionable errors with next steps
- TTY prompt vs non-TTY fail-fast behavior
- spinner/progress behavior for long tasks
- stdout/stderr separation
- `--json` support where commands return data
- exit code discipline: `0`, `1`, `2`, `126`, `127`
- XDG-compliant storage
- atomic writes for config and state files
- `-` stdin/stdout convention where relevant
- universal flags: `--help`, `--version`, `--verbose`, `--quiet`
- shell completion support
- `NO_COLOR` and non-TTY ANSI stripping
- idempotency for repeated runs
- pre-flight checks for risky operations
- graceful `SIGINT`/`SIGTERM` handling with cleanup
- secret masking in logs
- deprecation warnings and migration guidance
- optional `doctor` command for environment diagnostics

## Opinionated Onboarding

When the user's request is vague, do not only list options. Propose a best-practice default design, explain the reason briefly, and ask for lightweight confirmation or small adjustments.

### Decision guidance principles

- Default-driven: if the user is unsure, lead with a CLIG-style default.
- Show the tradeoff briefly: explain why the default is better for UX or automation.
- Keep confirmation lightweight: the user should be able to reply with "yes", "looks good", or one small tweak.
- Prefer progress through proposals over interrogations.

### Step 1: blueprint proposal

Turn the request into a recommended command topology immediately.

Example pattern:

```text
For this feature, I recommend a `noun verb` structure such as `<tool> <object> sync`. I'll treat `--json` support and XDG storage as defaults unless you want different behavior. Does that command shape feel intuitive to you?
```

Include by default when appropriate:

- `noun verb` command shape
- `--json` for data output
- XDG config/cache/data paths
- stdout/stderr separation
- universal flags

### Step 2: auth and UX presets

Offer a standard preset for credentials, persistence, and long-running task feedback.

Example pattern:

```text
For API credentials, I recommend environment variables first (for example `MY_APP_KEY`) with local config under `~/.config/<app>` when needed. For tasks longer than about 2 seconds, I'll add a progress bar automatically. Want to keep those defaults or change anything?
```

Default recommendations:

- env vars override config files
- config under XDG paths
- atomic writes for config/state
- spinner for >500ms operations
- progress bar for known longer tasks
- secret masking in verbose output

### Step 3: error recovery presets

Propose a default failure-handling model instead of waiting for the user to ask.

Example pattern:

```text
If the network drops, I recommend retrying up to 3 times for transient failures, then exiting with code `1` and printing an actionable recovery command. Does that fit your automation needs?
```

Default recommendations:

- categorize user/system/internal errors
- retry transient external failures conservatively
- fail fast on usage errors with exit code `2`
- provide exact remediation commands where possible
- handle `SIGINT` and `SIGTERM` with cleanup

## How to Use This Skill

### For new CLI design

Turn the user's feature request into:

1. command topology
2. flag design
3. stdout/stderr contract
4. error model
5. progress/reporting behavior
6. persistence/config layout
7. automation/CI behavior
8. completion and diagnostics plan

Prefer concrete specs over vague advice. When the request is underspecified, use Opinionated Onboarding: propose a default architecture, default auth/UX preset, and default error model instead of asking a long list of open-ended questions.

### For CLI review or refactor

Audit the existing CLI against these categories:

1. command naming and topology
2. flag consistency
3. structured output and piping safety
4. TTY vs non-TTY behavior
5. long-running task feedback
6. error clarity and remediation quality
7. config/cache/data placement
8. compatibility and deprecation handling

Call out mismatches and propose precise fixes.

## Suggested Output Format

When the user asks for a spec, review, or rewrite plan, prefer this structure:

### CLI spec

- commands and subcommands
- flags and arguments
- exit codes
- stdout contract
- stderr contract
- interactive behavior
- non-interactive behavior
- config/cache/data locations
- completion support
- diagnostics/deprecation strategy

### Review findings

- what already matches good CLI practice
- what breaks automation or UX
- severity of each issue
- recommended fix
- optional code examples

## References

Read `references/checklist.md` when you need a compact review sheet or want to mirror the original source material more directly.
