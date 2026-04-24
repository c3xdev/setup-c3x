# setup-c3x

GitHub Action to install the [C3X](https://c3x.dev) CLI for cloud cost estimation.

## Usage

```yaml
- uses: c3xdev/setup-c3x@v1
```

### Pin a specific version

```yaml
- uses: c3xdev/setup-c3x@v1
  with:
    version: "1.0.0"
```

### Branded PR comments with C3X Cloud

By default, PR comments appear as the generic "github-actions" bot. To show comments as **C3X Cloud** with the C3X logo:

1. Install the [C3X Cloud](https://github.com/apps/c3x-cloud) GitHub App on your repository
2. Add the App ID and private key as repository secrets (`C3X_APP_ID`, `C3X_APP_PRIVATE_KEY`)
3. Use the action with app credentials:

```yaml
name: Cost Estimation
on: [pull_request]
permissions:
  pull-requests: write
jobs:
  c3x:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: c3xdev/setup-c3x@v1
        id: c3x
        with:
          github-app-id: ${{ secrets.C3X_APP_ID }}
          github-app-private-key: ${{ secrets.C3X_APP_PRIVATE_KEY }}

      - name: Post cost estimate
        run: c3x comment github --path . --github-token ${{ steps.c3x.outputs.token }}
```

### Without app credentials (fallback)

```yaml
- uses: c3xdev/setup-c3x@v1
- run: c3x comment github --path . --github-token ${{ secrets.GITHUB_TOKEN }}
```

This works but comments appear as the default github-actions bot.

## Inputs

| Input | Description | Default |
|---|---|---|
| `version` | C3X version to install (e.g. `1.0.0`) | `latest` |
| `github-app-id` | C3X Cloud GitHub App ID (for branded comments) | |
| `github-app-private-key` | C3X Cloud GitHub App private key in PEM format | |

## Outputs

| Output | Description |
|---|---|
| `token` | GitHub token for `c3x comment`. App installation token if app credentials provided, empty otherwise. |

## Caching

The C3X binary is cached across workflow runs using `actions/cache`. Subsequent runs skip the download.

## License

Apache-2.0
