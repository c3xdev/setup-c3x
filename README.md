# setup-c3x

> **This action now lives in [c3xdev/c3x](https://github.com/c3xdev/c3x#in-ci)**
> and on the GitHub Marketplace as
> [C3X Cost Estimation](https://github.com/marketplace/actions/c3x-cost-estimation).
> New workflows should use `c3xdev/c3x@v0`. Existing `c3xdev/setup-c3x@v1`
> workflows keep working unchanged: this repository is a thin wrapper around
> `c3xdev/c3x@v0` and gets every fix to it automatically.

GitHub Action for [c3x](https://c3x.dev): cloud cost estimation for
Terraform, OpenTofu and CloudFormation. It posts the cost change of every
pull request as a comment. Free and open source, no API key, no secrets.

## Quick start

```yaml
name: Cost estimation
on: [pull_request]
permissions:
  pull-requests: write
  contents: read
jobs:
  c3x:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: c3xdev/setup-c3x@v1
        with:
          path: .
```

The comment shows the monthly cost before and after the pull request and
the change per resource:

| Baseline | New | Change |
|---:|---:|---:|
| $1038.32/mo | $1318.64/mo | 🔺 +$280.32 |

## Gates

```yaml
      - uses: c3xdev/setup-c3x@v1
        with:
          path: .
          budget-delta: "50"   # fail if this PR adds more than $50/mo
          budget: "5000"       # fail if the monthly total exceeds $5,000
          strict: true         # fail if any line rests on an assumption
```

## Plan JSON

If your pipeline already runs `terraform plan`, give c3x the plan. It has
the real values of data sources and computed attributes:

```yaml
      - run: terraform plan -out=tfplan && terraform show -json tfplan > plan.json
      - uses: c3xdev/setup-c3x@v1
        with:
          path: plan.json
```

## Install only

Leave `path` empty to install the CLI and run it yourself:

```yaml
      - uses: c3xdev/setup-c3x@v1
      - run: c3x estimate --path . --format markdown
```

## Inputs

| Input | Description | Default |
|---|---|---|
| `version` | c3x version, e.g. `0.3.12` | `latest` |
| `path` | Directory, plan JSON or CloudFormation template. Empty installs only. | |
| `comment` | Post or update the PR comment | `true` |
| `budget` | Fail when the monthly total exceeds this (0 disables) | `0` |
| `budget-delta` | Fail when the PR increases the total by more than this (0 disables) | `0` |
| `strict` | Fail when any line rests on an assumption | `false` |
| `currency` | Display currency (USD, EUR, GBP, …) | USD |
| `untrusted` | Untrusted-input mode: `auto` (forks), `true`, `false` | `auto` |
| `branded-comments` | Comment as the [C3X Cloud](https://github.com/apps/c3x-cloud) app. Needs the app installed and `permissions: id-token: write`. | `false` |
| `show-all-projects` | Deprecated, ignored | |

The `token` output is kept for older workflows and returns the workflow's
`GITHUB_TOKEN`.

## License

Apache-2.0
