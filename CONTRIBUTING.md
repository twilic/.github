# Contributing

Thank you for improving shared Twilic organization files.

## Scope

This repository hosts organization-wide community assets such as GitHub profile configuration under `profile/`, and shared CI configuration including Semgrep static analysis under `semgrep/`.

## Commit Messages

We follow [Conventional Commits](https://www.conventionalcommits.org/).

Use this format:

```text
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

Common types include `feat`, `fix`, `docs`, `refactor`, `test`, `build`, `ci`, and `chore`.

Examples:

- `docs: update organization profile README`
- `chore: refresh community links`

Run `pnpm install` once to enable local commit-message checks via Husky. Pull requests are also checked in CI so every commit in the branch follows the same rules.

## Semgrep (organization-wide SAST)

Twilic repositories run [Semgrep](https://semgrep.dev/) on pull requests via a reusable workflow defined in this repository.

### Layout

| Path | Purpose |
| --- | --- |
| `.github/workflows/semgrep-reusable.yml` | Reusable workflow invoked by each repository |
| `semgrep/.semgrepignore` | Shared ignore patterns (build outputs, lock files, etc.) |
| `semgrep/profiles/*.txt` | Language-specific ruleset profiles (space-separated registry rulesets) |

Each application repository adds a thin caller workflow at `.github/workflows/semgrep.yml` that references `twilic/.github/.github/workflows/semgrep-reusable.yml@main` and sets the appropriate `profile` input.

### Available profiles

| Profile | Rulesets | Typical repositories |
| --- | --- | --- |
| `js` | javascript, typescript, nodejs, security-audit, owasp-top-ten | axios, cli, express, website, etc. |
| `js-rust` | javascript, typescript, rust, security-audit | twilic-js |
| `python` | python, security-audit | twilic-python |
| `go` | golang, security-audit | twilic-go |
| `rust` | rust, security-audit | twilic-rust |
| `java` | java, security-audit | twilic-java |
| `kotlin` | kotlin, java, security-audit | twilic-kotlin |
| `scala` | scala, security-audit | twilic-scala |
| `php` | php, security-audit | twilic-php |
| `ruby` | ruby, security-audit | twilic-ruby |
| `c` | c, security-audit | twilic-c |
| `cpp` | cpp, security-audit | twilic-cpp |
| `csharp` | csharp, security-audit | twilic-csharp |
| `swift` | swift, security-audit | twilic-swift |
| `terraform` | terraform, security-audit | cloudflare-infra |
| `default` | auto, security-audit | dart, elixir, lua, r, zig |

### Updating shared configuration

1. Edit files under `semgrep/` or `.github/workflows/semgrep-reusable.yml` in this repository.
2. Merge to `main`. Caller workflows reference `@main`, so changes apply on the next PR scan in each repository.
3. For breaking changes (new rules causing widespread failures), coordinate rollout or temporarily set `fail_on_findings: false` in the affected caller workflow.

To add a new profile, create `semgrep/profiles/<name>.txt` with space-separated rulesets and reference it from the caller workflow's `profile` input.

To exclude paths globally, add patterns to `semgrep/.semgrepignore`.

### Local execution

Install Semgrep locally ([installation guide](https://semgrep.dev/docs/getting-started/)), then from any repository root:

```bash
# Auto-detect languages and run security rules
semgrep scan --config auto --config p/security-audit

# Match a specific profile (example: python)
export SEMGREP_RULES="$(cat path/to/twilic/.github/semgrep/profiles/python.txt)"
semgrep scan --error
```

Use `--baseline-commit <sha>` to scan only changes since a commit (similar to CI on pull requests).

### Semgrep Cloud (optional)

When `SEMGREP_APP_TOKEN` is configured as a repository or organization secret, the reusable workflow automatically switches from `semgrep scan` (Community Edition) to `semgrep ci` (AppSec Platform) for diff-aware scanning, PR comments, and dashboard integration.

## Contribution Checklist

- Changes are limited to organization/community assets
- Commit messages follow Conventional Commits
- Semgrep profile or ignore changes are validated against at least one affected repository when possible
