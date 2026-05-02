---
name: pin-actions
description: Ensure GitHub Actions workflows use up-to-date and securely pinned action versions. Use this skill whenever the user asks to write, create, edit, review, or update a GitHub Actions workflow file (.github/workflows/*.yml or *.yaml), add a new action step, or audit existing workflows for outdated or unpinned versions. Also trigger when the user mentions actions like "actions/checkout", "uses:", or any GitHub Actions step configuration.
license: MIT
---

# pin-actions: GitHub Actions Version Pinning

Whenever you write or edit a GitHub Actions workflow, follow these rules to ensure every `uses:` step references the correct, up-to-date version.

## Pinning strategy

There are two categories of actions:

**Official GitHub actions** — owner is `actions` or `github` (e.g., `actions/checkout`, `actions/setup-node`, `github/codeql-action`):
- A floating major-version tag like `@v4` is acceptable.
- Still check that the major version is current — using `@v3` when `@v4` exists is a problem.

**Third-party actions** — every other owner (e.g., `docker/setup-buildx-action`, `aws-actions/configure-aws-credentials`, `google-github-actions/auth`):
- Pin to the full commit SHA.
- Add an inline comment with the corresponding tag. This is the format Renovate and Dependabot generate and expect when managing SHA-pinned actions — without the comment these tools cannot determine the current version and will skip the dependency.

Example of correct third-party pinning:
```yaml
- uses: docker/setup-buildx-action@f95db851eec2b9a1e0f0b694cf0cd01abb40e84a  # v3.0.0
```

## How to look up the latest version and SHA

Use the `gh` CLI. For any action `owner/repo`:

```bash
repo=owner/repo
tag=$(gh release view -R "$repo" --json tagName --jq .tagName)
sha=$(gh api "repos/$repo/commits/$tag" --jq .sha)
echo "tag=$tag  sha=$sha"
```

Run this for every action in the workflow before writing or modifying it.

If `gh release view` fails (no releases, only tags), fall back to:
```bash
tag=$(gh api "repos/$repo/tags" --jq '.[].name' | sort -V | tail -n 1)
sha=$(gh api "repos/$repo/commits/$tag" --jq .sha)
```

## Step-by-step workflow

1. **Collect all actions** — scan every `uses:` line in the workflow.
2. **For each action**, extract `owner/repo` and the currently referenced version.
3. **Look up latest** — run the commands above to get `$tag` and `$sha`.
4. **Decide the correct reference**:
   - Official (`actions/*` or `github/*`): use `@<major-tag>` (e.g., `@v4`). If the major version is behind, update it.
   - Third-party: use `@<full-sha>  # <tag>`.
5. **Apply** the correct reference in the workflow file.
6. **Report** what was changed and why, so the user can verify.

## When the right choice is unclear

Some organizations occupy a grey zone (e.g., `hashicorp/setup-terraform`, `slsa-framework/slsa-github-generator`). When you are not sure whether an owner qualifies as "official enough" for loose tag pinning, **ask the user** before writing the workflow:

> `hashicorp/setup-terraform` is a third-party action. Should I pin it to a SHA, or is the major-version tag acceptable for your use case?

## Example output

Before:
```yaml
- uses: actions/checkout@v2
- uses: docker/setup-buildx-action@v2
```

After running this skill:
```yaml
- uses: actions/checkout@v4
- uses: docker/setup-buildx-action@f95db851eec2b9a1e0f0b694cf0cd01abb40e84a  # v3.0.0
```
