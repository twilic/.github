# Dependabot configuration

Dependabot version updates are configured **per repository**. A `dependabot.yml` in this organization profile repository does **not** apply to other Twilic repositories.

## Adding Dependabot to a repository

1. Copy [`templates/dependabot.yml`](./templates/dependabot.yml) to `.github/dependabot.yml` in the target repository.
2. Remove ecosystems that do not apply (for example, omit `cargo` when the repository has no `Cargo.toml` at the root).
3. Merge the change on the default branch. Dependabot reads the file from the default branch.

## Recommended ecosystems

| Ecosystem        | When to include                                                             |
| ---------------- | --------------------------------------------------------------------------- |
| `github-actions` | Any repository with workflows under `.github/workflows/`                    |
| `cargo`          | Root `Cargo.toml` or a workspace you want updated from `/`                  |
| `npm`            | Root `package.json` (works with npm, pnpm, and Yarn lockfiles when present) |

Use a **weekly** schedule unless a repository has a reason to check more or less often.

## Merge policy for Dependabot pull requests

When merging bot pull requests, follow [MERGE_POLICY.md](./MERGE_POLICY.md): squash merge with a Conventional Commits-style subject and a body limited to `Signed-off-by` / `Co-authored-by` lines.

## Tracking

Cross-repository rollout is tracked in [issue #1](https://github.com/twilic/.github/issues/1).
