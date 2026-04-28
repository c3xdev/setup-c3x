# setup-c3x

GitHub Action for [C3X](https://c3x.dev) cloud cost estimation.

## Quick start

```yaml
name: Cost Estimation
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

That's it. Every PR gets a cost estimate comment. No API key, no secrets.

### Branded comments

Install the [C3X Cloud](https://github.com/apps/c3x-cloud) app on your repository. Comments will appear as **C3X Cloud** with the C3X logo instead of the generic github-actions bot. No configuration needed.

### Pin a specific version

```yaml
- uses: c3xdev/setup-c3x@v1
  with:
    path: .
    version: "1.0.1"
```

### Setup only (no auto-comment)

Omit the `path` input to install the CLI without running estimation:

```yaml
- uses: c3xdev/setup-c3x@v1
  id: c3x
- run: c3x estimate --path . --format json --out-file estimate.json
- run: c3x comment github --path estimate.json --github-token ${{ steps.c3x.outputs.token }} --repo ${{ github.repository }} --pull-request ${{ github.event.pull_request.number }}
```

## Inputs

| Input | Description | Default |
|---|---|---|
| `version` | C3X version to install | `latest` |
| `path` | Path to Terraform directory. When set, runs estimation and posts a PR comment. | |
| `show-all-projects` | Show all projects in the comment | `true` |

## Outputs

| Output | Description |
|---|---|
| `token` | GitHub token for `c3x comment`. Branded if C3X Cloud app is installed. |

## How it works

When `path` is set on a pull request:

1. Installs the C3X CLI (cached across runs)
2. Gets a branded token from C3X Cloud (if the app is installed)
3. Checks out the base branch and estimates costs
4. Checks out the PR branch and diffs against the base
5. Posts a cost comment on the PR

## License

Apache-2.0
