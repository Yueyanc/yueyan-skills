# CLI Engineering Checklist

Use this as a compact reference when auditing or designing a command-line tool.

## Human-first UX

- Actionable errors with a clear next step
- TTY prompts for missing required input
- Non-TTY fail-fast with exit code `2`
- Spinner for tasks >500ms
- Progress bar for longer known-volume tasks
- Semantic markers: `✔`, `✖`, `⚠`, `ℹ`
- Scannable table/list output for humans
- Opinionated onboarding for vague requests: propose a best-practice default instead of only listing choices
- Lightweight confirmation flow: ask for confirmation or one small tweak, not a long questionnaire

## Automation safety

- `stdout` reserved for data
- `stderr` reserved for logs, progress, warnings, and errors
- `--json` support for data-returning commands
- Valid JSON under redirection/piping
- `-` supported for stdin/stdout when applicable
- ANSI stripped in non-TTY output
- `NO_COLOR` honored

## Standards and environment

- XDG config/cache/data paths
- Atomic writes for config and state files
- Grouped short flags when appropriate
- Descriptive long flags
- Universal flags: `--help`, `--version`, `--verbose`, `--quiet`
- `completion` support for bash/zsh/fish
- Env vars override config when appropriate

## Reliability and security

- Idempotent re-runs where practical
- Pre-flight checks before destructive/high-cost work
- Graceful `SIGINT`/`SIGTERM` handling with cleanup
- Secret masking in verbose/debug logs
- Optional `doctor` command
- SemVer discipline
- `[DEPRECATED]` warnings with migration path

## Error categories

### User error
- bad syntax, invalid input, missing required args
- hint instead of stack trace
- usually exit code `2`

### System error
- remote service failure, permissions, dependency missing
- explain failure and remediation
- usually exit code `1`, `126`, or `127`

### Internal bug
- unexpected crash or invariant break
- include debug path and issue-report guidance
